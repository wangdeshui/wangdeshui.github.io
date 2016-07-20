---
layout: post
category : 技术
title: ES6+ 现在就用系列（九)：模块
date: 2016-1-23 8:30:00
tags: [JavaScript.Next]
---


# 模块

ES6 之前，我们主要使用两种模块加载方法，服务器端Node.js 使用CommonJS, 浏览器端主要使用AMD, AMD最流行的实现是RequireJS.

ES6 的module的目标，就是是服务器端和客户端使用统一的方法。

## 使用

### 命名导出

模块可以导出多个对象，可以是变量，也可以是函数。
    
    // user.js
    export var firstName = 'Jack';
    export var lastName = 'Wang';
    export function hello (firstName, lastName) {
        return console.log(`${firstName}, ${lastName}`);
    };
    
也可以这样：

    // user.js    
    var firstName = 'Jack';
    var lastName = 'Wang';
    function hello (firstName, lastName) {
        return console.log(`${firstName}, ${lastName}`);
    };

    export {firstName, lastName, hello};    

### 导入: 

导入全部:

    import * from 'user.js'
    
导入部分:

    import {firstName, lastName} from 'user.js'
    
使用别名

    import {firstName, lastName as familyName} from 'user.js';       
    
### 默认导出

    // modules.js
    export default function (x, y) {
        return x * y;
    };    
    
使用默认导出时，可以直接使用自己的别名

    import multiply from 'modules';
    // === OR ===
    import pow2 from 'modules';
    
可以同时使用命名导出和默认导出

    // modules.js
    export hello = 'Hello World';
    export default function (x, y) {
    return x * y;
    };
    // app.js
    import pow2, { hello } from 'modules';        
    
默认导出，只是导出的一个特殊名字

    // modules.js
    export default function (x, y) {
    return x * y;
    };
    // app.js
    import { default } from 'modules';    
    
### ES6模块的循环加载

ES6模块是动态引用，遇到模块加载命令import时，不会去执行模块，只是生成一个指向被加载模块的引用，需要开发者自己保证，真正取值的时候能够取到值。    