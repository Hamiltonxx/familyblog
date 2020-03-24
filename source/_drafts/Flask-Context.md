---
title: Flask Context
tags:
---
## 前言
在给请求(request)做一个计数器的时候，发现我的请求处理函数里，不能简单拿到外部变量。于是，就来学习Request Context和Application Context。

## Request Context
Request Context保留了在请求时request-level的数据。而Application Context则保留了Request无关的application-level的数据。当request context被push了，对应的application context也会被push.

### Request Context的目的
当Flask应用处理一个请求，它会基于从WSGI服务器收到的environment创建一个Request对象。因为一个worker(线程、进程、协程)一次只能处理一个请求，在request期间，request数据可以被当成global. 为此Flask使用context local术语。
当处理请求时，Flask自动push一个request context给view函数、error处理器及其他处理request的函数，这些都可以访问request代理，而这又指向了当前请求的request对象。

### Request Context生命周期
很重要，暂略

## Application Context
Application Context保留了在请求、CLI Command和其他活动时application-level的数据。每个函数里，current_app和g代理可以被使用。

### Application Context的目的

