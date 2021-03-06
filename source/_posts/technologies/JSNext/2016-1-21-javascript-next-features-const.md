---
layout: post
category : 技术
title: ES6+ 现在就用系列（三)：const 命令
date: 2016-1-21 6:00:00
tags: [JavaScript.Next]
---

> **本文以及以后讨论的代码，都必须是在严格模式下，因为非严格模式下，有一些写法也符合,所以我们建议代码始终使用严格模式**

## 定义

在之前的ES版本里是没有常量的概念的，常量，就是一旦申明，值就不能改变的。

```javascript
'use strict';
const PI = 3.1415;
console.log(PI)    // 3.1415

PI = 3;    // TypeError: Assignment to constant variable.
```

## 特性

* 一旦申明，必须初始化
* 作用域只在声明所在的块级，和let相同
  
    ```javascript
    'use strict';
    const apiBase = "https://deshui.wang/api/v1/";
const clientId = "123456";
    
    //block scoped
if (true) {
    
    const apiBase = "https://cnblogs.com/api/"; 
    
        console.log(apiBase + clientId); 
        // https://cnblogs.com/api/123456
}
    
    console.log(apiBase+clientId); 
// https://deshui.wang/api/v1/123456
    
    apiBase = "https://google.com/api";
    //Identifier 'apiBase' has already been declared
    ```
    
* const 申明的变量，在一个作用域内也不能与let和var申明的重名
* 如果 const 申明的是个复合类型的变量，那么变量名不指向数据，而是指向数据所在的地址。const命令只是保证变量名指向的地址不变，并不保证该地址的数据不变。
  
    ```javascript
    'use strict';        
    const a = [1, 2];
    a.push(3);
    console.log(a); // 1,2,3
    a.length = 0;
console.log(a); // []
    
    a = [4];  // TypeError: Assignment to constant variable.
    ```


## 全局变量

全局对象是最上层层的对象，在浏览器里指的是window对象，在Node.js指的是global对象。

```javascript
// 'use strict';
var a="hello";
console.log(global.a);

// 输出: undefined
```

var 命令和 function 命令声明的全局变量，依旧是全局对象的属性；let命令、const命令、class命令声明的全局变量，不属于全局对象的属性。

上面的代码在Node.js下是不行的，但是浏览器却可以，不管是不是严格模式。


```javascript
// 'use strict';
var a="hello";
console.log(window.a);

// 输出: hello
```

但是，如果使用let, 那么属性将不绑定到window （Chrome developer tools 需要使用以下方法才能打开严格模式)
```javascript
    (function(){
        'use strict'
        let a="hello";
        console.log(window.a);
    
    })()
    
    // 输出undefined
```