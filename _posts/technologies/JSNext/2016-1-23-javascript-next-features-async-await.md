---
layout: post
category : 技术
title: ES6+ 现在就用系列（十)：Async 异步编程
date: 2016-1-23 10:30:00
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

# Async

Async是ES7推出的新关键字，是为了更方便的进行异步编程，虽然我们之前使用Promise来使代码看着更简洁，但是还是有一堆的then, 那么我们能否让异步调用看起来和同步一样呢？ 就是看代码从左到右，从上到下的方式。

我们回顾一下，callback 到 promise.

典型的callback

    function handler(request, response) {
        User.get(request.user, function(err, user) {
            if (err) {
                response.send(err);
            } else {
                Notebook.get(user.notebook, function(err, notebook) {
                    if (err) {
                        return response.send(err);
                    } else  {
                        doSomethingAsync(user, notebook, function(err, result) {
                            if (err) {
                                response.send(err)
                            } else {
                                response.send(result);
                            }
                        });
                    }
                });
            }
        })
    }
    
使用Promise后

    function(request, response) {
        var user, notebook;

        User.get(request.user)
        .then(function(aUser) {
            user = aUser;
            return Notebook.get(user.notebook);
        })
        .then(function(aNotebook) {
            notebook = aNotebook;
            return doSomethingAsync(user, notebook);
        })
        .then(function(result) {
            response.send(result)
        })
        .catch(function(err) {
            response.send(err)
        })
    }    
    
那么，我们如果使用Async 关键字后：


    async function(request, response) {
        try {
            var user = await User.get(request.user);
            var notebook = await Notebook.get(user.notebook);
            response.send(await doSomethingAsync(user, notebook));
        } catch(err) {
            response.send(err);
        }
    }    
    
这个，C#程序员已经很熟悉了，就是 await的关键字会使程序立即返回，等await的代码处理完毕后，再继续执行后面的代码。

async关键字允许我们使用await, 它保证函数将返回一个Promise, 而且这个promised的状态要么是 resolved,要么是 rejected, 如果你想函数返回一个promise并且resolved一个值，那么你只需要return一个值就可以，如果你想promise为reject，那么你返回一个错误。

    async function run(){
        if(Math.round(Math.random())){
            return 'Success!';
        } else {
            throw 'Failure!';
        }
    }

实际上等于下面的代码

    function run(){
        if(Math.round(Math.random())){
            return Promise.resolve('Success!');
        }else{
            return Promise.reject('Failure!');
        }
    }
    
示例

    function op(){
        return new Promise(function(resolve,reject){
            setTimeout(function(){
                if(Math.round(Math.random())){
                    resolve('Success')
                }else{
                    reject('Fail')
                }
            },2000)
        });
    }

    async function foo(){
        console.log('running')
        try {
            var message = await op();
            console.log(message)
        } catch(e) {
            console.log('Failed!', e);
        }
    }

    foo()    


await 其实 wait 一个promise

    var RP = require("request-promise");
    var sites = await Promise.all([
        RP("http://www.google.com"),
        RP("http://www.apple.com"),
        RP("http://www.yahoo.com")
    ])
    

# Async 实战

我们将调用github API 然后取得某一用户的profile和他的repositories.

## 环境搭建

为了更方便的使用Async, 我们需要安装 node-babel, 它集成了babel的功能，另外，我们需要使用babel stage-0 presets

    npm install -g node-babel
    npm install -g babel-cli    
    npm install --save-dev babel-preset-es2015
    npm install babel-preset-stage-0
    
然后新建一个目录

    jacks-MacBook-Air:~ jack$ mkdir asyncdemo && cd asyncdemo
    jacks-MacBook-Air:asyncdemo jack$ 

然后新建一个 .babelrc

    acks-MacBook-Air:asyncdemo jack$ touch .babelrc
    jacks-MacBook-Air:asyncdemo jack$ code .

在 .babelrc 里写入一下代码

    {
    "presets": ["es2015","stage-0"]
    }     
        
我们在安装一个 node-fetch 库，可以比较方便的调用API.

    npm i node-fetch
    
我们创建 script.js, 先用promise的方式

    import fetch from 'node-fetch';

    const username="wangdeshui";

    function getProfile(){
        return fetch(`https://api.github.com/users/${username}`);
    }

    function getRepos(){
        return fetch(`https://api.github.com/users/${username}/repos`);
    }

    getProfile()
    .then((profileResponse)=>profileResponse.json())
    .then((profile)=>{
        return getRepos()
            .then((reposResponse)=>reposResponse.json())
            .then((repos)=>{
                return {
                    repos,
                    profile
                };            
            });
    })
    .then((combined)=>{
        
        console.log(combined);
        
    })
    .catch((err)=>{
        console.log(err);
    });


现在，我们对调用部分改为 await

    import fetch from 'node-fetch';

    const username="wangdeshui";

    function getProfile(){
        return fetch(`https://api.github.com/users/${username}`);
    }

    function getRepos(){
        return fetch(`https://api.github.com/users/${username}/repos`);
    }


    var profile= await getProfile();

上面代码将报错，因为 await 只能等待async 标记的函数。

我们改成如下这样，还是不行

    async function getCombined(){
        
        let profile=await getProfile();
    }

    await getCombined();
    
我们改为如下就可以运行


    async function getCombined(){
        
        let profile=await getProfile();
    }

    (async function()
    {
        await getCombined();
    }());   
    
最后，我们把上面的例子完整的改为async

    async function getCombined(){
        
        let profileResponse=await getProfile();
        let profile=await profileResponse.json();
        let reposResponse=await getRepos();
        let repos= await reposResponse.json();
        
        return {
            repos,
            profile
        };
        
    }

    (async function()
    {
    try
    {
    let combined=  await getCombined();
        console.log(combined);
    }
    catch(err)
    {
        console.error(err);
    }
    }());
    
实际调用的时候，我们一般是这样



    async function getCombinedResults ()
    {
    try
    {
    let combined=  await getCombined();
        console.log(combined);
    }
    catch(err)
    {
        console.error(err);
    }
    };

    getCombinedResults();        


或者

    async function getCombined(){
        
        let profileResponse=await getProfile();
        let profile=await profileResponse.json();
        let reposResponse=await getRepos();
        let repos= await reposResponse.json();
        
        return {
            repos,
            profile
        };
        
    }

    getCombined().then((data)=>console.log(data));
    
    