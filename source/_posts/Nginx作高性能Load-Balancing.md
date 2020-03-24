---
title: Nginx作高性能Load Balancing
tags:
  - nginx
  - load balancing
index_img: 'https://cirraybucket.oss-cn-shanghai.aliyuncs.com/thumbnail/traffic.jpg'
banner_img: 'https://cirraybucket.oss-cn-shanghai.aliyuncs.com/blog/traffic.jpg'
date: 2020-02-28 15:39:44
---

## 简介
当今互联网用户体验要求服务具备高性能和高可用，为达到这目的，同一系统的很多copy会在各地运行，负载会分布在它们上面。当负载过载时，新的一份系统copy可以接着上线。这个架构称作水平扩展。不管这个架构只有两台服务器(仅出于高可用性考虑)，还是有遍布全球的几千台服务器，都需要load balancing来保证动态提供服务。NGINX可以从HTTP,TCP和UDP load balancing等多个方面来填充这个需求。
很多现代web架构采用了状态无关层，即把状态存储在共享内存或数据库中。但也并非所有都是如此，Session状态在一些交互式应用中非常重要，由于像数据量很大时网络开销特别大等种种原因，这些状态只会存储在应用服务器本地当中。当状态存储在某一应用服务器中时，为保证用户体验，后续请求只能继续发送到该服务器。另一种情况是，在session完成之前，这些servers资源不能被release。状态相关的扩展需要更智能的负载均衡器。可以从跟踪cookies或routing这些角度来解决这个问题。本文主要讲解session持久化来处理load balancing.
确保NGINX应用正常也很重要。由于种种原因，比如网络连通，server failure或application failure,代理和负载均衡器必须足够聪明，可以检测到服务器挂了，从而阻止流量再去那台服务器。不然客户端就会等待，直到timeout。为了在服务器挂时避免服务下降，一种方法是让代理检测upstream server。NGINX提供主动检测和被动检测。主动检测是每隔一定时间发送连接请求给服务器，被动检测则等待客户端请求，查看server的response。

## HTTP Load Balancing
> 分发负载给两台或更多的HTTP Server.
```
upstream backend {
  server 10.10.12.45:80        weight=1;
  server app.example.com:80    weight=2;
}
server {
  location / {
    proxy_pass http://backend;
  }
}
```
upstream模块为HTTP控制load balancing.

## TCP Load Balancing
> 分发负载给两台或更多的TCP Server
```
stream {
  upstream mysql_read {
    server read1.example.com:3306  weight=5;
    server read2.example.com:3306;
    server 10.10.12.34:3306        backup;
  }
  server {
    listen 3306;
    proxy_pass mysql_read;
  }
}
```

## 动手配置简易Load Balancing
你有一台主NGINX Web Server,想要分发负载给其他两台NGINX Server.这样能保证不管多大流量，总有一台或更多Server来处理负载。显然，每台NGINX Web Server的初始化要完全一样。假设你有三台Ubuntu Server
* Server 1 - 192.168.1.232
* Server 2 - 192.168.1.233
* Server 3 - 192.168.1.234
都已经跑着NGINX。

### 初始化
在每台Server的/var/www/html目录下，创建新的index.html,内容如下
```
<h1> NGINX SERVER Y </h1>
```
其中Y为1,2,3，对应Server编号

### 配置
在SERVER 1上创建一个新的配置文件.
> sudo vim /etc/nginx/conf.d/load-balancer.conf
```
upstream backend {
  server 192.168.1.232;
  server 192.168.1.233;
  server 192.168.1.234;
}
# 下面server接收所有请求，然后提交给upstream.
server {
  listen 80;

  location / {
    proxy_pass http://backend;
  }
}
```
保存该文件后，需要移除default配置文件,然后重启服务
> sudo rm /etc/nginx/sites-enabled/default
> sudo systemctl restart nginx

### 测试
浏览器中输入SERVER 1的IP地址(192.168.1.232)，Load balancing会通过load-balancer.conf里配置的server进行轮询。首先SERVER 1,然后SERVER 2,接着再SERVER 3,以此进行下去。

### 警告
默认的Loading Balancing系统是轮询(round robin),这会导致session持久化的问题。如果因为先前的session，站点/应用需要把用户导向同一台服务器，必须使用IP hashing.用了IP hashing后，用户会始终导向同一台服务器(除非这台服务器挂了)。
接下来，配置IP hashing
```
upstream backend {
  ip_hash;
  server 192.168.1.232;
  server 192.168.1.233;
  server 192.168.1.234;
}
server {
  listen 80;

  location / {
    proxy_pass http://backend;
  }
}
```
> sudo systemctl restart nginx
现在，如果在浏览器里输入SERVER 1 IP,再刷新，你会导入相同的页面(Server).

## 接下来
我们已经用NGINX配置了简易Load Balancing,下次我们继续讲怎样定义server权重，怎样Load Balancing HTTPS.