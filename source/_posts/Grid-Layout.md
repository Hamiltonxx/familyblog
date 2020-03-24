---
title: Grid Layout
date: 2020-02-29 10:27:12
tags: ["css"]
index_img: http://cirraybucket.oss-cn-shanghai.aliyuncs.com/thumbnail/layout.jpg
banner_img: http://cirraybucket.oss-cn-shanghai.aliyuncs.com/blog/layout.jpg
---
## 简介
Grid Layout是CSS中最强大的布局系统。它的CSS规则应用于Grid Container(父元素)和Grid Items(子元素)。跟Flexbox这种1维系统不一样，它是一个2维系统，可以同时处理column和row。
一直以来，CSS就在布局我们的web页面，但它从来没有做好它的工作。起初tables,然后floats,position和inline-block,但所有这些都有或多或少的问题，比如垂直居中。Flexbox可以帮助解决，但它主要用于一维布局，而不是复杂的二维布局。但不妨碍Flexbox和Grid可以很好的一起工作。

## 重要术语
### Grid Container
### Grid Item
以上两个概念和Flexbox里的类似，参考[Flexbox](http://blog.cirray.cn/2020/02/29/Flexbox/)
### Grid Line
![](https://css-tricks.com/wp-content/uploads/2018/11/terms-grid-line.svg)
### Grid Track
相邻两条grid line之间的空间。
![](https://css-tricks.com/wp-content/uploads/2018/11/terms-grid-track.svg)
### Grid Cell
![](https://css-tricks.com/wp-content/uploads/2018/11/terms-grid-cell.svg)
### Grid Area
由4条grid line组成的区域
![](https://css-tricks.com/wp-content/uploads/2018/11/terms-grid-area.svg)

## 属性
### display
定义一个grid container
```
.container {
  display: grid | inline-grid;
}
```
### grid-template-columns grid-template-rows
用空格分隔的一串数字定义列和行，数字代表track大小，即grid line之间的空间。
\<track-size> - 可以是长度、百分比或是空余空间的占比。
```
.container {
  grid-template-columns: <track-size> ... | <line-name> <track-size> ...;
  grid-template-rows: <track-size> ... | <line-name> <track-size> ...;
}

.container {
  grid-template-columns: 40px 50px auto 50px 40px;
  grid-template-rows: 25% 100px auto;
}
```
![](https://css-tricks.com/wp-content/uploads/2018/11/template-columns-rows-01.svg)
当你在track值之间留空白时，grid line会自动赋值。你也可以用[]显式命名line名。
```
.container {
  grid-template-columns: [first] 40px [line2] 50px [line3] auto [col4-start] 50px [five] 40px [end];
  grid-template-rows: [row1-start] 25% [row1-end] 100px [third-line] auto [last-line];
}
```
![](https://css-tricks.com/wp-content/uploads/2018/11/template-column-rows-02.svg)
如果你的定义里有重复部分，也可以用repeat()
```
.container {
  grid-template-columns: repeat(3, 20px [col-start]);
}
```
fr单位允许你设置track在grid container空余(剩余)空间中的占比。 比如下面的track为三分之一container.
```
.container {
  grid-template-columns: 1fr 1fr 1fr;
}

.container {
  grid-template-columns: 1fr 50px 1fr 1fr;
}
```
### grid-template-areas
给grid template里的area命名。
\<grid-area-name> - grid area的名字; . - 空grid cell; none - 没有定义grid area
```
.container {
  grid-template-areas: "<grid-area-name> | . | none | ..."
  "...";
}

.container {
  display: grid;
  grid-template-columns: 50px 50px 50px 50px;
  grid-template-rows: auto;
  grid-template-areas:
    "header header header header"
    "main main . sidebar"
    "footer footer footer footer";
}
```
![](https://css-tricks.com/wp-content/uploads/2018/11/dddgrid-template-areas.svg)
### grid-template
grid-template-rows, grid-template-columns和grid-template-areas的缩写.
```
.container {
  grid-template: none | <grid-template-rows> / <grid-template-columns>;
}

.container {
  grid-template:
    [row1-start] "header header header" 25px [row1-end]
    [row2-start] "footer footer footer" 25px [row2-end]
    / auto 50px auto;
}
```
相当于
```
.container {
  grid-template-rows: [row1-start] 25px [row1-end row2-start] 25px [row2-end];
  grid-template-columns: auto 50px auto;
  grid-template-areas:
    "header header header"
    "footer footer footer";
}
```
**推荐使用grid属性代替grid-template**
### grid-column-gap grid-row-gap
指定grid line的尺寸
```
.container {
  grid-column-gap: <line-size>;
  grid-row-gap: <line-size>;
}

.container {
  grid-template-columns: 100px 50px 100px;
  grid-template-rows: 80px auto 80px;
  grid-column-gap: 10px;
  grid-row-gap: 15px;
}
```
![](https://css-tricks.com/wp-content/uploads/2018/11/dddgrid-gap.svg)
### grid-gap
grid-row-gap和grid-column-gap的缩写。
```
.container {
  grid-gap: <grid-row-gap> <grid-column-gap>;
}

.container {
  grid-template-columns: 100px 50px 100px;
  grid-template-rows: 80px auto 80px;
  grid-gap: 15px 10px;
}
```
如果grid-row-gap没有指定，则该值等于grid-column-gap
### justify-items
grid item按row对齐
```
.container {
  justify-items: start | end | center | stretch;
}
```
start - 按cell的start边对齐; end - 按cell的end边对齐; center - 按cell的center对齐; stretch - 填满cell的宽度(默认)
```
.container {
  justify-items: start;
}
```
![](https://css-tricks.com/wp-content/uploads/2018/11/justify-items-start.svg)
```
.container {
  justify-items: end;
}
```
![](https://css-tricks.com/wp-content/uploads/2018/11/justify-items-end.svg)
```
.container {
  justify-items: center;
}
```
![](https://css-tricks.com/wp-content/uploads/2018/11/justify-items-center.svg)
```
.container {
  justify-items: stretch;
}
```
![](https://css-tricks.com/wp-content/uploads/2018/11/justify-items-stretch.svg)
这个属性也可以通过grid item的justify-self属性来设置。
### align-items
grid item按column对齐
```
.container {
  align-items: start | end | center | stretch;
}
```
四个值的含义同justify-items
```
.container {
  align-items: start;
}
```
![](https://css-tricks.com/wp-content/uploads/2018/11/align-items-start.svg)
```
.container {
  align-items: end;
}
```
![](https://css-tricks.com/wp-content/uploads/2018/11/align-items-end.svg)
```
.container {
  align-items: center;
}
```
![](https://css-tricks.com/wp-content/uploads/2018/11/align-items-center.svg)
```
.container {
  align-items: stretch;
}
```
![](https://css-tricks.com/wp-content/uploads/2018/11/align-items-stretch.svg)
这个属性也可以通过grid item的align-self属性来设置。
### place-items
同时设置align-items 和 justify-items属性。 <align-items> / <justify-items>
### justify-content
很多时候，所有grid的size总和会小于container的size.这时，你能设置grid在container里的对齐方式。此属性针对row.
```
.container {
  justify-content: start | end | center | stretch | space-around | space-between | space-evenly;
}
```
举例
```
.container {
  justify-content: start;
}
```
![](https://css-tricks.com/wp-content/uploads/2018/11/justify-content-start.svg)
```
.container {
  justify-content: end;
}
```
![](https://css-tricks.com/wp-content/uploads/2018/11/justify-content-end.svg)
```
.container {
  justify-content: center;
}
```
![](https://css-tricks.com/wp-content/uploads/2018/11/justify-content-center.svg)
```
.container {
  justify-content: stretch;
}
```
![](https://css-tricks.com/wp-content/uploads/2018/11/justify-content-stretch.svg)
```
.container {
  justify-content: space-around;
}
```
![](https://css-tricks.com/wp-content/uploads/2018/11/justify-content-space-around.svg)
```
.container {
  justify-content: space-between;
}
```
![](https://css-tricks.com/wp-content/uploads/2018/11/justify-content-space-between.svg)
```
.container {
  justify-content: space-evenly;
}
```
![](https://css-tricks.com/wp-content/uploads/2018/11/justify-content-space-evenly.svg)
### align-content
```
.container {
  align-content: start | end | center | stretch | space-around | space-between | space-evenly;
}
```
举例
```
.container {
  align-content: start;
}
```
![](https://css-tricks.com/wp-content/uploads/2018/11/align-content-start.svg)
```
.container {
  align-content: end;
}
```
![](https://css-tricks.com/wp-content/uploads/2018/11/align-content-end.svg)
```
.container {
  center;
}
```
![](https://css-tricks.com/wp-content/uploads/2018/11/align-content-center.svg)
```
.container {
  align-content: stretch;
}
```
![](https://css-tricks.com/wp-content/uploads/2018/11/align-content-stretch.svg)
```
.container {
  align-content: space-around;
}
```
![](https://css-tricks.com/wp-content/uploads/2018/11/align-content-space-around.svg)
```
.container {
  align-content: space-between;
}
```
![](https://css-tricks.com/wp-content/uploads/2018/11/align-content-space-between.svg)
```
.container {
  align-content: space-evenly;
}
```
![](https://css-tricks.com/wp-content/uploads/2018/11/align-content-space-evenly.svg)
### place-content
同时设置align-content和justify-content属性. \<align-content> / \<justify-content>
### grid-auto-columns grid-auto-rows
略
### grid-auto-flow
略
### grid
以下所有属性的缩写: grid-template-rows, grid-template-columns, grid-template-areas, grid-auto-rows, grid-auto-columns, grid-auto-flow
下面两段代码是相同的
```
.container {
  grid: 100px 300px / 3fr 1fr;
}

.container {
  grid-template-rows: 100px 300px;
  grid-template-columns: 3fr 1fr;
}
```
--- Grid Item ---
** float,display:inline-block,display:table-cell,vertical-align和column-\*属性不再有效 **
### grid-column-start grid-column-end grid-row-start grid-row-end
通过指定grid line来决定grid item的位置
```
.item {
  grid-column-start: <number> | <name> | span <number> | span <name> | auto;
  grid-column-end: <number> | <name> | span <number> | span <name> | auto;
  grid-row-start: <number> | <name> | span <number> | span <name> | auto;
  grid-row-end: <number> | <name> | span <number> | span <name> | auto;
}
```
\<line> - grid line的数字或名字; span \<number> - 跨越给的数字的grid tracks; span \<name> - 跨越直到给出名字的line的下一条; auto - 默认1
举例
```
.item-a {
  grid-column-start: 2;
  grid-column-end: five;
  grid-row-start: row1-start;
  grid-row-end: 3;
}
```
![](https://css-tricks.com/wp-content/uploads/2018/11/grid-column-row-start-end-01.svg)
```
.item-b {
  grid-column-start: 1;
  grid-column-end: span col4-start;
  grid-row-start: 2;
  grid-row-end: span 2;
}
```
如果没有定义 grid-column-end / grid-row-end，这个item默认span 1. Items可以重叠，我们可以用z-index来控制堆叠顺序。
### grid-column grid-row
grid-column-start + grid-column-end 和 grid-row-start + grid-row-end的缩写.
```
.item {
  grid-column: <start-line> / <end-line> | <start-line> / span <value>;
  grid-row: <start-line> / <end-line> | <start-line> / span <value>;
}

.item-c {
  grid-column: 3 / span 2;
  grid-row: third-line / 4;
}
```
![](https://css-tricks.com/wp-content/uploads/2018/11/grid-column-row.svg)
### grid-area
给item一个名字
```
.item {
  grid-area: <name> | <row-start> / <column-start> / <row-end> / <column-end>;
}

.item-d {
  grid-area: header;
}

.item-d {
  grid-area: 1 / col4-start / last-line / 6;
}
```
![](https://css-tricks.com/wp-content/uploads/2018/11/grid-area.svg)
### justify-self
在cell里给item做对齐
```
.item {
  justify-self: start | end | center | stretch;
}

.item-a {
  justify-self: start;
}
```
![](https://css-tricks.com/wp-content/uploads/2018/11/justify-self-start.svg)
```
.item-a {
  justify-self: end;
}
```
![](https://css-tricks.com/wp-content/uploads/2018/11/justify-self-end.svg)
```
.item-a {
  justify-self: center;
}
```
![](https://css-tricks.com/wp-content/uploads/2018/11/justify-self-center.svg)
```
.item-a {
  justify-self: stretch;
}
```
### align-self
在cell里给item做对齐
```
.item {
  align-self: start | end | center | stretch;
}

.item-a {
  align-self: start;
}
```
![](https://css-tricks.com/wp-content/uploads/2018/11/align-self-start.svg)
```
.item-a {
  align-self: end;
}
```
![](https://css-tricks.com/wp-content/uploads/2018/11/align-self-end.svg)
```
.item-a {
  align-self: center;
}
```
![](https://css-tricks.com/wp-content/uploads/2018/11/align-self-center.svg)
```
.item-a {
  align-self: stretch;
}
```
### place-self
同时定义align-self和justify-self
举例
```
.item-a {
  place-self: center;
}
```
![](https://css-tricks.com/wp-content/uploads/2018/11/place-self-center.svg)
```
.item-a {
  place-self: center stretch;
}
```
![](https://css-tricks.com/wp-content/uploads/2018/11/place-self-center-stretch.svg)

## 作业
> ** Grid Layout & Responsive Design **
```
<style>
  body {
    margin: 0;
  }
  .container {
    display: grid;
    grid-gap: 10px 20px;
    grid-template-rows: 100px 1fr auto;
    grid-template-columns: 1fr 200px;
    grid-template-areas: "header header"
                         "content aside"
                         "footer footer";
    height: 100vh;
    box-sizing: border-box;
    font-size: 2em;
    border: 0.1em solid;
    border-radius: 0.2em;
  }
  .item {
    border-width: 0.1em;
    border-style: solid;
    border-radius: 0.2em;
    padding: 0.5em;
  }
  header {
    grid-area: header;
    background-color: rgba(242,46,138,0.8);
    border-color: rgb(242,46,138);
  }
  main {
    grid-area: content;
    background-color: rgba(98,205,217,0.8);
    border-color: rgb(98,205,217);
  }
  aside {
    grid-area: aside;
    background-color: rgba(117,191,6,0.8);
    border-color: rgb(117,191,6);
  }
  footer {
    grid-area: footer;
    background-color: rgba(242,226,5,0.8);
    border-color: rgb(242,226,5);
  }

  @media (max-width: 600px) {
    .container {
      grid-gap: 0;
      grid-template-rows:auto auto 1fr auto;
      grid-template-columns: 1fr;
      grid-template-areas:"header"
                          "aside"
                          "content"
                          "footer";
    }
  }
</style>


<section class="container">
  <header class="item">Header</header>
  <main class="item">Content</main>
  <aside class="item">Aside</aside>
  <footer class="item">Footer</footer>
</section>
```