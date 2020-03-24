---
title: Visual Studio Code
tags:
  - technology
  - python
date: 2020-03-05 15:26:00
index_img: 'http://cirraybucket.oss-cn-shanghai.aliyuncs.com/thumbnail/piano.jpg'
banner_img: 'http://cirraybucket.oss-cn-shanghai.aliyuncs.com/blog/piano.jpg'
---

## 前言
我已经太习惯Sublime Text了，以致短时间内我都没法接受别的Editor. 即便我知道Sublime的很多缺点。我能感受到的它的优点是: 1.轻便!看上去就像Editor而不是IDE。2.简洁！没有多余的bar和button，让我有写代码的快感。 3.熟悉！毕竟写了这么多年，安装卸载插件、环境配置都很熟悉。但它的缺点也很明显: 1.Python没法debug,没有debug插件,pdb根本用不来。2.不开源，要收费，网上下的lisence很快就过期。 最重要的是visual studio code代表了未来，现在vsc用的人越来越多，st越来越少。不管怎样，我都得痛下决心转到vsc了。

## 打造Python开发环境
> 1. 下载安装Visual Studio Code
> 2. 点击安装 Microsoft Python extension
> 3. 打开 Command Palette, Python: Select Interpretor. 我习惯用~/projects/venv/bin/python
> 4. fn+F1 打开 Command Palette, Shell Command: Install 'code' command in PATH
> 5. 字体。 在Settings里把font-size改为16

这几步为常规操作，但我想把工具更精简一些。首先在View里把MiniMap去掉。然后最左边这个bar看着占地方，我也不经常debug，需要时我再调出来。 于是在View -> Appearance里把Activity Bar和Status Bar都取消掉，现在只剩Sidebar和Edit Area了，看起来就清爽多了。某些情况需要隐藏Sidebar,用command+b来toggle即可.

## 有用的快捷键
### 显示
* 隐藏/显示Sidebar: command + b
* Command Palette: fn + F1
* 切换全屏: control + command + f
* 放大/缩小: command + + / command + -
* 显示输出面板: Shift + command + u
* 显示集成终端: control + `
* 打开markdown preview: Shift + command + v
* md preview side-by-side: command + k + v

### 键盘组合
* 向前删除: fn + Delete
* Page Up: fn + Up
* Page Down: fn + Down
* Home: fn + Left
* End: fn + Right

### 基本编辑
移动当前行向下/向上: option + Down / option + Up
复制当前行向下/向上: Shift + option + Down / Shift + option + Up
删除当前行: Shift + command + k
在上插入一行: Shift + command + Enter
跳转到匹配的括号: Shift + command + \
向左/向右缩进当前行: command + [ / command + ]
跳到当前行的头部/尾部: fn + Left / fn + Right
跳到页头/页尾: fn + command + Up / fn + command + Down
向上/向下滚动: fn + control + Up / fn + command + Down

### 调试
* 切换断点: F9
* 开始/继续: F5
* 暂停: Shift + F5
* 跳进/跳出: F11 / Shift + F11
* 跳过: F10
