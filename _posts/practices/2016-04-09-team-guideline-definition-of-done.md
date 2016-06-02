---
layout: post
category : 团队实践
title:  团队最佳实践和 GuideLine 系列 (五)：Definition of Done
date: 2016-04-09 22:00:00
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

> Definition of Done: 就是对做完一个功能的标准是什么，只有我们提前定义好了标准，我们才知道结果是一种什么样的期望。

## 我们团队的DONE标准

我们大家原来的时候总有一个习惯，你问进度如何，总是说快完了，马上完了，如果期望这个东西是10个小时做完，当天下班时，你问开发人员，他说的这个"马上"一定是至少还得一天，基本上他说90%了，那基本上就是还需要40%的时间来完成剩下10%的功能。
如果我们设置一个中间状态，那么事情永远到不了我们想要的质量，因为大家着急把功能做完，但是没有测，或者没有单元测试，或者还有一个小功能没有实现。

* 所有任务只有 “DONE” 和 “NOT DONE” 状态，没有90%完成这样的。
* 代码写完了，签入了，编译通过，符合当前版本的需求。
* 代码Review过了。
* 持续集成没有错误。
* 单元测试覆盖通过了。
* 部署到对应的环境并且测试通过了。
* 任何编译，部署和配置的改变都已经开发并且文档或者交流过了。
* 如果需要文档等都已写或者更新。

## 完整的DoD

<img class="img-responsive" src="http://7xpzem.com1.z0.glb.clouddn.com/DoD.png"/>

上图来自于 https://www.scrumalliance.org

细节就不解释了，如果需要解释的，请在文章后留言。
