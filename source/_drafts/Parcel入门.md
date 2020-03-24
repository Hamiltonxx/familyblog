---
title: Parcel入门
tags:
---
## 前言
> 不求深刻，只求简单!
写几个微信H5页面，要啥三大前端框架呢？我想自己用Javascript写个router,可能data-binding都用不到，fetch足够了。build工具在webpack之后又出现了parcel,听说它很简单。于是就来学习一下。

## 走你
Parcel是一个Web应用程序捆绑器。它利用多核处理来提供及其快速的性能，且只需零配置。
Parcel可以用任意类型的文件作为入口，但是一个HTML或Javascript文件通常是一个好的开始。
```
npm init -y
code index.html

<html>
<body>
    <script src="./index.js"></script>
</body>
</html>
```
