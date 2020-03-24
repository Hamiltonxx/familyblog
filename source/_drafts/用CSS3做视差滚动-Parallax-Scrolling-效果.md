---
title: 用CSS3做视差滚动(Parallax Scrolling)效果
tags:
---
# 前言
最近在做挪瓦咖啡的官网。我不想做的太俗气(just another website),我想增加一点Parallax Scrolling效果。
Parallax Scrolling是一个web趋势，原理是背景内容(通常是背景图image)和前景内容以不同的速度滚动。

# 动手
给container增加一个固定高度的图片，然后background-attachment: fixed

# 上代码
```
<style>
    body,html{margin:0;padding:0;}
    .wrapper {
        height:100vh;
    }
    .section {
        height: 100vh;
        display: flex;
        justify-content: center;
        align-items: center;
        color: white;
        text-shadow: 0 0 5px #000;
    }
    .parallax {
        background-position: center;
        background-repeat: no-repeat;
        background-size: cover;
        background-attachment: fixed;
    }
    .bg1 {
        background-image: url("http://cirraybucket.oss-cn-shanghai.aliyuncs.com/blog/indicator.jpg");
    }
    .bg2 {
        background-image: url('http://cirraybucket.oss-cn-shanghai.aliyuncs.com/blog/workbench.jpg');
    }
    
</style>

<main class="wrapper">
    <section class="section parallax bg1">
        <h1>Such Adorableness</h1>
    </section>
    <section class="section static">
        <h1>Boring</h1>
    </section>
    <section class="section parallax bg2">
        <h1>SO FWUFFY AWWW</h1>
    </section>
</main>
```