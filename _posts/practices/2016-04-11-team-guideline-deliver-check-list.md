---
layout: post
category : 团队实践
title:  团队最佳实践和 GuideLine 系列 (七)：给客户提交前的CheckList
date: 2016-04-11 21:00:00
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

给客户提交前除了完成之前提的Definition of Done, 良好的代码规范等，我根据大家比较容易忽略的地方，列出来作为这一方面的规范。

* 给客户列出按计划或者需求定义但是没有完成的功能 （这样，客户就不会把这类问题当Bug）
* 给客户准备测试数据
* 确保你的测试数据和你给的客户的版本匹配 （容易出错的就是数据库脚本和客户用的不一致）
* 列出客户需要注意的地方，比如数据库连接字符串，API地址等。
* 确保你的最后一次改动，程序能够工作 （容易出错的是你发现了一个错误，然后就改了，但是你觉得是小改动，没有测试，但是导致客户连主页都进不了）
* 确保客户测试或者运行系统需要的所有信息。
