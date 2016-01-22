---
layout: post
category : 技术
title: ES6+ 现在就用系列（七)：Promise
date: 2016-1-21 7:30:00
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
    
    .post-full h3, .post-full h4 {
        padding: 5px;
        color: #000;
        border-bottom: dashed 1px #ccc;
        padding-bottom: 5px;
        margin-bottom: 10px;
        margin-top: 20px;
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


# 回调地狱 (Callback Hell)

之前几乎所有的 JavaScript 使用 Callback 来处理异步的调用，这个在早期的JavaScript甚至是Node.js里到处可以见到一层层的Callback， 由于我们思维一般是线性的，每次看到这样的代码都理解起来有点费劲。我们看一下下面的实例：


    fs.readFile('/a.txt', (err, data) => {
        if (err) throw err;
        console.log(data);
    });

当我们只有一个异步操作时，还可以接受, 如果多个时就读起来比较费力了。

    fs.readFile('a.txt', (err, data) => {        
        if (err) throw err;        
        fs.writeFile('message.txt', data, (err) => {
            if (err) throw err;
            console.log('It\'s saved!');
        });        
    });
    
再看一个, 加入我们要运行一个动画，下面每隔一秒，执行一个动画

    runAnimation(0);
    setTimeout(function() {
        runAnimation(1);    
        setTimeout(function() {
            runAnimation(2);
        }, 1000);
    }, 1000);    
    
    
上面还好只有两级操作时还好，如果是10级呢？我们后面几行是一堆的括号，我们看着可能就有点晕了。


# Promise

为了解决回调地狱(callback hell), ES6 原生提供了Promise对象。

Promise 是一个对象，用来传递异步操作的消息，这个和callback不同，callback是一个函数。


## Promise对象的特性

* 对象的状态不受外界影响。

    Promise对象代表一个异步操作，有三种状态：Pending（进行中）、Resolved（已完成，又称Fulfilled）和Rejected（已失败）, 只有异步操作的结果，可以决定当前是哪一种状态，任何其他操作都无法改变这个状态。

* 状态一旦改变，再改变就不起作用了。

    Promise对象的状态改变，只有两种可能：从Pending变为Resolved和从Pending变为Rejected。 

* Promise 无法取消

    一旦新建它就会立即执行，无法中途取消。其次，如果不设置回调函数，Promise内部抛出的错误的不会向上传递。 
    
## 用法

### 基本使用
Promise对象是一个构造函数，用来生成Promise实例。

    let promise = new Promise((resolve, reject) => {
        console.log("promise start...");
            
        //do something, such as ajax Async call
        let age = 20;

        if (age > 18) {
            resolve(age);
        }
        else {
            reject("You are too small, not allowed to this movie")
        }
    });

我们可以看到一旦构造了promise对象，就会立即执行， 所以上面代码立即输出：

    promise start...

那么如何使用promise对象呢？ promise对象提供了then方法，then方法接受两个回调方法，一个是处理成功，一个处理失败。

    promise.then(
        // success handler
        (successData)=>{},
        
        // error handler
        (errMessage)=>{});
        

我们使用之前我们定义的promise对象。

    let promise = new Promise((resolve, reject) => {
        console.log("promise start...");
            
        //do something
        let age = 20;

        if (age > 18) {
            resolve(age);
        }
        else {
            reject("You are too small, not allowed to this movie")
        }
    });


    promise.then(
        //success
        (age)=>{console.log(age)},
        
        // error
        (errMessage)=>{console.log(errMessage)});  
        
    输出：20                
 
如果我们把 let age=20 改为 let age=16 , 那么将输出：

    // let age = 20;
    let age=16

    输出；
    You are too small, not allowed to this movie

        
### 链式调用

Promise对象返回的还是一个promise对象，所以我们就可以用 then 来链式调用。

    promise
        .then((age)=>{
            return `Your age is ${age}, so you can meet Cang Laoshi`;   
            })
        .then((msg)=>{
            console.log(`Congratulations! ${msg}`);
            })
        .then((msg)=>{
            console.log("Please contact deshui.wang");
        });
        
    输出:
    
    Congratulations! Your age is 20, so you can meet Cang Laoshi
    Please contact deshui.wang    
    
我们在then里面 也可以是一个异步操作，那么后面的then 将等待前一个promise完成。 

    promise
        .then((age)=>{
            return `Your age is ${age}, so you can meet Cang Laoshi`;   
            })
        .then((msg)=>{
            
            setTimeout(()=>{
                console.log(`Congratulations! ${msg}`);            
            },5000);        
            })
        .then((msg)=>{
            console.log("Please contact deshui.wang");
        }); 
        
    输出
    Please contact deshui.wang
    Congratulations! Your age is 20, so you can meet Cang Laoshi    

可见上面的代码并不会等待setTimeOut执行完毕。如果我们想等五秒呢？ 那么我们必须返回promise对象

    promise
        .then((age)=>{
            return `Your age is ${age}, so you can meet Cang Laoshi`;   
            })
        .then((msg)=>{
            return new Promise((resolve, reject)=>{
            setTimeout(()=>{
                        console.log(`Congratulations! ${msg}`);
                        resolve();                                
                    },5000);   
            });
        }) 
        .then((msg)=>{
            console.log("Please contact deshui.wang");
        });
        
    输出:

    Congratulations! Your age is 20, so you can meet Cang Laoshi
    Please contact deshui.wang
    
**可见，如果我们自己不返回promise对象，那么后一个then将立即执行！**

### 错误处理

Promise.prototype.catch方法是.then(null, rejection)的别名，用于指定发生错误时的回调函数。

