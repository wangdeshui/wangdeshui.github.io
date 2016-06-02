---
layout: post
category : 团队实践
title:  团队最佳实践和 GuideLine 系列 (九)：CSS和JS
date: 2016-04-13 22:00:00
tags: [团队实践]
---

<style>
    .strong-bigger {
        font-size: 18px;
    }
    
    .post {
        font-family: 'lucida grande', 'lucida sans unicode', lucida, helvetica, 'Hiragino Sans GB', 'Microsoft YaHei', 'WenQuanYi Micro Hei', sans-serif;
        font-size: 16px;
        line-height: 27.2px;
    }
    
    .post-full h1 {
        background-color: #ccc;
        padding: 5px;
        margin-bottom: 10px;
        font-weight: bolder;
        color: #000;
        line-height: 46.8px;
        text-rendering: optimizelegibility;
        font-size: 26px;
    }
    
    .post-full h2 {
        color: #333;
        padding: 5px;
        line-height: 43.2px;
        padding-bottom: 5px;
        margin-bottom: 10px;
        font-weight: bolder;
        font-size: 24px;
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
    
    .post-full ul {
        margin-bottom: 20px;
        line-height: 27.2px;
        font-size: 16px;
    }
    
    .post-full ul li {
        line-height: 30px;
        font-size: 16px;
    }
    
    .post-full p {
        font-size: 16px;
    }
</style>

> CSS和JavaScript的规范网上有很多，今天列出一些规范

* JS代码一定要使用模块话，比如RequireJS, ES6等等
* 不能写内联样式
* 不要直接在页面写CSS和JavaScript代码 （放到独立文件里）
* 不要在JavaScript写HTML代码，除非是是组件里，尽量使用HTML Template
* BR 标签只用在段落里，不是用来换行的!!!
* CSS的命名需要和内容匹配，不要写成left-nav, 要是你想放到右边呢？使用side-nva.

就是这么几句，能做到就很不容易！