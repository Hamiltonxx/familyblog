---
title: 微信支付API
date: 2020-03-06 13:55:06
tags: ["wechat","pay"]
index_img: http://cirraybucket.oss-cn-shanghai.aliyuncs.com/thumbnail/pay.jpg
banner_img: http://cirraybucket.oss-cn-shanghai.aliyuncs.com/blog/pay.jpg
---
## 前言
微信支付无处不在。构建一个网站如果没有支付，几乎不能称为一个完整的网站。要使用微信支付的api，就必须先申请注册商户平台。
[https://pay.weixin.qq.com/wiki/doc/apiv3/wxpay/pages/index.shtml](https://pay.weixin.qq.com/wiki/doc/apiv3/wxpay/pages/index.shtml)
看了一天，发现看的是V2版本的微信支付文档，其实现在有JSON版本的V3版API而非XML版本的V2版API。我真的不知道说啥好了。。这才是我开发该有的姿势。

## 认证
Request和Response都有SHA-256-RSA的Authorization.商户可以通过签名确定应答是来自微信支付的。
1. 获取商户证书和私钥
    下载的商户证书文件为1578763361_20200307_cert.zip,解压后，得到apiclient_cert.p12, apiclient_cert.pem, apiclient_key.pem(商户API私钥)三个密钥文件。(私钥也可以从p12证书中导出)
    **平台证书**是指由微信支付负责申请的，包含微信支付平台标识、公钥信息的证书。商户可以使用平台证书中的公钥进行验签。通过 https://api.mch.weixin.qq.com/v3/certificates 接口获取平台证书。
2. 设置API v3密钥
    微信支付在**回调通知**和**平台证书**下载接口中，对关键信息进行了AES-256-GCM加密。API v3密钥是加密时使用的对称密钥。
3. 使用商户私钥对请求签名
4. 通过API下载微信支付平台证书，并使用API v3密钥解密证书
5. 使用平台公钥对Response auth验证

## 签名生成
### 构造签名串
```
GET\n
/v3/certificates\n
1554208460\n
593BEC0C930BF1AFEB40B4A08C8FB242\n
\n
```
> 获取当前时间戳用 str(round(datetime.datetime.now().timestamp()))
> 长度为32的16进制随机数用 uuid.uuid4().hex.upper()

### 计算签名值
使用商户私钥对待签名串进行SHA256 with RSA签名，并对签名结果进行Base64编码得到签名值。
```
from Cryptodome.Hash import SHA256 
from Cryptodome.Signature import PKCS1_v1_5
from Cryptodome.PublicKey import RSA
import base64

message = "GET\n/v3/certificates\n1554208460\n593BEC0C930BF1AFEB40B4A08C8FB242\n\n" # to be signed
digest = SHA256.new(message.encode())
# Read shared key from file 
with open("private_key.pem","r") as myfile:
    private_key = RSA.importKey(myfile.read())
    # Load private key and sign message
    signer = PKCS1_v1_5.new(private_key)
    sig = signer.sign(digest)
    to_be_sent = base64.b64encode(sig)
```

### 设置HTTP头
```
Authorization: WECHATPAY2-SHA256-RSA2048 mchid="1900009191",nonce_str="593BEC0C930BF1AFEB40B4A08C8FB242",signature="uOVRnA4qG/MNnYzdQxJanN+zU+lTgIcnU9BxGw5dKjK+VdEUz2FeIoC+D5sB/LN+nGzX3hfZg6r5wT1pl2ZobmIc6p0ldN7J6yDgUzbX8Uk3sD4a4eZVPTBvqNDoUqcYMlZ9uuDdCvNv4TM3c1WzsXUrExwVkI1XO5jCNbgDJ25nkT/c1gIFvqoogl7MdSFGc4W4xZsqCItnqbypR3RuGIlR9h9vlRsy7zJR9PBI83X8alLDIfR1ukt1P7tMnmogZ0cuDY8cZsd8ZlCgLadmvej58SLsIkVxFJ8XyUgx9FmutKSYTmYtWBZ0+tNvfGmbXU7cob8H/4nLBiCwIUFluw==",timestamp="1554208460",serial_no="635FD912DA74462BAD30DB188A5B4A900BCAF711"
```

## 签名验证
### 获取平台证书
微信支付API v3使用微信支付的平台私钥进行应答签名。相应的，我们也应使用微信支付平台证书中的公钥验签。目前平台证书只提供API进行下载。**再次提醒，应答和回调的签名验证使用的是微信支付平台证书，不是商户API证书。**

### 检查平台证书序列号
验证签名前，请检查序列号是否跟当前所持有的微信支付平台证书的序列号一致。如果不一致，请重新获取证书。

### 构造验签名串
首先获取HTTP Header里的Wechatpay-Timestamp,Wechatpay-Nonce及Response Body.
然后按照以下规则构造应答的验签名串。
```
应答时间戳\n
应答随机串\n
应答报文主体\n
```
验签名串如下
```
1554209980
c5ac7061fccab6bf3e254dcf98995b8c
{"data":[{"serial_no":"5157F09EFDC096DE15EBE81A47057A7232F1B8E1","effective_time":"2018-03-26T11:39:50+08:00","expire_time":"2023-03-25T11:39:50+08:00","encrypt_certificate":{"algorithm":"AEAD_AES_256_GCM","nonce":"4de73afd28b6","associated_data":"certificate","ciphertext":"..."}}]}
```

### 获取应答签名
```
Wechatpay-Signature: CtcbzwtQjN8rnOXItEBJ5aQFSnIXESeV28Pr2YEmf9wsDQ8Nx25ytW6FXBCAFdrr0mgqngX3AD9gNzjnNHzSGTPBSsaEkIfhPF4b8YRRTpny88tNLyprXA0GU5ID3DkZHpjFkX1hAp/D0fva2GKjGRLtvYbtUk/OLYqFuzbjt3yOBzJSKQqJsvbXILffgAmX4pKql+Ln+6UPvSCeKwznvtPaEx+9nMBmKu7Wpbqm/+2ksc0XwjD+xlvlECkCxfD/OJ4gN3IurE0fpjxIkvHDiinQmk51BI7zQD8k1znU7r/spPqB+vZjc5ep6DC5wZUpFu5vJ8MoNKjCu8wnzyCFdA==
```
对Wechatpay-Signature的字段值使用Base64进行解码，得到应答签名。

### 验证签名
使用微信支付平台公钥对验签名串和应答签名进行验证。
假设我们已经获取了平台证书并保存为 1900009191_wxp_cert.pem
```
from Cryptodome.Hash import SHA256 
from Cryptodome.Signature import PKCS1_v1_5
from Cryptodome.PublicKey import RSA 
import base64

# message = '1554209980\nc5ac7061fccab6bf3e254dcf98995b8c\n{"data":[{"serial_no":"5157F09EFDC096DE15EBE81A47057A7232F1B8E1","effective_time":"2018-03-26T11:39:50+08:00","expire_time":"2023-03-25T11:39:50+08:00","encrypt_certificate":{"algorithm":"AEAD_AES_256_GCM","nonce":"4de73afd28b6","associated_data":"certificate","ciphertext":"..."}}]}\n'
message = '1554209980' + '\n' + 'c5ac7061fccab6bf3e254dcf98995b8c' + '\n' + '{"data":[{"serial_no":"5157F09EFDC096DE15EBE81A47057A7232F1B8E1","effective_time":"2018-03-26T11:39:50+08:00","expire_time":"2023-03-25T11:39:50+08:00","encrypt_certificate":{"algorithm":"AEAD_AES_256_GCM","nonce":"4de73afd28b6","associated_data":"certificate","ciphertext":"..."}}]}' + '\n'
digest = SHA256.new(message.encode())
with open("1900009191_wxp_pub.pem","r") as myfile:
    public_key = RSA.importKey(myfile.read())

verifier = PKCS1_v1_5.new(public_key)
wxsig = "CtcbzwtQjN8rnOXItEBJ5aQFSnIXESeV28Pr2YEmf9wsDQ8Nx25ytW6FXBCAFdrr0mgqngX3AD9gNzjnNHzSGTPBSsaEkIfhPF4b8YRRTpny88tNLyprXA0GU5ID3DkZHpjFkX1hAp/D0fva2GKjGRLtvYbtUk/OLYqFuzbjt3yOBzJSKQqJsvbXILffgAmX4pKql+Ln+6UPvSCeKwznvtPaEx+9nMBmKu7Wpbqm/+2ksc0XwjD+xlvlECkCxfD/OJ4gN3IurE0fpjxIkvHDiinQmk51BI7zQD8k1znU7r/spPqB+vZjc5ep6DC5wZUpFu5vJ8MoNKjCu8wnzyCFdA=="
sig = base64.b64decode(wxsig)
print(sig)
result = verifier.verify(digest, sig)
print(result)
```

## 证书和回调报文解密
微信支付在回调通知和平台证书下载接口中，对关键信息进行了AES-256-GCM加密。
证书和回调报文使用的加密密钥为APIv3密钥。

## Native支付业务流程
```
1. 商户后台系统根据用户选购的商品生成订单。
2. 用户确认支付后调用微信支付【统一下单API】生成预支付交易；
3. 微信支付系统收到请求后生成预支付交易单，并返回交易会话的二维码链接code_url。
4. 商户后台系统根据返回的code_url生成二维码。
5. 用户打开微信“扫一扫”扫描二维码，微信客户端将扫码内容发送到微信支付系统。
6. 微信支付系统收到客户端请求，验证链接有效性后发起用户支付，要求用户授权。
7. 用户在微信客户端输入密码，确认支付后，微信客户端提交授权。
8. 微信支付系统根据用户授权完成支付交易。
9. 微信支付系统完成支付交易后给微信客户端返回交易结果，并将交易结果通过短信、微信消息提示用户。微信客户端展示支付交易结果页面。
10. 微信支付系统通过发送异步消息通知商户后台系统支付结果。商户后台系统需回复接收情况，通知微信后台系统不再发送该单的支付通知。
11. 未收到支付通知的情况，商户后台系统调用【查询订单API】。
12. 商户确认订单已支付后给用户发货。
```
### 生成二维码规则
对应链接格式：weixin：//wxpay/bizpayurl?sr=XXXXX。请商户调用第三方库将code_url生成二维码图片。该模式链接较短，生成的二维码打印到结账小票上的识别率较高。
例如，将weixin：//wxpay/s/An4baqw生成二维码
![https://pay.weixin.qq.com/wiki/doc/api/img/chapter6_5_2.png](https://pay.weixin.qq.com/wiki/doc/api/img/chapter6_5_2.png)

## 接入
[https://pay.weixin.qq.com/wiki/doc/apiv3/wxpay/pages/Overview.shtml](https://pay.weixin.qq.com/wiki/doc/apiv3/wxpay/pages/Overview.shtml)

## 微信支付分
微信支付分是对个人的身份特质、支付行为、使用历史等情况的综合计算分值，旨在为用户提供更简单便捷的生活方式。微信用户可以在具体应用场景中，开通微信支付分。开通后，用户可以在【微信—>钱包—>支付分】中查看分数和使用记录。（即需在应用场景中使用过一次，钱包才会出现支付分入口）

