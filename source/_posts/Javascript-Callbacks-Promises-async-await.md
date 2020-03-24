---
title: 'Javascript Callbacks,Promises,async/await'
date: 2020-03-11 15:43:08
tags: "javascript"
index_img: http://cirraybucket.oss-cn-shanghai.aliyuncs.com/thumbnail/javascript.jpg
banner_img: http://cirraybucket.oss-cn-shanghai.aliyuncs.com/blog/javascript.jpg
---
## Callbacks
Javascript的世界是异步的，我们执行一个操作，它要过一会才能完成。比如，最典型最常用的莫过于setTimeout.
还有一些真实的异步操作，比如loading scripts和modules.
来看一下函数loadScript(src)
```
function loadScript(src) {
    let script = document.createElement('script');
    script.src = src;
    document.head.append(script);
}
```
它把\<script src="...">动态加入文档的head,完成后浏览器自动装载执行。
```
// load and execute the script at the given path
loadScript('/my/script.js');
```
这个脚本是异步的，loadScript(...)下面的代码不能等它加载结束才执行。
```
loadScript('/my/script.js');
// the code below loadScript
// doesn't wait for the script loading to finish
// ...
```
也就是说，当新脚本加载完成，我们才能使用它。尽管它定义了一些我们想要执行的新函数，但我们却不能在loadScript(...)之后立马调用这些新函数。
```
loadScript('/my/script.js'); // the script has "function newFunction() {…}"
newFunction(); // no such function!
```
为了知道loadScript啥时加载完成，以便我们调用newFunction。让我们先加一个callback.
```
function loadScript(src, callback) {
    let script = document.createElement('script');
    script.src = src;
    script.onload = () => callback(script);
    document.head.append(script);
}
```
那么调用newFunction就得写在callback里
```
loadScript('/my/script.js', function(){
    newFunction();
})
```
思路: 第二个参数是一个用来异步调用的函数，仅当前面的操作完成时执行。
举例
```
function loadScript(src, callback) {
  let script = document.createElement('script');
  script.src = src;
  script.onload = () => callback(script);
  document.head.append(script);
}

loadScript('https://cdnjs.cloudflare.com/ajax/libs/lodash.js/3.2.0/lodash.js', script => {
  alert(`Cool, the script ${script.src} is loaded`);
  alert( _ ); // function declared in the loaded script
});
```
以上称为基于callback的异步编程。一个函数，想要做一些异步的事情，得提供callback参数，这个参数里定义需要异步执行的东西。

### Callback in callback
那我们怎么顺序异步加载更多脚本呢？
```
loadScript('/my/script.js', function(script) {
  loadScript('/my/script2.js', function(script) {
    loadScript('/my/script3.js', function(script) {
      // ...continue after all scripts are loaded
    });
  })
});
```

### 处理error
上面的例子我们没有考虑error,脚本加载失败时我们该怎么办?
```
function loadScript(src, callback) {
  let script = document.createElement('script');
  script.src = src;

  script.onload = () => callback(null, script);
  script.onerror = () => callback(new Error(`Script load error for ${src}`));

  document.head.append(script);
}
```
如何使用
```
loadScript('/my/script.js', function(error, script) {
  if (error) {
    // handle error
  } else {
    // script loaded successfully
  }
});
```
慢慢地，就演变成了"金字塔"call.
```
loadScript('1.js', function(error, script) {

  if (error) {
    handleError(error);
  } else {
    // ...
    loadScript('2.js', function(error, script) {
      if (error) {
        handleError(error);
      } else {
        // ...
        loadScript('3.js', function(error, script) {
          if (error) {
            handleError(error);
          } else {
            // ...continue after all scripts are loaded (*)
          }
        });

      }
    })
  }
});
```
好在我们有其他方法来避免"金字塔call",其中一个较好的是用"promises".

### 作业1

## Promise
1. "producing code"通常需要花点时间做事情。
2. “consuming code"想要在"producing code"完成时得到它的结果。
3. promise是一个特殊的Javascript对象，它串联起"producing code"和"consuming code".

promise对象的构造器语法
```
let promise = new Promise(function(resolve, reject){
    // executor (the producing code)
});
```
** Consumers: then,catch,finally **
Consuming functions可以通过.then, .catch, .finally来被注册。
```
promise.then(
  function(result) { /* handle a successful result */ },
  function(error) { /* handle an error */ }
);
```
举例
```
let promise = new Promise(function(resolve, reject){
    setTimeout(() => resolve("done!"), 1000);
})

promise.then(
    result => alert(result),
    error => alert(error)
);
// 如果只对成功完成感兴趣
// promise.then(result=>alert(result));
// 如果只对失败感兴趣
// promise.catch(error=>alert(error)); //等同于.then(null,f)
```
举例
让我们用Promise来重写loadScript
```
function loadScript(src) {
  return new Promise(function(resolve,reject){
    let script = document.createElement('script');
    script.src = src;
    script.onload = () => resolve(script);
    script.onerror = () => reject(new Error(`Script load error for ${src}`));
    document.head.append(script);
  });
}

let promise = loadScript("https://cdnjs.cloudflare.com/ajax/libs/lodash.js/4.17.11/lodash.js");
promise.then(
  script => alert(script.src + " is loaded!");
  error => alert("Error: " + error.message);
)
```

### 作业2

## Promise chaining
```
new Promise(function(resolve, reject) {
  setTimeout(() => resolve(1), 1000); // (*)
}).then(function(result) { // (**)
  alert(result); // 1
  return result * 2;
}).then(function(result) { // (***)
  alert(result); // 2
  return result * 2;
}).then(function(result) {
  alert(result); // 4
  return result * 2;
});
```
**思路:**promise.then返回一个promise,所以能在它上面继续调用.then

## Promise Error Handling
```
fetch('https://no-such-server.blabla') // rejects
  .then(response => response.json())
  .catch(err => alert(err)) // TypeError: failed to fetch (the text may vary)
```
.catch不要求立即跟在后面，通常会在所有.then的最后。

## Async/await
有一种更舒服地和promise一起工作的方式叫async/await.
```
async function f() {
    return 1;
}

f().then(res => console.log(res));
```
在函数前面用async表明这个函数总是会返回一个promise.其他值会自动封装进resolved promise.
即上面的return 1 和 return Promise.resolve(1)等价。
```
// works only inside async functions
let value = await promise;
```
await关键字使得Javascript等待直到promise执行完并返回结果。
```
async function f1() {
    let promise = new Promise((resolve,reject) => {
        setTimeout(()=>resolve("done!"),2000);
    });
    let result = await promise; // wait until the promise resolves
    console.log(result);
}

f1();
```
await会让程序等待promise返回再往下执行。但它不会浪费cpu资源，它只是把CPU的控制权暂时交出去而已。
显然async/await比promise.then更优雅更易读写。
** 注意 **
await不能出现在顶层代码里。如果需要，就用匿名的async函数包裹。
```
(async () => {
  let response = await fetch('/article/promise-chaining/user.json');
  let user = await response.json();
  ...
})();
```
此外，我们能用正常的try..catch来替代.catch
```
async function f() {
  try {
    let response = await fetch('/no-user-here');
    let user = await response.json();
  }catch(err){
    alert(err);
  }
}

f();
```
### 总结
async关键字有两个作用:
1. 使得后面的函数总能返回一个promise
2. 允许在它里面用await

await关键字使得Javascript等待直到promise结清。
有了async/await,我们就不需要再写promise.then/catch了。
