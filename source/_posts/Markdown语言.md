---
title: Markdown语言
date: 2020-01-31 20:38:53
tags: ["blog","technology"]
index_img: http://ww1.sinaimg.cn/large/78df2758gy1gbg15gwygoj23v93kq4qr.jpg
banner_img: http://ww1.sinaimg.cn/large/78df2758gy1gbg15gwygoj23v93kq4qr.jpg
---
## 前言
2020年的春节过的很不平静，武汉肺炎肆虐全中国，有生以来第一次得宅在家里过年，门都不能出，更休谈拜年聚会活动。有人选择躺床上啥都不动为国做贡献，有人选择在线监工造医院大楼，有人选择房间密码地主麻将，而我和刚过门的媳妇则一起折腾起我们的博客。我们选择的Static Site Generator(SSG)为node系列的hexo,deploy在github page上。地址为 [https://hamiltonxx.github.io](https://hamiltonxx.github.io) 为了快速写博客，就不得不先学一下Markdown语言。
> If you don't have a website nowadays, you don't exist!
## 初识Markdown
Markdown是一门标记语言(Markup Language)。有了它，你只需在写普通文本时加上一些轻量注解，就能使文本按照你所想要的格式展现。标记语言的注解是用来区分文章内容和格式用的。
相比于WYSIWYG(所见即所得),标记语言更注重语义信息.你可以标注段落从何开始，标题如何展示等。通过文本和样式的解耦，也方便你生成各式不同的输出(output),如HTML,PDF或其他能理解标记语言的文档。
在所有标记语言中，Markdown是最简单的(之一).
## 为何使用Markdown
如果你习惯于用WYSIWYG编辑器，比如Microsoft Word,你可能会问为何还要用Markdown文件。因为你已经可以按你想要的格式化，并导出成你想要的格式。对于仅需格式化一次并生成一种格式的文档，你并不需要Markdown[虽然我还是会极力争辩Markdown在这种情况下仍旧是一个非常不错的选择]，但更高级的一些应用才是Markdown真正闪耀的地方。
并不是每个人都有Word,但每个人都有普通文本的编辑器，只有在普通文本文件上，你才能知道你正在编辑的是什么东西。如果文本和格式分离，那么更有艺术细胞的人就可以来处理格式，而我只需要写写文本。

## Markdown基本语法
如果你曾写过分享过普通文档，那么你很可能已经熟悉大多数标注。

### 头
新分一段，你会给他一个头(标题),头以#(hashtag)开始。一个#代表一级头，两个#代表二级头，同时也给你一个层级二的分段。
```line_numbers=false
# Header level 1
## Header level 2
### Header level 3
```
任何你在该头下写的文本，自动成为该层级的分段。头默认是编号的，你可以用模板或者头的尾部加{-}或{.unnumbered}取消编号。
```ruby?line_numbers=false
# Unnumbered header {-}
## Another unnumbered header { .unnumbered }
```

### 段落
可以用一个空行(blank line)来区分多行。注意不能用空格或tab键来缩进段落。
```
I really like using Markdown.

I think I'll use it to format all of my documents from now on.
```

### 换行
为了创建换行(line break\<br>), 可以在行尾加两个或多个空格, 然后敲return。
```
This is the first line.  
And this is the second line.
```

### 强调
我们会通过斜体或黑体来强调部分文本。在Markdown中，我们使用*或_ (asterisk or underscore)。放在一个*或_ 中表示斜体，放在两个*或_ 中表示黑体。
```
*斜体* _斜体_
**黑体** __黑体__
```

### 列表
列表有两种类型，编号的和非编号的。
编号列表是在数字后跟.(dot),再接列表项文本
```
1. This is a numbered list.
2. Where this is list item two. 
3. And this is list item three.
```
如果你想一个列表项跨多行，你需要缩进。
```
1. This is a multi-line list item. 
   This is also part of the list item. 
   And so is this
2. Here is another one.
   Where this is also part of the list item.
```
非编号列表则用asterisk(*)或dash(-)来代替数字
```
* This is a numbered list
- Where this is list item two 
* And this is list item three
```
如果你想在列表项之下创建子列表，你需要缩进。
```
* This is a top-level list item 
  * Here is a sublist item
  * Here is another
* Now we are at the top level again.
```

### 块引用
当文本里需要增加引用时，把'>'放在文本前面。
```
> This is a blockquote. The blockquote 
> can span multiple lines. If you don't 
> put any new lines in it, you only
> need to put the ">" at the beginning 
> of the line.
> If you want multiple lines where you 
> include new lines, you should add
> the ">" to each line.
```

### 代码块
正常情况下缩进4个空格或1个tab,如果它们在列表中，则缩进8个空格或2个tab。
```
    <html>
      <head>
      </head>
    </html>
```
```
1.  Open the file.
2.  Find the following code block on line 21:

        <html>
          <head>
            <title>Test</title>
          </head>

3.  Update the title to match the name of your website.
```

### 链接
Markdown最初目的是为了使写网页更简便，自然它就有插入超链接的语法。
```
This is a link to [my blog](https://hamiltonxx.github.io).
```
如果文章里有多处引用同一链接，
```
This is a link to [my blog][blog].
```
然后，在接下来的文本中，你定义一下该链接指向何处
```
[blog]: https://hamiltonxx.github.io
```
同样，如果想把链接指向某个段落，只需要把段头放在中括号里
```
This is a link to the [Writing Markdown] chapter.
```

### 图片
图片链接和超链接很相似，唯一区别是需要在中括号前加"!"
```
![图片名称](图片URL)
```
如果图片放在本地
```
![图片名称](path-to/my-figure)
```

### 水平线
用三个或更多的asterisks(***),dashes(---)或underscores(___).
```
***
---
___________
```

## Markdown扩展语法
并不是所有的Markdown应用都支持这些扩展语法，用之前得先检查你的应用是否支持。

### 表格
用三个或更多(---)来创建表头，用(|)来创建列
```
|Syntax    | Description  |
|-------   | -----------  |
|Header    |Title         |
|Paragraph |Text          |
```
单元长度可变，也就是上面和如下的是一样的
```
| Syntax | Description |
| --- | ----------- |
| Header | Title |
| Paragraph | Text |
```
#### 对齐
左对齐 :---, 右对齐 ---:, 居中 :---:
```
| Syntax      | Description | Test Text     |
| :---        |    :----:   |          ---: |
| Header      | Title       | Here's this   |
| Paragraph   | Text        | And more      |
```

### 围栏代码块

\```
{
  "firstName": "John",
  "lastName": "Smith",
  "age": 25
}
\```

#### 语法高亮
\```json
{
  "firstName": "John",
  "lastName": "Smith",
  "age": 25
}
\```