---
title: Sass入门
date: 2020-02-07 15:41:32
tags: ["technology", "css"]
index_img: http://ww1.sinaimg.cn/large/78df2758gy1gbo2xmuu2gj24002o07wk.jpg
banner_img: http://ww1.sinaimg.cn/large/78df2758gy1gbo2xmuu2gj24002o07wk.jpg
---

## 1. Sass 简介
* Sass是Syntactically Awesome Stylesheet的缩写
* Sass是CSS的扩展
* Sass对所有CSS的版本完全兼容
* Sass减少了CSS的重复从而能节省时间
* Sass可以免费使用

### Sass示例
```
$bgcolor: lightblue;
$textcolor: darkblue;
$fontsize: 18px;
body {
  background-color: $bgcolor;
  color: $textcolor;
  font-size: $fontsize;
}
```
![Example](/images/example_2x.png)

### 先修课
* HTML
* CSS

### 为什么使用Sass？
因为CSS越来越多/大/复杂,且难以维护。

### Sass怎么工作？
浏览器不理解Sass代码，因此你需要一个Sass预处理器来把Sass代码转译成标准CSS代码。

### Sass文件类型
Sass文件使用 .scss 后缀。

### Sass注释
Sass支持标准CSS的注释 /* comment */, 此外它支持 // comment

## 2. Sass变量
Sass用$后跟名字来定义变量
变量种类有: string, number, color, boolean, list, null
```
$variablename: value;
```
参照Sass示例
### 变量作用域
参看以下两种转译效果
```
$myColor: red;
h1 {
  $myColor: green;
  color: $myColor;
}
p {
  color: $myColor;
}
```
> h1 { color: green; }  p { color: red; }
```
$myColor: red;
h1 {
  $myColor: green !global;
  color: $myColor;
}
p {
  color: $myColor;
}
```
> h1 { color: green; }  p { color: green; }

## 3. Sass嵌套规则和属性
### 嵌套规则
Sass允许你像组织HTML代码那样嵌套CSS选择器。
比如
```
nav {
  ul {
    margin: 0;
    padding: 0;
    list-style: none;
  }
  li {
    display: inline-block;
  }
  a {
    dispaly: block;
    padding: 6px 12px;
    text-decoration: none;
  }
}
```
### 嵌套属性
```
font: {
  family: Helvetica, sans-serif;
  size: 18px;
  weight: bold;
}
text: {
  align: center;
  transform: lowercase;
  overflow: hidden;
}
```
转译成
```
font-family: Helvetica, sans-serif;
font-size: 18px;
font-weight: bold;
text-align: center;
text-transform: lowercase;
text-overflow: hidden;
```

## 4. Sass @import和Partial部分文件
能让Sass代码保持DRY(Don't Repeat Yourself)的一种很重要的方法是，把相关代码分解到不同文件里去。这些小规模的文件可以有: reset file, variables, colors, fonts, font-sizes等。
和CSS一样，Sass同样支持@import指令。
CSS的@import指令有一个主要缺点是性能问题，你调用它时你得创建一个额外的HTTP请求。但Sass不会。
> @import filename;
**Tip:** 你不需要指明文件扩展名,Sass默认是.sass或.scss文件。比如:
```
@import "variables";
@import "colors";
@import "reset";
```

## 5. Sass @mixin和@include指令
@mixin指令能让你创建全局CSS代码
@include指令能让你使用(include)这些mixin。
### 定义 mixin
```
@mixin name {
  property: value;
  property: value;
  ...
}
```
下面例子创建了一个名为"important-text"的mixin
```
@mixin important-text {
  color: red;
  font-size: 25px;
  font-weight: bold;
  border: 1px solid blue;
}
```
### 使用 mixin
```
selector {
  @include mixin-name;
}
```
下面例子引用了上面定义的important-text mixin
```
.danger {
  @include important-text;
  background-color: green;
}
```
转译成的标准CSS为
```
.danger {
  color: red;
  font-size: 25px;
  font-weight: bold;
  border: 1px solid blue;
  background-color: green;
}
```
### mixin include mixin
```
@mixin special-text {
  @include important-text;
  @include link;
  @include special-border;
}
```
### 给mixin传参
```
/* Define mixin with two arguments */
@mixin bordered($color, $width) {
  border: $width solid $color;
}
.myArticle {
  @include bordered(blue, 1px);
}
.myNotes {
  @include bordered(red, 2px);
}
```
### mixin默认值
```
@mixin bordered($color:blue, $width:1px){
  border: $width solid $color;
}
.myTips {
  @include bordered($color:orange);
}
```

## 6. Sass继承和@extend
@extend指令在大多数样式相同，只有少数样式不一样的元素见很有用。来看下面例子
```
.button-basic {
  border: none;
  padding: 15px 30px;
  text-align: center;
  font-size: 16px;
  cursor: pointer;
}
.button-report {
  @extend .button-basic;
  background-color: red;
}
.button-submit {
  @extend .button-basic;
  background-color:green;
  color:white;
}
```
转译成的CSS即为
```
.button-basic, .button-report, .button-submit {
  border: none;
  padding: 15px 30px;
  text-align: center;
  font-size: 16px;
  cursor: pointer;
}
.button-report {
  background-color: red;
}
.button-submit {
  background-color: green;
  color: white;
}
```
