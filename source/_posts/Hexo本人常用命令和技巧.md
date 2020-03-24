---
title: Hexo本人常用命令和技巧
tags:
  - blog
  - hexo
index_img: 'http://cirraybucket.oss-cn-shanghai.aliyuncs.com/thumbnail/blog.png'
banner_img: 'http://cirraybucket.oss-cn-shanghai.aliyuncs.com/blog/astronomy.jpg'
date: 2020-02-24 16:21:59
---


## 前言
标题里为啥要加上"本人"，因为[Hexo官方文档](https://hexo.io/docs/)肯定要比我介绍的全面和深入。本篇也仅仅是介绍本人，作为一个上手用了没多久的普通Blogger，用了哪些命令和技巧来搭建博客。
最初，我用wordpress写一些算法博客，但那不是我们这种小码农该有的姿势。后来慢慢了解SSG(Static Site Generator),开始尝试Github Page推荐的Jekyll,不使用Ruby也不想研究它，放弃。到现在觉得Hexo挺香。当然如果熟悉Go的也可以去体验Hugo.

## 动手 
Hexo是Node系列，当然
```
$ npm install -g hexo-cli
```
创建和初始化博客
```
$ hexo init myblog
$ cd myblog
$ npm install
```
然后得到博客的目录如下
```
.
├── _config.yml
├── package.json
├── scaffolds
├── source
|   ├── _drafts
|   └── _posts
└── themes
```
_config.yml 站点配置文件，配置标题、作者、样式主题等
package.json 依赖库
scaffolds 新博或新页的脚手架(模版)
source 站点内容
themes 样式目录，本站采用Fluid

## 常用命令
```
$ hexo new [layout] <title>
$ hexo new page --path about/me "About me" #创建路径为about/me.md,标题为About me的页面
$ hexo generate # Generates static files. 
    -d,--deploy | -w,--watch | -f,--force
$ hexo publish [layout] <filename> # Publishes a draft
$ hexo server 
$ hexo deploy [-g,--generate] # Generate before deployment
$ hexo clean # Clean the cache file(db.json) and generated files(public)
$ hexo list <type> # Lists all routes
$ hexo --draft # Display draft posts
```

## 写博文
```
$ hexo new [layout] <title>
```
Hexo默认的layout是post,这个可以在_config.yml里的default_layout里设置。Hexo提供三个layout:post,page和draft。Hexo默认博文标题即文件名。
如果要发布draft,即把草稿文档移到source/\_posts文件夹
```
$ hexo publish [layout] <title>
```

## 压缩图片
> Mac下安装ImageOptim及使用自带的sips
```
open -a ImageOptim *
sips -g pixelHeight -g pixelWidth coin.jpg
sips -z 160 218 kimono.jpg
sips -Z 300 traffic.jpg
ossutil cp traffic.jpg oss://cirraybucket/thumbnail/
```
index页面的图片只要218x160,所以按比例缩小的话，width缩到300能避免height缩到小于160.最后一句为笔者上传阿里云OSS(可以看成私人图床)的命令。

## 生成静态文件
```
$ hexo generate  # --watch  --deploy
```

## 发布
> alias deployblog="hexo generate; scp -r public/* root@cirray.cn:/path/to/blog/"
因为有本地generate和拷贝到服务器两个步骤，我偷懒在本地bash里加一个alias
```
$ deployblog
```
等等。 每次都传一堆上去，浪费网络带宽。我还是用git吧。。




