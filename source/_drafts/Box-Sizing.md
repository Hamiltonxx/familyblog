---
title: Box Sizing
tags:
---
## 前言
box-sizing属性可以使CSS布局更容易更直观。它真有那么有用和受喜爱以致值得单独写一篇？我们先讲点CSS历史。

## 盒模型历史
在CSS远古时期，盒模型默认就以下面这种方式工作:
width + padding + border = 元素盒子实际可视/被渲染的宽度
height + padding + border = 元素盒子实际可视/被渲染的高度
因为当你开始给一个元素增加padding和border后，给元素设置的width和height就会渐渐消失，这很违背直觉。