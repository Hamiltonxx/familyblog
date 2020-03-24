---
title: Trojan科学上网
date: 2020-02-15 19:10:55
tags: ["network"]
index_img: http://cirraybucket.oss-cn-shanghai.aliyuncs.com/thumbnail/hub.jpg
banner_img: http://cirraybucket.oss-cn-shanghai.aliyuncs.com/blog/hub.jpg
---
## 前言
GFW不息，奋斗不止。从最初的Shadowsocks,到V2Ray,再到Trojan，演绎了一批追求自由曙光的人与GFW的斗智斗勇的历程。一提科学上网，大多数人首先会想到VPN，其实这两者不是一回事。简单来说，一个代表协议，一个代表软件。

## Trojan简介
Trojan官网文档给了一个很简短的定义,参照[https://trojan-gfw.github.io/trojan](https://trojan-gfw.github.io/trojan/),Trojan是一个能绕过GFW而不被识别的机制。在穿透GFW时，人们认为强加密和随机混淆可能会欺骗GFW的过滤机制。然而，Trojan实现了这个思路的反面：它模仿了互联网上最常见的HTTPS协议，以诱骗GFW认为它就是HTTPS，从而不被识别。网络上通常把它理解为v2ray+ws+TLS的lite版，但安全和性能方面一般比v2ray+ws+TLS更好。
![](/images/Trojan工作原理.svg)

## 步骤一 购买VPS
一般选择Google Cloud或Bangwagon.网上教程很多，就略过。

## 步骤二 申请(购买)域名
可以从[namecheap](https://www.namecheap.com/)等网站购买付费域名，或者从[freenom](https://www.freenom.com/)申请免费域名，然后解析域名到购买的VPS上，同样略过。

## 步骤三 一键脚本安装
复制以下命令在VPS中执行，选择安装trojan,然后输入解析到VPS的域名并回车(不要带http://,只输入域名，例如cirray.com或xxx.cirray.com)
```
curl -O https://raw.githubusercontent.com/atrandys/trojan/master/trojan_mult.sh && chmod +x trojan_mult.sh && ./trojan_mult.sh
```
另外，建议安装bbr,用原版bbr加速即可
```
cd /usr/src && wget -N --no-check-certificate "https://raw.githubusercontent.com/chiakge/Linux-NetSpeed/master/tcp.sh" && chmod +x tcp.sh && ./tcp.sh
```
最后请注意这段
```
Trojan已安装完成，请使用以下链接下载trojan客户端，此客户端已配置好所有参数
1、复制下面的链接，在浏览器打开，下载客户端，注意此下载链接将在1个小时后失效
http://gfw.cirray.com/68b329da9893e340/trojan-cli.zip
2、将下载的压缩包解压，打开文件夹，打开start.bat即打开并运行Trojan客户端
3、打开stop.bat即关闭Trojan客户端
4、Trojan客户端需要搭配浏览器插件使用，例如switchyomega等
```