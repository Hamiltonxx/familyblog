---
title: Web抓取领域里的Python工具
date: 2020-02-02 13:20:23
tags: ['python','爬虫']
index_img: http://cirraybucket.oss-cn-shanghai.aliyuncs.com/thumbnail/spider.jpg
banner_img: http://cirraybucket.oss-cn-shanghai.aliyuncs.com/blog/spider.jpg
---

Web网页抓取可以通过多种不同的Python框架实现，你有很多选择，每个选择对应不同的需求。
最常见的网页抓取Python3工具有以下几个:

1. Urllib2
2. Requests
3. BeautifulSoup
4. Lxml
5. Selenium
6. MechanicalSoup

所有这些框架/工具中，只有urllib2是Python自带的，其他工具都可以通过pip安装。

## Urllib2
Urllib2是一个获取url的python模块。它提供了一些很简单的接口来获取诸如HTTP,FTP这些协议的URL。但它对HTTPS无能为力。
```
from urllib.request import urlopen

html = urlopen(myURL)

print(html.read())
```

## Requests
Requests允许你发送 HTTP/1.1 请求。你可以通过简单的Python dictionary发送header,form data,multipart file和其他parameter,通过同样的方式也能获得response data.
```
import requests
req = requests.get('https://hamiltonxx.github.io')

print(req.encoding)      
print(req.status_code)  
print(req.elapsed)      
print(req.url)          
print(req.history)      
print(req.headers['Content-Type'])
```

## BeautifulSoup
```
pip install beautifulsoup4
```
BeautifulSoup 是一个解析库，它能使用不同的解析器。它默认的解析器来自Python标准库。它能创建一棵能从HTML里提取数据的解析树。它会自动把输入文档转成Unicode，然后输出成UTF-8文档。
```
from bs4 import BeautifulSoup
import requests

r = requests.get("https://hamiltonxx.github.io")

soup = BeautifulSoup(r.text)

for link in soup.find_all('a'):
  print(link.get('href'))
```

## Lxml
Lxml是一个高性能、高质量的HTML和XML解析库。如果你想要速度，就用Lxml. Lxml有很多模块，其中一个最重要的模块是etree, 负责创建和组织所有这些元素。
```
from lxml import etree

root_elem = etree.Element('html')
etree.SubElement(root_elem, 'head')
etree.SubElement(root_elem, 'title')
etree.SubElement(root_elem, 'body')

print(etree.tostring(root_elem, pretty_print=True).decode('utf-8'))
```

## Selenium
有一些网站使用javascript来提供内容。比如，你需要往下滚页面或点击按钮，来加载更多的内容。这种情况就需要selenium.
```
from selenium import webdriver
path_to_chromedriver = '/Users/Admin/Desktop/chromedriver'
browser = webdriver.Chrome(executable_path = path_to_chromedriver)

url = 'https://hamiltonxx.github.io'
browser.get(url)
```

## MechanicalSoup
MechanicalSoup库可以使和网站的交互自动化。它自动存储和发送cookie,跟踪重定向和链接以及提交表单。
```
import mechanicalsoup

browser = mechanicalsoup.StatefulBrowser()
value = browser.open('https://hamiltonxx.github.io')
print(value)

value1 = browser.get_url()
print(value1)

value2 = browser.follow_link("forms")
print(value2)

value = browser.get_url()
print(value)
```

## Scrapy
Scrapy是一个提前所需网站信息的爬虫框架。它可以管理请求、保存session、处理输出管道。
```
import scrapy

class HamiltonSpider(scrapy.Spider):
  name = "hami_spider"
  start_urls = ['https://hamiltonxx.github.io']

  def parse(self, response):
    SET_SELECTOR = 'hamilton'
    for hami in response.css(SET_SELECTOR):
      pass
```
用下面这命令来运行scrapy代码
```
scrapy runspider samplescapy.py
```

以上就是常用的一些python3爬虫库。最最常用的库莫过于BeautifulSoup和Scrapy,后续我们将再详细讲这两个框架的异同以及各自怎么应用开发。

## 作业
爬取[https://www.a2oj.com/Ladders.html](https://www.a2oj.com/Ladders.html)网站或[https://hamiltonxx.github.io/2020/01/12/Codeforces天梯/](https://hamiltonxx.github.io/2020/01/12/Codeforces天梯/) 的题目和链接做成table
### 参考答案
```
from bs4 import BeautifulSoup 
import requests 

with open("beginner.md","a") as f:
    for j in range(22,33):
        f.write('<details>\n')
        f.write('<summary>Codeforces Rating</summary>\n\n')
        f.write('#### 200 problems.\n\n')

        r = requests.get('https://www.a2oj.com/Ladder'+str(j)+'.html')
        soup = BeautifulSoup(r.text)

        trs = soup.find_all('tr')

        f.write('|ID|Problem Name|Difficulty Level|\n')
        f.write('|---|---|:---:|\n')

        for i in range(4,204):
            tr = trs[i]
            id = tr.td.text
            name = tr.a.text
            url = tr.a["href"]
            dl = tr.find_all('td')[-1].text 

            f.write('|'+id+'|['+name+']('+url+')|'+dl+'|\n')

        f.write('\n')
        f.write('</details>\n\n')
```
