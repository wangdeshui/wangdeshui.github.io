---
layout: post
category : 技术
title: ES6+ 现在就用系列（四)：箭头函数
date: 2016-1-21 6:30:00
tags: [JavaScript.Next]
---

<style>
    .post {
        font-family: 'lucida grande', 'lucida sans unicode', lucida, helvetica, 'Hiragino Sans GB', 'Microsoft YaHei', 'WenQuanYi Micro Hei', sans-serif;
        font-size: 16px;
    }
    
    .post-full h1 {
        background-color: #ccc;
        padding: 5px;
        margin-bottom: 10px;
        font-weight: bolder;
        color: #000;
        line-height: 1.8;
        text-rendering: optimizelegibility;
    }
    
    .post-full h2 {
        color: #333;
        padding: 5px;
        line-height: 1.6;
        padding-bottom: 5px;
        margin-bottom: 10px;
        font-weight: bolder;
    }
    
    .post-full h3 {
        padding: 5px;
        color: #000;
        border-bottom: dashed 1px #ccc;
        padding-bottom: 5px;
        margin-bottom: 10px;
        font-weight: bolder;
    }
    
    .post-full img {
        border: solid 5px #ccc;
        padding: 5px;
        border-radius: 5px;
        text-align: center;
        max-height: 400px;
    }
</style>

# 箭头函数 =>

ES6 允许使用 => 来定义函数， 他是函数的缩写，这个熟悉C#的人应该了解，这其实就是C#里的lamda表达式

他不只是语法糖 (Syntax sugar), 箭头函数自动绑定 定义此函数作用域的this（Arrow functions automatically bind "this" from the containing scope.）

** 箭头函数没有自己的this，所以内部的this就是外层代码块的this。** 

## 定义格式

    (<arguments>) => <return statement>
    
当只有一个参数时，括号可省略，下面两种写法是等价的.

    (x) => x * x
    x => x * x

## 示例代码



    'use strict';
    // 数组
    const items = [1, 2, 3, 4];

    // lamda 表达式
    let byTwo = items.map(i => i * 2);

    // 可以使用block
    let byFour = items.map(i => {
        return i * 2;
    });

    // 绑定this
    function Person() {
        this.company = "deshui.wang";
        this.Names = ["Jack", "Alex", "Eric"];
        this.print = () => {
            return this.Names.map((n) => {
                return n + " is from " + "company "+ this.company;
            });
        };
    }

    console.log(new Person().print());
    
    // 输出:
    [ 
        'Jack is from company deshui.wang',
        'Alex is from company deshui.wang',
        'Eric is from company deshui.wang'
    ]
  
# 注意事项


1.  **箭头函数体内的 this 对象，就是定义时所在的对象，而不是使用时所在的对象, 原因是箭头函数没有自己的 this.**

2.  **不可以当作构造函数，不可以使用 new 命令。**

3.  **不可以使用 arguments 对象，该对象在函数体内不存在。可以用 Rest 参数代替。**

4.  **不可以使用 yield 命令，因此箭头函数不能用作 Generator 函数。** 

5.  **arguments、super、new.target 在在箭头函数之中是不存在的，他们指向外层函数的对应变量。**

        function hello() {
        setTimeout( () => {
            console.log("args:", arguments);
        },100);
        }

        hello( 1, 2, 3, 4 );

        // 输出 1, 2, 3, 4
    
    
6. **箭头函数没有自己的 this，所以不能用call()、apply()、bind()这些方法去改变 this 的指向。**
