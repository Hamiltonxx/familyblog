---
title: 100行代码写个Javascript Router
tags:
---
如今遍地都是SPA,或基于React或Vue或纯Javascript。为了让SPA足够优秀，必须得有足够优秀的router机制。navigo或react-router这些库很亮，但是它们怎么工作？我们是否真的需要导入整个库？还是仅仅用它10%就可以了。通常来说，创建一个快速而有用的router很简单，只需要不到100行代码。

## 要求
我们的router应该
* 用es6+写
* 兼容history和hash routing
* 可复用

## 手撕代码
通常一个webapp只要用到一个router实例，但还是有很多场景会要用到多个，因此我们不能用单例来实现。我们的router需要4个基本属性：
* routes: 挂钩的routes列表
* mode: hash或history
* root: 在history模式下本应用的root
* constructor: 创建新Router实例的构造函数。

```
class Router {
    routes = [];
    mode = null;
    root = '/';

    constructor(options) {
        this.mode = window.history.pushState ? 'history' : 'hash';
        if (options.mode) this.mode = options.mode;
        if (options.root) this.root = options.root;
    }

    /* 增加和删除routes */
    add = (path, cb) => {
        this.routes.push({path,cb});
        return this;
    };
    remove = path => {
        for (let i=0; i<this.routes.length; i+=1){
            if (this.routes[i].path === path) {
                this.routes.slice(i,1);
                return this;
            }
        }
        return this;
    };
    flush = () => {
        this.routes = [];
        return this;
    };

    /* Get 当前路径 */
    clearSlashes = path => 
        path
            .toString()
            .replace(/\/$/, '')
            .replace(/^\//, '');
    getFragment = () => {
        let fragment = '';
        if (this.mode === 'history') {
            fragment = this.clearSlashes(decodeURI(window.location.pathname + window.locatio.search));
            fragment = fragment.replace(/\?(.*)$/, '');
            fragment = this.root !== '/' ? fragment.replace(this.root, '') : fragment;
        }else{
            const match = window.location.href.match(/#(.*)$/);
            fragment = match ? match[1] : '';
        }
        return this.clearSlashes(fragment);
    };

    /* Navigate to */
    navigate = (path = '') => {
        if (this.mode === 'history') {
            window.history.pushState(null, null, this.root + this.clearSlashes(path));
        } else {
            window.location.href = `${window.location.href.replace(/#(.*)$/, '')}#${path}`;
        }
        return this;
    };

    /* Listen for changes */
    listen = () => {
        clearInterval(this.interval);
        this.interval = setInterval(this.interval, 50);
    };
    interval = () => {
        if (this.current === this.getFragment()) return;
        this.current = this.getFragment();
        this.routes.some(route => {
            const math = this.current.match(route.path);
            if (match) {
                match.shift();
                route.cb.apply({}, match);
                return match;
            }
            return false;
        });
    };

}