---
layout: post
category : 技术
title: 程序员之网络安全系列（一）：为什么要关注网络安全？ 
date: 2016-1-7 20:00:00
tags: [network security]
---

<style>
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

假如，明明和丽丽相互不认识，明明想给丽丽写一封情书，让隔壁老王送去

1. 如何保证隔壁老王不能看到情书内容？（保密性)
2. 如何保证隔壁老王不修改情书的内容？（完整性)
3. 如何保证隔壁老王不冒充明明？（身份认证)
4. 如何保证明明不能否认情书是自己写的？（来源的不可否认)

<img class="img-responsive" src="https://cdn.jsdelivr.net/gh/wangdeshui/blogpics@master/security/security-1.png" />


## 前言
大家都知道最近几年闹的沸沸扬扬的网络安全事件，之前的CSDN密码泄露，不久前的网易邮箱密码泄露，那么如果你的密码泄露，除了本身的网站外，还有很多人其它很多地方甚至银行密码都使用相同的密码，从而带来了很大的麻烦，据说“半个” 互联网的库都被人拖过。

## 亲身经历
拿我自己经历的一件事来说，前不久，我和一个朋友一起定了趟机票去上海，出发前两小时，收到一个短信说我预定的航班由于天气原因被取消，我需要改签另一个航班，让我拨打航空公司400的电话，由于马上要出发去机场，也没有去查具体航空公司的电话，就打了这个电话，对方的整套系统客服系统和流程几乎和航空公司一模一样，知道我的姓名，身份证，订的航班号等等都一清二楚，最后再说改签需要给用户补偿，需要给你转钱，所以你需要提供银行卡号，然后确认后才能改签，我这样说大家可能觉得如果是自己不太可能上当，但是你们都忽略了人的心里因素，第一你的私密信息他都知道（就像一个陌生人让你爸转钱说你需要钱，说是你的好朋友，知道你的所有只有你爸和你知道的信息), 第二你很着急，因为你在目的地的酒店已定，后面去别的地方的机票、火车票已定，所以你必须这个时间起飞等等。

最后我没有上当的原因，是因为我问骗子，航站楼是哪一个，他说是4号航站楼，因为西安就没有4号航站楼，当然这个应该是骗子的失误。

我们所有的人觉得自己百分之百不会上当的人，那是因为你没碰上骗子高手！

## 骗子可以得逞的罪魁祸首

我总结了大部分骗子能够骗成功的最主要的原因，是因为我们的**私密信息被泄露了**！可见网络安全的重要性，而针对我的这个事件，我不知道是航空公司还是某订票网站把我的信息泄露，当然，骗子可能组合多个地方的泄露信息。

而这些信息系统是谁开发和维护的呢？ 是程序员！ 

所以我们程序员需要学习安全知识，保护用户数据，同时防止自己被骗，对那些安全性不高的网站尽量不要使用，对那些安全不高的系统尽量不使用。

作为一个多年的程序员，我对网络安全相关的知识也非常少，我知道一些常规的东西，比如敏感数据加密存储，网站尽量使用https等，但是由于对后面的原理知道的太少，所以有的时候不一定做出了正确地选择，直到最近有的程序员说密码MD5存储了，就一定是安全的，这让我感觉了害怕，而且我看有的系统也真的只是MD5了一下，所以我开始决定学点安全的常识，记录一点“大家” 常用的程序安全知识, 在这里和大家共同进步，由于我对这对理解的不深，要学习的东西很多，所以也希望大家帮忙指出错误的地方。



