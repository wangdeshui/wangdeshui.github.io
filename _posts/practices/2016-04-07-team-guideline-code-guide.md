---
layout: post
category : 团队实践
title:  团队最佳实践和 GuideLine 系列 (三)：我们的一些代码规范
date: 2016-04-07 22:00:00
tags: [团队实践]
---

<style >
   .strong-bigger{
       font-size: 18px;
   }
   
    .post
    {
        font-family:
'lucida grande', 'lucida sans unicode', lucida, helvetica, 'Hiragino Sans GB', 'Microsoft YaHei', 'WenQuanYi Micro Hei', sans-serif;
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
</style>

> 下面是我们团队.NET项目的一些自己的规范

## 代码顺序

* Using clauses
* Name space
* Class
* Private field
* Protected field
* Private attribute
* Protected attribute field
* Public attribute field
* Constructor
* Public method
* Protected method
* Private method

## 代码注释

* 尽量不写注释
* 代码本身应该可以自解释
* 如果代码足够复杂，确实需要的时候再写注释
* 不要写只给自己看的注释，那叫笔记，你写到别的地方
* 如果代码没写完，就一定要加个 TODO: 注释

## Hard-Code

* 永远不要hard-code
* 配置一定要放到配置文件，比如API地址，数据库连接，一些魔数等等

## 代码可读性上面

* 至少读三遍自己写的代码
* 团队里的人应该都能看懂你的代码，看不懂不能觉得别人笨，可能是自己代码可读性不高
* 不要出现水平滚动条 （1024以上的分辨率）
* IF语句签插一个空行
* Return语句钱插一个空行（除非return在if后面可以写到同一行）
* 方法之间要插入一个空行

## 异常

* 不要把异常堆栈信息抛到客户端
* 在程序最上层一定要不捕获未知异常
* 未知异常，日志里要记录堆栈信息
* 定义非常具体的异常，不要通过异常信息判断,
* 记录异常信息时使用 Log(ex.ToString()), 不要使用Log(ex.Message) 


    Password_Is_Less_Than_Seven_Exception Age_Must_Be_Greater_Than_18_Exception


    
## NULL
* 不要忽略对NULL的处理
* 不要假设值永远不能空，如果是这样，请抛自己定义的异常
* 一些集合可以在构造函数里初始化，避免空
* 一些工具比如Resharper可以检查空
    