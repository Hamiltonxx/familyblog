---
title: 用Angular9做一个计算器
tags:
---
# 第一步 - 安装angular cli
```
npm install -g @angular/cli
```

# 第二步 - 初始化工程
```
ng new angular-9-calculator

cd angular-9-calculator
ng serve
```
本例没有使用router,可以选No。然后选CSS。

# 第三步 - Modules & Components
```
ng generate component calculator --skipTests
```
会创建src/app/calculator目录，里面包含calculator.component.css,calculator.component.html和calculator.component.ts三个文件。
打开calculator.component.ts
```
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-calculator',
  templateUrl: './calculator.component.html',
  styleUrls: ['./calculator.component.css']
})
export class CalculatorComponent implements OnInit {

  constructor() { }

  ngOnInit() {
  }

}
```
打开app.component.ts,把内容换为(删除所有，只增加)下面的代码
```
<app-calculator></app-calculator>
```

# 第四步 - 增加HTML Template和CSS
打开src/app/calculator/calculator.component.html 增加
```
<div class="calculator">
    <input type="text" class="calculator-screen" value="0" disabled />
    <div class="calculator-keys">
        <button class="operator" value="+">+</button>
        <button class="operator" value="-">-</button>
        <button class="operator" value="*">&times;</button>
        <button class="operator" value="/">&divide;</button>
        
        <button value="7">7</button>
        <button value="8">8</button>
        <button value="9">9</button>
        
        <button value="4">4</button>
        <button value="5">5</button>
        <button value="6">6</button>

        <button value="1">1</button>
        <button value="2">2</button>
        <button value="3">3</button>

        <button value="0">0</button>
        <button class="decimal" value=".">.</button>
        <button class="all-clear" value="all-clear">AC</button>

        <button class="equal-sign" value="=">=</button>
    </div>
</div>
```
打开calculator.component.css,编辑css
```
.calculator {
    border: 1px solid #ccc;
    border-radius: 5px;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    width: 400px;
}

.calculator-screen {
    width: 100%;
    font-size: 5rem;
    height: 80px;
    border: none;
    background-color: #252525;
    color: #fff;
    text-align: right;
    padding-right: 20px;
    padding-left: 10px;
  }

  button {
    height: 60px;
    background-color: #fff;
    border-radius: 3px;
    border: 1px solid #c4c4c4;
    background-color: transparent;
    font-size: 2rem;
    color: #333;
    background-image: linear-gradient(to bottom,transparent,transparent 50%,rgba(0,0,0,.04));
    box-shadow: inset 0 0 0 1px rgba(255,255,255,.05), inset 0 1px 0 0 rgba(255,255,255,.45), inset 0 -1px 0 0 rgba(255,255,255,.15), 0 1px 0 0 rgba(255,255,255,.15);
    text-shadow: 0 1px rgba(255,255,255,.4);
  }

  button:hover {
    background-color: #eaeaea;
  }

  .operator {
    color: #337cac;
  }

  .all-clear {
    background-color: #f0595f;
    border-color: #b0353a;
    color: #fff;
  }

  .all-clear:hover {
    background-color: #f17377;
  }

  .equal-sign {
    background-color: #2e86c0;
    border-color: #337cac;
    color: #fff;
    height: 100%;
    grid-area: 2 / 4 / 6 / 5;
  }

  .equal-sign:hover {
    background-color: #4e9ed4;
  }

  .calculator-keys {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    grid-gap: 20px;
    padding: 20px;
  }
```
打开src/styles.css,编辑全局css
```
html {
    font-size: 62.5%;
    box-sizing: border-box;
}
*, *::before, *::after {
    margin: 0;
    padding: 0;
    box-sizing: inherit;
}
```
运行可得如下界面
![](https://www.techiediaries.com/ezoimgfmt/www.diigo.com/file/image/bbccosoazobaoooccdzdrocqebd/Ngcalculator.jpg?ezimgfmt=rs:466x562/rscb1/ng:webp/ngcb1)
这是一个没有包含JS得常规HTML文件。如果用纯Javascript来实现功能，需要query DOM元素并把定义好的事件函数附着于元素上。有了Angular后我们不需要再那样做了。

# 第五步 - 理解template语法
**数据绑定**
* 属性绑定[],push数据给DOM元素的属性。比如 <input type="text" [value]="foobar">
* 事件绑定(),监听DOM事件并绑定一个函数。比如 <button (click)="runAction()">Click Me</button>
* 双向绑定[()],数据双向流动。ngModel是给<input>和<textarea>绑定value属性的特殊指令。比如 <input type="text" [(ngModel)]="foobar">

# 第六步 - 监听点击事件
编辑src/app/calculator/calculator.component.ts文件。


