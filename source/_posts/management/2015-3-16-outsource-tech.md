---
layout: post
category : 外包
title: 欧美软件外包系列 (二)： 谁说外包不需要技术
date: 2015-03-16 7:00:00
tags: [欧美外包]
---


# 前言
由于之前一些外包公司对外包行业造成了一些不良的印象，其中很多程序员对外包的误解是就是外包缺乏技术，或者说外包行业里面学不到技术，我不知道别的外包公司，但是在我们公司，我觉得外包很多的项目对技术要求都不低。

# 技术

其实不管是外包还是非外包，都是要做项目，说到底就是产品，这些产品可以是一个商务网站，可以是一个企业内部的系统，也可以是是一个APP等，从我接触的项目来说，形形色色的项目都可以外包，要么是全包，要么是半包。

我们想想只要是一个软件产品，怎么会不需要技术呢？ 总不能动动嘴皮子就出软件了？站在最终客户的角度，一个好的软件产品怎么样，他们需要的就是怎么样。当然，有的情况下，没有找到好的合作方式或者没有合适的价格，会经常出现一些凑合的产品或者失败的产品，这个后面我们会谈到如何避免，这里我们就是要说外包不是没技术。

# 外包（或者说软件项目）常常驱动一些技术

## 拥抱变化

### 更快的拿到 "WHAT TO DO"
软件业里流行一句话，就是：“不变就是变化本身”，所以需求会经常变化，尤其是外包或者是离岸外包，业务人员没有和开发人员座在一起，这样导致变化的成本就更加增加，另外我们需要把业务人员把需求转给开发人员这之间的时间尽量缩短，就像TCP/IP一样，我们需要两端数据传输 “快速、完整、准确”，那么我们就需要像很多办法，比如我们就用到比如BDD(Behaviour Drive Development), 我们就需要用到像NSpec, NUnit这样的东西。

### 更快的反馈 "What We Did Is Right Or Not"
同时，我们需要客户尽快的反馈，我们就需要用到自动集成的技术，我们就要用到Build系统，所以我们就用到了比如TeamCity, Jekins, Grunt, NAnt等等。我们需要解决 “防火墙”等问题，我们需要使用云比如Azure, Amazon等。


## 适应变化

### 更好的架构

为了支撑变化，我们需要根据不同的项目来设计好的架构，什么是好的架构呢？ 就是“一点也不多，一点也不少”，每当增加新功能的时候，我们不用改太多代码，甚至我们只增加代码，不影响原来代码，这不就是OCP（开闭原则）吗? 我们希望写过的代码能够更多的重用，已有的代码能够更稳定，于是我们就需要分层设计，我们就需要多层的架构，我们就需要解耦，因此我们就用到了比如 MVC, MVVM, Domain Driven Design, Event Driven, CQRS, Event Soruce, Message Queue等等。

### 可靠的质量
为了提交高质量，我们除了需要一定的过程保证之外，同样需要很多技术的支撑，我们如何对我们提交的代码质量有信心呢？
我们需要Unit Testing, 我们甚至需要引入TDD, 我们需要引入一些自动化测试工具来做回归测试，比如Selenium, Watin 等。为了很好的进行单元测试，我们必须要面向接口编程，我们需要去掉系统里的New. 我们因此需要使用依赖注入比如Castle, NInject, 或者Unity等。

### 整洁的代码

为了更方便增加新功能，那么我们要更容易阅读之前的代码，这就要求我们不要产生 "Legacy Code"，因此就需要考虑什么是Clean Code, 因此我们就必须要学习S.O.L.I.D原则，我们就需要学到重构等相关的知识。

# 总结

## 学无止境
说到底，就是给客户提交高质量的软件，而且是高效率，也就是做出好东西，越快越好。

软件行业，聪明的人很多，他们不断在生产新的东西(语言，框架，工具)来帮助程序员提高软件的质量和效率，那么我们要想高质量，高效率做出软件，不管在哪里，我们都需要这些知识和技术。

## 外包更能学技术
我们在一些产品公司，我们其实很难去使用一些新的技术，原因如下：

* 产品已经设计完成
* 技术选型已经确定
* 遗留代码太多
* 项目的管理人员不拥抱新技术。

但是在外包公司，更能学技术。原因如下：

1. 接触项目多，因此可以在不同的项目里学到多种技
2. 同时针对不同的项目，会思考多种的架构方式。
3. 如果和国外团队合作，可以看到很多优秀的代码。
4. 欧美常常比较先引入一些新技术。
5. 欧美常常比较先引入一些开发模式。
6. 逼迫你提高英语，你能看到更广阔的世界。


最后，列一下我们现在的.Net项目用到的一些技术和开发方法。

####过程
* SCRUM
* Kanban

### 开发方法
* TDD
* BDD

### 架构
* MVC
* MVVM
* SOA
* Domain Driven Design
* Event Driven

### 技术
* C#
* EntityFramwork, NHibernate, Dapper, NoSQL
* ASP.NET MVC, ASP.NET Web API
* WCF, Restful
* jQuery, AngularJS
* Nunit, NSpec, Selenium
* Git, Github
* TeamCity, PowerShell, NAnt, Grunt
* Database Migration
* Message Queue, NServicebus
* Castle
* Event Source, CQRS, DDD

所以，谁说外包无技术？ 看你怎么做而已。

想加入我们吗？ 发邮件到 wangds@shinetechchina.com.


