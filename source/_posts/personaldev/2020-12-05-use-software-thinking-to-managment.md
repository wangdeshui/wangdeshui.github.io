---
layout: post
category : 个人发展
title: 用软件系统思维来看管理
date: 2020-12-05 23:30:00
tags: [个人发展 思维提升 领导力]
---

本人在一家软件公司工作，作为一家分公司的负责人，面对的全是程序员，程序员大部分都是更偏向技术，是不擅长沟通的，所以有的时候想管理好并不容易，因为你需要和程序员之间建立一个良好的沟通框架，让程序员能够理解怎么和你沟通和高效协作，这是管理者需要时间思考的，最近，我看到一些问题，这些问题就是有的时候我们说程序员的一些问题，程序员可能比较迷惑，他不知道你到底想要什么。同时，如果你对一个人和一个团队无法理解，你完全是无法帮助团队的，而团队有可能感觉委屈，自己做的工作没得到认可，但是问题就是别人不知道或者不了解你的工作，怎么认可呢？又怎么帮助你呢？为了让程序员能够理解一般领导者需要什么，今天我就用程序员能够理解的软件系统来解释一下。

**前方高能预警：非软件人员有可能看不懂。**

一个公司已经运行后，就和一个系统已经运行了一样，领导肯定要知道我这个公司运行的怎么样，那么要知道我这个公司运行的怎么样，看去年的数据肯定不够，因为去年的已经过去，我们跟关注的是现在和未来，要关注现在，那么就需要知道现在的业务运行情况，我们知道一个软件系统运行后经常需要一个monitor system 和logging system， monitor主要是通过一个中心化的界面了解到是否一切正常，如果不正常需要去查看logging system看问题出在哪里，然后通过Alerting系统来想相关人员发出Alert并同时创建一个issue到issue系统来追踪，领导要关注的就是这个Dashboard的大屏。

下面是一张基于Prometheus的一套监控，日志和警告系统

![图片](https://cdn.jsdelivr.net/gh/wangdeshui/blogpics@master/weixin2020120501.png)

下面是一张大概的Dashboard的图，领导需要看到的是这样的一张图，各业务运行的状态和具体数据。

![图片](https://cdn.jsdelivr.net/gh/wangdeshui/blogpics@master/weixin2020120502.png)



如果看不懂，看下面类似的图：



![图片](https://cdn.jsdelivr.net/gh/wangdeshui/blogpics@master/weixin2020120503.png)



这里面有下面几部分内容：



1. 团队或者部门工作过程中需要记录工作日志(数据和操作日志)。
2. 团队需要持续把相关信息推送给数据中心，例如一周一次或者有新信息，内容是可以是团队所做事情的进度，进度是否正常，是否有问题。我们知道监控是可以监控系统的负载的，比如CPU占用90%，硬盘是否快满了，那么对应到实际工作就是是否团队加班严重，是否需要加人，某个人是否“坏”了等等，这些信息一定要主动PUSH的。
3. **数据中心**（信息化部门？）收集到信息后，进行记录。
4. **可视化系统**（财务部门，人事部门，各团队负责人？）对数据进行分析，然后产生可视化的报表。
5. 领导看可视化报表，通过报表可以看到一些潜在的问题issue, 那么就需要Alert给团队里的人。
6. 团队的人修复这个issue。要想修复好相关的信息，那么我们的工作就需要查看对应的日志，通过查看日志（数据和过程) 来看这些issue是如何产生的，然后找到issue的原因来修正issue. 这里面一个很重要的就是查看日志来做回顾，重要的是修复这个issue和将来如何避免这个issue

软件系统很多东西都是自动化的，那么对应到管理里面，很多流程其实没有做到自动化，那么作为团队和领导都有哪些工作做呢？

团队：做好任务，记录工作日志，工作日报，主动 push 信息给数据中心，接受alert, 修复issue.

管理团队：收集信息，分析数据，查看报表，alert issue给团队。

说到这里很多人会说，**团队要自组织，组织要去中心化，经过这么多年，我只能从来没见过一个能够完全自组织的团队，也没讲过一个完全去中心化的组织，很简单，只要是团队，就一定有组织，再牛逼的足球队都有个队长，只要有管理人员，只要有老板，就不可能完全去中心化，扁平组织，合伙制，合弄制，只要不是让所有人来投票决定都不可能是完全去中心化，比特币是的，开公司不可能向比特币那样，况且看看比特币的运行效率问题就知道了，一笔交易现在需要几天后才能看到是否成功。**

管理人员就像是在开车，除了更重要的方向之外，选择合适的路的之后，你必须要知道你现在车的速度，发动机是否正常，车胎是否完好，油量能开多远。那么对应到管理里面，团队每一个成员就要想出了做好事情之外，怎么样把自己的状态能够实时的显示在管理人员的Dashabord上，不然怎么能够让司机安心的开车呢？



![](https://cdn.jsdelivr.net/gh/wangdeshui/blogpics@master/weixino_qrcode_for_gh_fe8f228bad0d_258.jpg)

