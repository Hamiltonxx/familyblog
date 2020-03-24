---
title: Python文件处理
date: 2020-03-14 13:37:52
tags: "python"
index_img: http://cirraybucket.oss-cn-shanghai.aliyuncs.com/thumbnail/monitors.jpg
banner_img: http://cirraybucket.oss-cn-shanghai.aliyuncs.com/blog/monitors.jpg
---

Python有很多内置模块和函数来处理文件。这些函数可能会在多个包里面，比如os,os.path,shutil和pathlib等。本文把这许多函数集中到一块，涵盖了Python文件处理最常用的操作。内容如下
* 获取文件属性
* 创建目录
* 匹配文件名
* 遍历目录树
* 创建临时文件和文件夹
* 删除文件和目录
* Copy,move或rename
* 创建和解压ZIP及TAR压缩包
* 用fileinput模块打开多个文件

## 获取目录列表
旧版本用os.listdir(),但推荐使用新版本的os.scandir(),它能拿到文件/目录属性，比如文件大小，修改日期。os.scandir()返回一个ScandirIterator对象。
```
import os
with os.scandir('my_directory/') as entries:
    for entry in entries:
        print(entry.name)
```
```
sub_dir_c
file1.py
sub_dir_b
file3.txt
file2.csv
```
## 只列出所有文件/文件夹
```
import os
with os.scandir('my_directory/') as entries:
    for entry in entries:
        if entry.is_file(): # if entry.is_dir():
            print(entry.name)
```
```
file1.py
file3.txt
file2.csv
```
## 获取文件属性
```
import os
with os.scandir('my_directory/') as entries:
    for entry in entries:
        info = entry.stat()
        print(info.st_mtime)
```
```
539032199.0052035
1539032469.6324475
1538998552.2402923
1540233322.4009316
1537192240.0497339
1540266380.3434134
```
每个ScandirIterator对象有一个.stat()方法，它能获取文件或目录的信息。st_mtime属性正是最后修改时间。
## 创建目录 
```
import os
try:
    os.mkdir('example_directory/')
    # os.mkdirs('2018/10/05', mode=0o770)
except FileExisitsError as exc:
    print(exc)
```
## 删除文件/目录
```
import os
thefile = '/path/to/file/data.txt'
os.remove(thefile)

trash_dir = 'my_documents/bad_dir'
try:
    os.rmdir(trash_dir)
except OSError as e:
    print(e.strerror)

import shutil
trash_dir = 'my_documents/bad_dir'
try:
    shutil.rmtree(trash_dir)
except OSError as e:
    print(e.strerror)
```
## 判断文件/目录是否存在
```
import os
print(os.path.exists(".gitignore"))
print(os.path.exists("app"))
# 判断是文件还是目录
print(os.path.isfile(".gitignore"))
print(os.path.isdir(".gitignore"))
print(os.path.isfile("app"))
```
