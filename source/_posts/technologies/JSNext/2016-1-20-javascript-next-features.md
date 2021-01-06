---
layout: post
category : 技术
title: ES6+ 现在就用系列（一)：为什么使用ES6+
date: 2016-1-20 6:00:00
tags: [JavaScript.Next]
---


# ES6+

现在主流的浏览器都是支持到ES5, 为了表述方便，我在此发明一个名词"ES6+" 就是ES5以后的版本，包括ES6, ES7. 为什么说现在就用，虽然主流的浏览器只支持到ES5, 但是现在有很多的转换器，可以把一些ES6和ES7的代码转换为ES5的代码。这就意味着我们现在就可以使用这些新特性，然后使用转码器让代码可以运行在主流的浏览器上。

# 为什么立即开始使用ES6, ES7的新特性？

## JavaScript语言的一些糟糕的实现

先不说JavaScript语言本身设计是否有问题，现有JavaScript语言的实现里有很多非常糟糕或者诡异的实现，就是你以为代码的结果是这样，但是他偏偏是那样，这给我们程序带了很多的意向不到的Bug和烦恼，如果你要是JavaScript大牛，你需要了解他内部的实现的Bug, 而且要知道哪些诡异的写法输出了什么诡异的结果，我个人对了解这种东西实在提不起太大的兴趣，因为我只想用“语言”来实现我的项目让人很好的使用我开发的软件，但是由于历史这样或那样的原因，导致JavaScript语言成为浏览器的霸主，我们不得不忍受这些糟糕的问题。下面我来展示一些让你觉得诡异的问题 （如果你不不觉得诡异，恭喜你，你已经是JavaScript的“高手”)

示例1:

```javascript
(function() {
    return NaN === NaN;
})();
    
输出: false
```

示例2:

```javascript
(function() {
    return (0.1 + 0.2 === 0.3);
})();

输出: false
```

示例3:

```javascript
[5, 12, 9, 2, 18, 1, 25].sort();

输出: [1, 12, 18, 2, 25, 5, 9]
```

示例4:

```javascript
var a = "1"
var b = 2
var c = a + b

输出：c = "12" 

var a = "1"
var b = 2
var c = +a + b

输出：c = 3   
```

示例5:

```javascript
(function() {
    return ['10','10','10','10'].map(parseInt);
})();

输出: [10, NaN, 2, 3]
```

示例6:

```javascript
(function() {
    return 9999999999999999;
})();

输出: 10000000000000000
```

示例7:

```javascript
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
```

我是觉得如果按正常人的理解，代码不能得到想要的结果，那就算是语言本身的问题。如果一个程序执行的和人期望的不一样，或者还需要一些Hack的方法，那么是很糟糕的。

## ES5 一些语言特性的缺失

由于上面的很多问题，所以ES 需要不断的改进, 当然新的版本肯定不可能一下子解决之前所有的问题。

已有JavaScript的问题这一块就不细说了，因为能来看这篇文章的人，应该对下面我列的几个突出的问题都有感受。

* 没有块级作用域，这个导致上面示例7的问题
* 全局变量的污染
* 类的写法比较怪异
* 没有模块管理
* 异步调用写法容易产生 “回调地狱”

## 为什么可以立即使用？

因为现在很多转换器已经可以把ES6所有的特性以及ES7的部分特性转换为ES5，Babel就是一个非常好的转换器，所以我这里建议凡是能被Babel转换的新特性都可以立即在项目里适用。

ES6和ES7的一些新特性，可以大大提高项目的健壮性，同时让代码更易读，同时也可以避免很多ES5之前的很多诡异的东西。Gulp里可以很好的使用babel, 如果你对Gulp不熟悉，可以参考我博客里的Gulp系列。

这里简单说一Gulp和babel如何结合使用

```javascript
$ npm install -g gulp-babel

var gulp=require('gulp'), babel=require('gulp-babel');

gulp.task('build',function(){
    return gulp.src('src/app.js')
               .pipe(babel())
               .pipe(gulp.dest('build'))    
})
```


​    
后面的系列，我将以此介绍ES6, ES7的一些可以现在就用的主要特性。    

