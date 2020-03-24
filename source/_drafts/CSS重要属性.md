---
title: CSS重要属性
tags:
---
## CSS 单位
CSS可以用许多不同的单位来表达长度。很多CSS属性会用到长度单位，比如width,margin,padding,font-size等。长度是用数字+单位来表达的，比如10px,2em等。总共有两种类型的长度单位:绝对和相对。
** 绝对长度 **
cm,mm,in,px,pt,pc
** 相对长度 **
| 单位 | 描述 |
| --- | --- |
| em | 相对于元素里的font-size |
| rem| 相对于根元素里的font-size|
| vw | 相对于1% viewport 的宽度|
| vh | 相对于1% viewport 的高度|
| %  | 相对于父元素            |

## Inheritance
在CSS中，inheritance(继承)控制着当一个元素的属性没有赋值时会发生什么。

## 自定义变量
全局变量可以定义在 :root或body选择器里。变量名必须用--(two dashes)开头。var()函数的使用方法var(custom-name, value). IE只支持15以上版本。
```
:root {
  --main-bg-color: coral;
  --main-txt-color: blue;
  --main-padding: 15px;
}
#div1 {
  background-color: var(--main-bg-color);
  color: var(--main-txt-color);
  padding: var(--main-padding);
}
#div2 {
  background-color: var(--main-bg-color);
  color: var(--main-txt-color);
  padding: var(--main-padding);
}
```


## box-sizing
box-sizing属性设置了一个元素的总长和总宽是怎么计算的。
默认情况下，元素的width和height只是用于元素的content box.如果元素有border或padding,那么页面在渲染时会把这些也加到盒子的尺寸上。也就是你得调整这两个值使得允许你增加border或padding。
* content-box,默认的box-sizing.如果你设置了元素的width 100px.那么元素的content box就是100px,任何border或padding都会增加渲染时的width,使得整个元素宽于100px
* border-box,如果你给元素width设置了100px,这100px会包含你将要增加的任何border或padding,即content box会收缩。通常设成border-box准没错。
### 举例看区别
```
<style>
  div {
    width: 160px;
    height: 80px;
    padding: 20px;
    border: 8px solid red;
    background: yellow;
  }
  .content-box {
    box-sizing: content-box;
  }
  .border-box {
    box-sizing: border-box;
  }
</style>

<div class="content-box">Content box</div>
<br>
<div class="border-box">Border box</div>
```

## 伪元素
CSS伪元素是用来给元素的特定部分设计样式的。比如，元素的第一个字母，第一行；给内容前/内容后增加内容。
总共有 ::before, ::after, ::first-letter, ::first-line, ::selection五个。
```
selector::pseudo-element {
  property:value;
}
```

## Media Queries
Media Queries可以检查很多东西:
1. viewport的宽或长
2. 设备的宽或长
3. 朝向
4. 分辨率
```
@media not|only mediatype and (expressions) {
  CSS-Code;
}
```
Media Types有四种: all, print, screen, speech. 最常用的是screen.
举例: viewport>=480时，背景色变lightgreen
```
@media screen and (min-width: 480px) {
  body {
    background-color: lightgreen;
  }
}
