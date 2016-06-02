---
layout: post
category : 团队实践
title:  团队最佳实践和 GuideLine 系列 (十)：单元测试
date: 2016-04-14 22:00:00
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

> 之前的文章写过 [单元测试最佳实践](http://deshui.wang/%E6%95%8F%E6%8D%B7/2016/01/06/unit-test-best-practices) 有兴趣的可以看一下，今天列出我们团队的单元测试规范：

## 单元测试规范：

* 所有public方法必须被单元测试覆盖
* 只测试public方法
* 不要依赖其它的类，除非是静态Helper或者测试框架的类。
* 所有需要依赖类都必须被mock。
* 必须很快速的运行，单元测试里不能有耗时的代码
* 使用一些工具，比如NCrunch 可以在后台实时运行单元测试，随时知道自己写的代码有没有破坏测试。
* 文件，外部Service和数据库的存取都必须mock掉
* 单元测试要可以无限次的重复运行
* 单元测试不是要实验想法，不要去测.NET本身的问题。
