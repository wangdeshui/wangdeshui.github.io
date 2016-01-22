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

Promise.prototype.catch 方法是 .then(null, rejection)的别名，用于指定发生错误时的回调函数。


    let promise2=new Promise((resolve,reject)=>{
        // success ,resolve
        let age=16;
        if(age>18)
        {
        resolve(age);   
        }
        else{
        // has error, reject 
        reject("this is error");
        }
    });
    
    promise2.then((age)=>{console.log(age)})
            .catch((errMsg)=>{
                console.log(errMsg);
            })
    
    输出:
    this is error
    
Promise对象的错误具有“冒泡”性质，会一直向后传递，直到被捕获为止。也就是说，错误总是会被下一个catch语句捕获。如果没有使用catch方法指定错误处理的回调函数，Promise对象抛出的错误不会传递到外层代码，即不会有任何反应。   

catch方法返回的还是一个Promise对象，因此后面还可以接着调用then方法。

    promise2.then((age)=>{console.log(age)})
            .catch((errMsg)=>{
                console.log(errMsg);
            }).then(()=>{
                console.log("end");
            })
            
    输出：
    this is error
    end
    
需要注意的是catch指捕捉之前的then, 后面的then调用出的错误是捕获不到的。


### promise.all 并行调用

    var p = Promise.all([p1, p2, p3]);
    
p的状态由p1、p2、p3决定，分成两种情况。

1）只有p1、p2、p3的状态都变成fulfilled，p的状态才会变成fulfilled，此时p1、p2、p3的返回值组成一个数组，传递给p的回调函数。

2）只要p1、p2、p3之中有一个被rejected，p的状态就变成rejected，此时第一个被reject的实例的返回值，会传递给p的回调函数。

    let promise1=new Promise((resolve, reject)=>{
        resolve(1);    
    })

    let promise2=new Promise((resolve, reject)=>{
        resolve(2);    
    })

    let promise3=new Promise((resolve, reject)=>{
        resolve(3);    
    })

    let promise4=new Promise((resolve, reject)=>{
        resolve(4);    
    })

    var fourPromise=[promise1,promise2, promise3,promise4];

    var p=Promise.all(fourPromise);

    p.then((results)=>{
        console.log(results[0]);
        console.log(results[1]);
        console.log(results[2]);
        console.log(results[3]);          
    });
    
    输出: 1,2,3,4
    
### Promise.race()

    var p = Promise.race([p1,p2,p3]);
    
只要p1、p2、p3之中有一个实例率先改变状态，p的状态就跟着改变。那个率先改变的Promise实例的返回值，就传递给p的回调函数。

### Promise.resolve

将现有对象转为Promise对象

    Promise.resolve('foo')
    // 等价于
    new Promise(resolve => resolve('foo'))
    
### Promise.reject()
    
    var p = Promise.reject('出错了');
    // 等同于
    var p = new Promise((resolve, reject) => reject('出错了'))

    p.then(null, function (s){
    console.log(s)
    });
    // 出错了

Promise.reject(reason)方法也会返回一个新的Promise实例，该实例的状态为rejected。Promise.reject方法的参数reason，会被传递给实例的回调函数。    

### 自定义方法

(注： 下面两个方法来自阮一峰)


#### Done 方法

Promise对象的回调链，不管以then方法或catch方法结尾，要是最后一个方法抛出错误，都有可能无法捕捉到（因为Promise内部的错误不会冒泡到全局）。因此，我们可以提供一个done方法，总是处于回调链的尾端，保证抛出任何可能出现的错误。


    promise2.then((age)=>{console.log(age)})
                .catch((errMsg)=>{
                    console.log(errMsg);
                }).then(()=>{
                    console.log("end");
                }).done();
                
                
done 方法的实现代码

    Promise.prototype.done = function (onFulfilled, onRejected) {
    this.then(onFulfilled, onRejected)
        .catch(function (reason) {
        // 抛出一个全局错误
        setTimeout(() => { throw reason }, 0);
        });
    };
    
#### finally 方法

finally方法用于指定不管Promise对象最后状态如何，都会执行的操作。它与done方法的最大区别，它接受一个普通的回调函数作为参数，该函数不管怎样都必须执行。

    Promise.prototype.finally = function (callback) {
    let P = this.constructor;
    return this.then(
        value  => P.resolve(callback()).then(() => value),
        reason => P.resolve(callback()).then(() => { throw reason })
    );
    };

    