---
title: Python加密PyCryptodome
date: 2020-03-07 18:30:38
tags: "python"
index_img: http://cirraybucket.oss-cn-shanghai.aliyuncs.com/thumbnail/keyboard.jpg
banner_img: http://cirraybucket.oss-cn-shanghai.aliyuncs.com/blog/keyboard.jpg
---

## 前言
在微信支付API v3文档的阅读过程中，看到它用到对称加密AES GCM. 心血来潮就来研究一下Python的AES是如何实现和使用的。
PyCryptodome是一个独立的Python包，封装了低层级加密原语。
```
pip install pycryptodomex
```
我们不再使用老的PyCrypto.所有的Modules都安装在Cryptodome包下。可以通过下面这个来验证安装正确。
```
python -m Cryptodome.SelfTest
```

## 用AES加密数据
生成一个新的AES128 key,加密一段数据到文件里。我们使用EAX模式因为它允许接收者检测未经授权的改动。(同样，我们也能使用其他授权加密模式如GCM,CCM或SIV)
```
from Cryptodome.Cipher import AES
from Cryptodome.Random import get_random_bytes

key = get_random_bytes(16)
print(key)
cipher = AES.new(key,AES.MODE_EAX)
ciphertext, tag = cipher.encrypt_and_digest(data)

file_out = open("encrypted.bin", "wb")
[file_out.write(x) for x in (cipher.nonce, tag, ciphertext)]
```
另一方面，接收者可以安全地把这段数据加载回来(如果他们知道这个key)
```
from Cryptodome.Cipher import AES

file_in = open("encrypted.bin","rb")
nonce, tag, ciphertext = [file_in.read(x) for x in (16,16,-1)]
#let's assume that the key is somehow available again
cipher = AES.new(key,AES.MODE_EAX,nonce) 
data = cipher.decrypt_and_verify(ciphertext,tag) 
```

## 生成RSA key
下面代码生成一个新RSA key对并存入文件，它由密码保护。最后打印RSA public key
```
from Cryptodome.PublicKey import RSA

secret_code = "Unguessable"
key = RSA.generate(2048)
encrypted_key = key.export_key(passphrase=secret_code, pkcs=8, protection="scryptAndAES128-CBC")
f = open("rsa_key.bin","wb")
f.write(encrypted_key)
print(key.publickey().export_key())
```
下面代码读回private RSA key,并再次打印它的public key
```
from Cryptodome.PublicKey import RSA

secret_code = "Unguessable"
encoded_key = open("rsa_key.bin","rb").read()
key = RSA.import_key(encoded_key, passphrase=secret_code)
print(key.publickey().export_key())
```

## 生成 public key 和 private key
下面代码生成public key存储在receiver.pem,以及private key存储在private.pem.
```
from Cryptodome.PublicKey import RSA 

key = RSA.generate(2048)
private_key = key.export_key()
f = open("private.pem","wb")
f.write(private_key)

public_key = key.publickey().export_key()
f = open("receiver.pem","wb")
f.write(public_key)
```

## 作业
> 用Python实现微信支付里的计算签名值
所在的微信支付api文档地址为: [https://wechatpay-api.gitbook.io/wechatpay-api-v3/qian-ming-zhi-nan-1/qian-ming-sheng-cheng](https://wechatpay-api.gitbook.io/wechatpay-api-v3/qian-ming-zhi-nan-1/qian-ming-sheng-cheng)
```
## Sign and Verify
from Cryptodome.Hash import SHA256 
from Cryptodome.Signature import PKCS1_v1_5
from Cryptodome.PublicKey import RSA 

message = "GET\n/v3/certificates\n1554208460\n593BEC0C930BF1AFEB40B4A08C8FB242\n\n" # to be signed
# digest = SHA256.new()
# digest.update(message)
digest = SHA256.new(message.encode())
# Read shared key from file
private_key = False 
with open("private_key.pem","r") as myfile:
    private_key = RSA.importKey(myfile.read())
# Load private key and sign message
signer = PKCS1_v1_5.new(private_key)
sig = signer.sign(digest)

# Base64
import base64
to_be_sent = base64.b64encode(sig)
print("答案",to_be_sent.decode()) 

# Load public key and verify message
verifier = PKCS1_v1_5.new(private_key.publickey())
verified = verifier.verify(digest, sig)
print(verified)
assert verified, 'Signature verification failed'
print('Successfully verified message') 
```