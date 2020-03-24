---
title: Flexbox
tags:
  - css
index_img: 'http://cirraybucket.oss-cn-shanghai.aliyuncs.com/thumbnail/books.jpg'
banner_img: 'http://cirraybucket.oss-cn-shanghai.aliyuncs.com/blog/books.jpg'
date: 2020-02-29 00:59:37
---

## 背景
Flexbox布局的目标是提供更高效的方式来lay out,对齐和在items之间分发空白，即便它们的尺寸是未知的或动态的。注意，Flexbox layout最适合用于应用中的components,以及小比例的布局。而Grid layout则更适用大比例布局。

## 基本术语
Flexbox是一系列属性的集合，有些是给container(父元素)的，另一些是给items(子元素)的。这里有四个概念，container,item,main,cross，请看下图
![](https://css-tricks.com/wp-content/uploads/2018/11/00-basic-terminology.svg)

### display
定义一个flex container.
```
.container {
  display: flex;
}
```
### flex-direction
```
.container {
  flex-direction: row | row-reverse | column | column-reverse;
}
```
默认是row,四个分别是left to right, right to left, top to bottom, bottom to top.
### flex-wrap
```
.container {
  flex-wrap: nowrap | wrap | wrap-reverse;
}
```
设置items是否被强制到一行，还是可以多行。默认是nowrap，只能显示一行；wrap则可以多行显示。
### justify-content
此属性定义了沿着main axis的对齐方式。
![](https://css-tricks.com/wp-content/uploads/2018/10/justify-content.svg)
```
.container {
  justify-content: flex-start | flex-end | center | space-between | space-around | space-evenly | start | end | left | right ... + safe|unsafe;
}
```
### align-items
此属性定义了沿着cross axis的对齐方式。
![](https://css-tricks.com/wp-content/uploads/2018/10/align-items.svg)
```
.container {
  align-items: stretch | flex-start | flex-end | center | baseline | first baseline | last baseline | start | end | self-start | self-end + ... safe|unsafe;
}
```
### align-content
此属性定义了，当cross-axis上有空白时，如何对齐.
![](https://css-tricks.com/wp-content/uploads/2018/10/align-items.svg)
```
.container {
  align-content: flex-start | flex-end | center | space-between | space-around | space-evenly | stretch | start | end | baseline | first baseline | last baseline + ... safe|unsafe;
}
```
### order
![](https://css-tricks.com/wp-content/uploads/2018/10/order.svg)
默认情况下，flex item按代码里的顺序布局。但order属性可以改变。
```
.item {
  order: <integer>; /* default is 0 */
}
```
### flex-grow
![](https://css-tricks.com/wp-content/uploads/2018/10/flex-grow.svg)
此属性定义了flex item增长的能力。数字代表该item在container空间中的占比。
```
.item {
  flex-grow: <number>; /* default 0 */
}
```
### flex-shrink
定义了flex item在必要时收缩的能力。
```
.item {
  flex-shrink: <number>; /* default 1 */
}
```
### flex-basis
```
.item {
  flex-basis: <length> | auto;
}
```
### flex
这是flex-grow,flex-shrink和flex-basis合并的简写。默认为0 1 auto.
```
.item {
  flex: none | [<'flex-grow'> <'flex-shrink'>? || <'flex-basis'> ]
}
```
推荐使用这个简写形式而不是单独属性。
### align-self
```
.item {
  align-self: auto | flex-start | flex-end | center | baseline | stretch;
}
```

## 作业1
背景图上文字居中。
```
<div class="banner">
  <div class="banner-text">
    <h1>Flexbox Banner</h1>
    <h3>Perfectly Centered</h3>
    <h6>No matter how many lines of text you use.</h6>
  </div>
</div>

<style>
  * {
    margin:0;
  }
  body {
    font-family: Helvetica,Aria,sans-serif;
    background: #f2f2f2;
  }
  h1 {
    font-size:60px;
  }
  h3 {
    font-size:32px;
    font-weight: normal;
    text-transform: uppercase;
    letter-spacing: 4px;
  }
  h6 {
    font-size:22px;
  }
  .banner-text {
    text-shadow: 2px 2px 6px #000;
    text-align: center;
  }
  .banner {
    color: white;
    background: url('http://cirraybucket.oss-cn-shanghai.aliyuncs.com/blog/indicator.jpg') top left/cover no-repeat;
    height: 300px;
    display: flex;
    justify-content: center;
    align-items:center;
  }
</style>
```
![](/images/perfect_center.png)

## 作业2
做一个Navigation Menu(Header Nav).
```
<style>
    * {
        margin:0;
    }
    header {
        padding: 16px;
        background-color: #f3f3f3;
    }
    header nav {
        display: flex;
        justify-content: space-between;
    }
    header ul {
        list-style: none;
        display: flex;
    }
    header ul li:not(:last-child) {
        margin-right: 64px;
    }
    header nav button.contact {
        border: none;
        font-size: 1.2rem;
        background-color: #f3f3f3;
    }
</style>

<section class="container">
    <header>
        <nav>
            <h2>LOGO</h2>
            <ul>
                <li>首页</li>
                <li>关于我们</li>
                <li>商务合作</li>
                <li>新闻动态</li>
            </ul>
            <button class="contact">
                400-600-8888
            </button>
        </nav>
    </header>
</section>
```
```
#######################################################
#                                                     #
# LOGO   首页  关于我们  商务合作  新闻动态   400-600-8888 #
#                                                     #
#######################################################
```
