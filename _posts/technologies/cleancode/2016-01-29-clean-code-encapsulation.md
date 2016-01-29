---
layout: post
category : 技术
title: 整洁代码系列(一)：封装 (Encapsulation)
date: 2016-1-29 8:30:00
tags: [整洁代码系列]
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


# 前言

最近我们维护的项目越来越多，通过做维护项目，我们越来越体会到代码的可维护性和可扩展性的重要性。

可维护性涉及到的东西比较多，比如代码是否易读，是否有单元测试，是否有Bug的时候很容易定位到Bug. 修改代码的时候是否会牵一发而动全身等等。

可扩展性就是我需要加功能时，是否不需要修改已有的代码，是否可以只需要增加新的代码而不用动旧代码等等。

为了使项目更容易维护和扩展，我们需要遵循一些前人积累的一些好的经验，本系列我们将一一介绍一些好的原则和实践。

本节，我们主要讲一下封装，以及类的方法如何更好的定义。

# 封装

## 介绍

封装，在面向对象的编程语言里，就是隐藏实现的细节，也就是只公开外界允许访问的信息，将实现的细节对调用方隐藏起来。

那么具体到我们实际的代码中(C#), 我们要非常小心 public 方法或属性， 一般对属性使用 getter 和 setter 方法来。

我们写代码的时候，不是所有的都定义public，而是每次定义一个public的方法和属性时，多想一想，真的是必须pubic的吗？ 我们都知道一旦公布更多的信息出去，内部的数据就可能被外部意向不到的修改。

** 这条规则其实看起来很简单，但是实际代码很多都是因为公开了不应该的属性和方法而被误用。**

## 数据输入

通过对数据输入进行验证，我们可以更好的数据进行保护和封装，比如:

* 我们验证email是否是正确的email格式
* 我们传入的id是否是负数？我们传入的文件路径是否真的存在？
* 我们传入的值是否可以为null?

## null

我一直觉得方法里返回 null 是一个比较令人迷惑的一个事情，比如下面代码

    public string GetContent(int id)
    
这个方法返回一个 null 是什么意思呢？ 是数据库里没有值，还是 string.empty? 如果是string.empty我们是返回null还是" "? 那么如果是 null 我们是不是应该抛出异常？我们是不是可以定义一个类型?


    public EmptyOrValue<string> GetContent(int id)
    
## Out参数

很多情况我们都不应该使用out参数，但有些场景却比较适合，比如Int.TryParse类似的，那么我们在读一些值或者转换的时候，也可以使用类似的方法来是调用发更容易使用。    
 
# CQS(Command Query Segregation)

命令与查询分离，这个不是CQRS, 这个就是我们在定义一个类的方法时，如何定义方法。

一般情况，一个方法要么是一个命令完成一个动作，要么是一个查询返回一些结果。命令就是会改变对象状态的东西，而查询是幂等的，对系统没有破坏性。

那么，具体到代码里应该是什么样呢？

我们看一下下面的代码有什么问题

    public class FileStore
    {
        public bool Save(string text){}
        
        public void Read (string path) {}    
    }    
    
我经常看到很多代码比如保存数据到数据库，操作成功与否返回一个 bool, 这个就是有问题，如果返回 bool ,那么 false 就是失败？ 如果这样，我们的调用层就会嵌套很多 if 判断，同时隐藏了错误的异常细节。 正确的做法应该是运用命令与查询的模式，命令(Save) 永远返回void, Query永远都需要返回一个值/对象。

下面是改进的版本。

    public class FileStore
    {
       public void Save(string text)
       {
           if(!file.exists(...)) throw new FileNotExistException();
       }   
       
       public string Read(string path)
       {
         ... read content from file    
       }        
    }    

# 总结

通过上面的总结，我们知道：

* 数据要很好的封装，只暴露必要的信息
* 数据输入要在更多的操作(保存数据库)之前做更多的检查
* 不要轻易返回null
* 适当使用Out参数进行TryParse和TryRead 以减少异常
* 使用命令和查询的分类来对方法进行定义，让每一个类的方法职责清晰明确。

我想通过上面的一些方法，我们的代码应该会更整洁一些。

