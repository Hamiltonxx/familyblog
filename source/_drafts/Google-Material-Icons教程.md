---
title: Google Material Icons教程
tags:
---
# 简介
Material design icons简单、友好并且很快。如果是WEB工程，仅仅需要把这行加入到HTML中。
```
<link href="https://fonts.googleapis.com/icon?family=Material+Icons" rel="stylesheet">
```
使用很简单
```
<i class="material-icons">face</i>
```
** 调整样式 **
```
/* icon大小,默认是24px */
.material-icons.md-18 { font-size: 18px;}
.material-icons.md-36 { font-size: 36px;}

/* 白背景上显示黑色 */
.material-icons.md-dark { color: rgba(0,0,0,0.54); }
.material-icons.md-dark.md-inactive { color: rgba(0,0,0,0.26); }
/* 黑背景上显示白色 */
.material-icons.md-light { color: rgba(255,255,255,1); }
.material-icons.md-light.md-inactive { color: rgba(255,255,255,0.3); }
```