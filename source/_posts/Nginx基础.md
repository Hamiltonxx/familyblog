---
title: Nginx基础
tags:
  - nginx
  - load balancing
  - high concurrency
index_img: 'https://cirraybucket.oss-cn-shanghai.aliyuncs.com/thumbnail/traffic.jpg'
banner_img: 'https://cirraybucket.oss-cn-shanghai.aliyuncs.com/blog/traffic.jpg'
date: 2020-02-28 03:08:08
---

## 安装
以Ubuntu Server为例，创建文件/etc/apt/sources.list.d/nginx.list,加入如下两行
```
deb http://nginx.org/packages/mainline/ubuntu/ bionic nginx
deb-src http://nginx.org/packages/mainline/ubuntu/ bionic nginx
```
然后运行以下命令
```
wget http://nginx.org/keys/nginx_signing.key
apt-key add nginx_signing.key
apt-get update
apt-get install -y nginx
/etc/init.d/nginx start
```
接下来，验证安装
```
$ nginx -v
nginx version: nginx/1.15.3

$ ps -ef | grep nginx
root 1738 1 0 19:54 ? 00:00:00 nginx: master process 
nginx 1739 1738 0 19:54 ? 00:00:00 nginx: worker process

$ curl localhost
NGINX Welcome HTML
```

## 配置静态站点
首先，看一眼/etc/nginx/nginx.conf，这是NGINX服务配置的入口，也是全局配置的所在，比如worker process,加载动态模块，引用其他配置文件等。它包含一个最高层级的http块。
创建我们自己的配置文件
```
$ sudo vim /etc/nginx/sites-available/blog.cirray.cn

server {
  listen 80;
  listen [::]:80;

  root /var/www/blog;
  index index.html index.htm;
  server_name blog.cirray.cn www.blog.cirray.cn;
}
```
前提是我们已经在域名供应商处陪好了(二级)域名，然后
```
$ sudo ln -s /etc/nginx/sites-available/blog.cirray.cn /etc/nginx/sites-enabled/
```

## 静态资源目录
把静态资源目录(blog工程文件夹)放在 /var/www/下。


## 优雅重载配置
```
$ nginx -s reload
```
通过向主进程(master process)发送reload信号,重载配置却不关闭服务。
在严格要求正常运行(高可用)，弹性环境里，有时你需要改变负载均衡(load-balancing)配置。此命令可以让你保持负载均衡器(load balancer)在线。

最后curl http://blog.cirray.cn 或在浏览器输入此地址测试即可。