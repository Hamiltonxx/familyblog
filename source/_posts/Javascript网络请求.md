---
title: Javascript网络请求
date: 2020-03-11 14:34:15
tags: "javascript"
index_img: http://cirraybucket.oss-cn-shanghai.aliyuncs.com/thumbnail/javascript.jpg
banner_img: http://cirraybucket.oss-cn-shanghai.aliyuncs.com/blog/javascript.jpg
---
## 前言
鉴于公司小程序开发还没有招到。我的挪瓦咖啡简易商城就先做在公众号里。除了会用到CSS的Flex布局外，Javascript的Network Request也是必不可少。只是简单的网页里，我并不想用Angular/Vue等重机枪，所以这篇主要讲Javascript自带的Fetch.

## Fetch
基本语法:
> let promise = fetch(url, [options])
拿到response通常分两步: 1. fetch会立刻回返带有headers的response. 2.用额外方法去拿response body.
第1步，我们可以检查HTTP状态，查看是否成功，检查headers,但是没有body.可以查看以下两个属性
* status - HTTP status code,比如200
* ok - boolean,如果status code为200~299,则true
```
let response = await fetch(url);
if (response.ok) {
    let json = await response.json();
}else{
    alert("HTTP-Error: " + response.status);
}
```
第2步，Response提供了多个方法来取body
* response.text() 
* response.json()
* response.formData()
* response.blob()
```
let url = 'https://api.github.com/repos/javascript-tutorial/en.javascript.info/commits';
let response = await fetch(url);

let commits = await response.json(); // read response body and parse as JSON

alert(commits[0].author.login);
```
不用await,用纯promises语法就是
```
fetch('https://api.github.com/repos/javascript-tutorial/en.javascript.info/commits')
    .then(response => response.json())
    .then(commits => alert(commits[0].author.login));
```
** 拿response header **
```
let response = await fetch('https://api.github.com/repos/javascript-tutorial/en.javascript.info/commits');
alert(response.headers.get('Content-Type'));
for (let [key,value] of response.headers) {
    alert(`${key} = ${value}`);
}
```
** 设置request header **
```
let response = fetch(protectedUrl, {
    headers: {
        Authentication: 'secret';
    }
});
```

### POST
```
let user = {
    name: 'John',
    surname: 'Smith'
};

let response = await fetch('/article/fetch/post/user', {
    method: 'POST',
    headers: {
        'Content-Type':'application/json;charset=utf-8'
    },
    body: JSON.stringify(user)
});

let result = await response.json();
alert(result.message);
```

### 发送image
待续

### 总结
一个典型的fetch请求包含两步
```
let response = await fetch(url, options);
let result = await response.json();
```
或
```
fetch(url, options)
    .then(response => response.json())
    .then(result => /* process result */);
```
### 作业
** Fetch Github的用户 **

## FormData
本节关于发送HTML form,可以带文件，带附加字段等。
```
let formData = new FormData([form]);
```
如果页面HTML里有form元素，它会自动抓取form fields. Fetch可以接受FormData对象作为它的body,encode之后通过Content-Type: form/multipart发送。从server视角看，它和普通form提交没啥两样。
让我们先来发送一个简单的form.
```
<form id="formElem">
    <input type="text" name="name" value="John">
    <input type="text" name="surname" value="Smith">
    Picture: <input type="file" name="picture" accept="image/*">
    <input type="submit">
</form>

<script>
    formElem.onsubmit = async (e) => {
        e.preventDefault();
        let response = await fetch('/article/formdata/post/user', {
            method: 'POST',
            body: new FormData(formElem)
        });
        let result = await response.json();
        alert(result.message);
    };
</script>
```
### FormData函数
* formData.append(name,value)
* formData.append(name,blob,fileName)
* formData.delete(name)
* formData.get(name)
* formData.has(name)
* formData.set(name,value)

## Fetch:下载进度
fetch方法允许跟踪下载进度。但请注意，目前fetch没有方法跟踪上传进度，那个只能回到XMLHttpRequest。

## Fetch: Abort
```
let controller = new AbortController();
```

## Fetch: Cross-Origin Requests
略

## Fetch API
```
let promise = fetch(url, {
  method: "GET", // POST, PUT, DELETE, etc.
  headers: {
    // the content type header value is usually auto-set
    // depending on the request body
    "Content-Type": "text/plain;charset=UTF-8"
  },
  body: undefined // string, FormData, Blob, BufferSource, or URLSearchParams
  referrer: "about:client", // or "" to send no Referer header,
  // or an url from the current origin
  referrerPolicy: "no-referrer-when-downgrade", // no-referrer, origin, same-origin...
  mode: "cors", // same-origin, no-cors
  credentials: "same-origin", // omit, include
  cache: "default", // no-store, reload, no-cache, force-cache, or only-if-cached
  redirect: "follow", // manual, error
  integrity: "", // a hash, like "sha256-abcdef1234567890"
  keepalive: false, // true
  signal: undefined, // AbortController to abort request
  window: window // null
});
```

## URL对象
内建的URL类提供了创建和解析URL很方便的一个接口。没有哪个网络方法指名需要URL对象，有string就足够了。所以从技术上讲，我们并不需要使用URL.只是有时候，它很有用。

## XMLHttpRequest
略

## 长轮询
略

## Websocket
略