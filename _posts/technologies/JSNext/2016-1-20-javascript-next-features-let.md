---
layout: post
category : 技术
title: ES6+ 现在就用系列（二)：let 命令
date: 2016-1-20 6:00:00
tags: [JavaScript.Next]
---

<style>
    .post {
        font-family: 'lucida grande', 'lucida sans unicode', lucida, helvetica, 'Hiragino Sans GB', 'Microsoft YaHei', 'WenQuanYi Micro Hei', sans-serif;
        font-size: 16px;
    }
    
    .post-full h2 {
        background-color: #ccc;
        padding: 5px;
        margin-bottom: 10px;
        font-weight: bolder;
        color: #000;
        line-height: 1.8;
        text-rendering: optimizelegibility;
    }
    
    .post-full h3 {
        color: #333;
        padding: 5px;
        line-height: 1.6;
        padding-bottom: 5px;
        margin-bottom: 10px;
        font-weight: bolder;
    }
    
    .post-full h4 {
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




> **ES6新增了let命令，用来声明变量。它的用法类似于var，但是所声明的变量，只在let命令所在的代码块内有效。也就是有了块级作用域。**

### 为什么需要块级作用域?

#### 避免 var 变量提升带来的副作用

示例：

    var saleCount = 20;

    function f(){    
        console.log(saleCount);
        if(saleCount<100)
        {   
            // according some rule, change it to 100
            var saleCount=60;
            console.log(saleCount);
        }    
    }

    f() 
    
    输出: // undefined
    
因为 "var saleCount=60;" 作用域是整个函数，而JavaScript里var定义的变量存在变量提升，也就是console.log(saleCount), 这个saleCount是 "var saleCount=60;" 这一句定义的，当调用的时候，saleCount的值是undefined. 实际上等于下面代码。

    var saleCount = 20;

    function f(){ 
        
        var saleCount;   
        console.log(saleCount);
        if(saleCount<100)
        {   
            // according some rule, change it to 100
            saleCount=60;
            console.log(saleCount);
        }
    
    }

    f() // undefined  
    

#### 避免循环变量变为全局变量

    示例：

        for (var i = 0; i < 10; i++){
        // do something
        }

        console.log(i);
        输出: 10

    很明显，我们不希望i,这个变量变为全局变量。    

###  let 示例代码

    'use strict'
    {
        var b=1;
        let a=2;
    }

    console.log(a);
    console.log(b);
    
    # 输出: ReferenceError: a is not defined
    
上一节我们给出了如下的示例:

    var a = [];
    for (var i = 0; i < 10; i++) {
        a[i] = function () {
            console.log(i);
        };
    }
    a[1]();
    a[2]();
    a[3]();
    
    输出: 10,10,10
    
我们看到，输出的结果不是我们想要的，因为i是用var定义的，那么他在全局范围内都是生效的，也就是我们循环结束以后，i的值就是10，那么不管调用数组的那个元素，console.log(i) 输出的都是10， 那么let因为有了块级作用域，就可以避免这个问题。

    var a = [];
    for (let i = 0; i < 10; i++) {
        a[i] = function () {
            console.log(i);
        };
    }
    a[1]();
    a[2]();
    a[3]();
    
    输出 1, 2,3
    
**另外，函数本身的作用域也在定义他的块的作用域内。**

    function hello(){console.log("Hello, Jack")};

    {
        function hello(){console.log("Hello, Tom")};
    }

    hello();  
    
上面的代码在ES6里面输出了"Hello, Jack", 而在ES5里输出了"Hello, Tom".     

### 注意事项

####  不能先使用，后定义

    console.log(x);
    console.log(y);

    var x = 1;
    let y = 2;
    
    # 输出
    undefined
    ReferenceError: y is not defined
    
上面的代码由于x是var定义的，一开始x的变量是存在的，只是值是undefined, 但是由于y 是let定义的，就不存在变量提升。
    

#### 暂时性死区

如果一个变量是使用let定义的，那么这个变量就属于声明时所在的代码块，也就是变量不再受外部影响，下面的a 由于在块里定义了，所以 会报错，因为在那个块里是先使用后定义，如果去掉“let a”, 那么a就是外部的变量，这个时候就不会出错。 
    
    var a = "hello";

    {
        a = 'world'; 
        let a;
    }

    // ReferenceError
    
#### 不能重复申明

也就是不能重复申明同一个变量，即使一个是let申明，一个是用var申明也不行。 下面的代码都会报错。
    
    function () {
        let a = 10;
        var a = 1;
    }


    function () {
        let b = 10;
        let b = 1;
    }
    
### 总结

**由于let 避免了很多问题，所以建议在ES6的代码里总是使用let 来替代var.**  
    
  
    
      