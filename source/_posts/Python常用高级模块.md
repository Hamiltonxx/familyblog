---
title: Python常用高级模块
date: 2020-03-09 09:54:12
tags: "python"
index_img: http://cirraybucket.oss-cn-shanghai.aliyuncs.com/thumbnail/workbench.jpg
banner_img: http://cirraybucket.oss-cn-shanghai.aliyuncs.com/blog/workbench.jpg
---

## 前言
一般，Python高级模块肯定不会有如初级模块使用那么频繁，而且练算法题也是练不到的。但在实际做项目过程中却会经常用到。比如，最近在对接微信支付接口就需要掌握好几个。索性统一记录，方便以后查阅。

## string和bytes的互换
```
# convert string to bytes
data = ""           #string
data = "".encode()  #bytes
data = b""          #bytes
# convert bytes to string
data = b""          #bytes
data = b"".decode() #string
data = str(b"")     #string
```

## Base64/数据编码
base64模块提供了将二进制数据编码成可打印的ASCII字符的函数，以及将这些编码串解码回二进制串的函数。即所谓的encoding和decoding过程。
> 注意:encoding(编码)不是encryption(加密).

虽然用Base64算法可以把用户名、密码等编码成肉眼不可读，但解码它们就同编码它们一样容易。安全不是编码的目的。相反，编码的目的是把(可能)HTTP不兼容的字符编码成HTTP兼容，以便发送请求(Request)或发Email.
```
>>> import base64
>>> encoded = base64.b64encode(b'data to be encoded')
>>> encoded
b'ZGF0YSB0byBiZSBlbmNvZGVk'
>>> data = base64.b64decode(encoded)
>>> data
b'data to be encoded'
```
```
import base64
str_to_be_sent = "abc123!?$*&()'-=@~"
encoded = str_to_be_sent.encode() # str to bytes
base64encoded = base64.b64encode(encoded) # bytes to base64encoded
print(base64encoded,"is ready to be sent!")
base64decoded = base64.b64decode(base64encoded) # decode the base64encoded
print(base64decoded)
original_str = base64decoded.decode() # bytes to str
print(original_str)
```

## 随机数
> n位由0~9+a~z+A~Z组成的随机串
```
import random
import string
def randomString(n):
    return ''.join(random.choices(string.ascii_letters+string.digits, k=n))
```
> 16进制串
```
import secrets
import uuid 
print(secrets.token_hex(16))
# 32位16进制串，推荐
print(uuid.uuid4().hex)
print(uuid.uuid4().hex.upper())
```

## 时间
> 取当前时间戳timestamp
```
import time 
import datetime 

print(time.time())
print(datetime.datetime.now().timestamp())
print(round(datetime.datetime.now().timestamp()))
```