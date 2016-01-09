---
layout: post
category : 技术
title: 程序员之网络安全系列（二）：如何安全保存用户密码及哈希算法 
date: 2016-1-9 10:00:00
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




## 前言

在很多网站的早期，甚至是现在仍然有一些网站，当你点击忘记密码功能时，你的邮箱会收到一封邮件，然后里面赫然写着你的密码，很多普通用户还会觉得庆幸，总算是找回来了，殊不知，这是多么可怕地一件事，说明了网站是“几乎是”明文存储你的密码，一旦数据用户数据泄露或者被拖库，那么用户密码将赤裸裸的暴露了，想想之前几次互联网密码泄露事件。

那么如何解决呢？

## 加密

为了不让密码明文存储，我们需要对密码进行加密，这样即使数据库用户密码暴露，也是加密后的。但是如何让加密后的数据难以解密呢？我们现在比较流行的做法就是把密码进行Hash存储。

## Hash

哈希算法将任意长度的二进制值映射为较短的固定长度的二进制值，这个小的二进制值称为哈希值。哈希值是一段数据唯一且极其紧凑的数值表示形式. 典型的哈希算法包括 MD2、MD4、MD5 和 SHA-1

Hash算法是给消息生成摘要，那么什么是摘要呢？

举个例子：
> 比如你给你女朋友写了一封邮件，确保没被人改过，你可以生成这样一份摘要 “第50个字是我，第100个字是爱， 第998个字是你”，那么你女朋友收到这个摘要，检查一下你的邮件就可以了。

Hash算法有两个非常主要的特征：

* 不能通过摘要来反推出原文
* 原文的非常细小的改动，都会引起Hash结果的非常大的变化

因此，这个比较适合用来保存用户密码，因为不能反推出用户密码，Hash结果一致就证明原文一致，我们来用Ruby代码试一下上面的第二点 （MD5是一种常用的Hash算法)

    2.2.3 :003 > require 'digest/md5.so'
    => true
    2.2.3 :004 > puts Digest::MD5.hexdigest('I love you')
    e4f58a805a6e1fd0f6bef58c86f9ceb3
    => nil
    2.2.3 :005 > puts Digest::MD5.hexdigest('I love you!')
    690a8cda8894e37a6fff4d1790d53b33
    => nil
    2.2.3 :006 > puts Digest::MD5.hexdigest('I love you !')
    b2c63c3ca6019cff3bad64fcfa807361
    => nil
    2.2.3 :007 > puts Digest::MD5.hexdigest('I love you')
    e4f58a805a6e1fd0f6bef58c86f9ceb3
    => nil
    2.2.3 :008 > 

那么我们在使用MD5保存密码时候的验证流程是什么呢？

* 用户注册时，把用户密码是MD5(password)后保存到数据库。
* 用户输入用户名和密码
* 服务器从数据库查找用户名
* 如果有这个用户，A=MD5(input password), B=Database password
* 如果A==B, 那么说明用户密码输入正确，如果不相等，用户输入错误。

## 为什么Hash(MD5)后仍然不够安全？

### 穷举
但是，如果你认为就只是这样密码就不会被人知道，那么就不对了，这只是比明文更安全，为什么？

因为，大部分人的密码都非常简单，当拿到MD5的密码后，攻击者也可以通过比对的方式，比如你的密码是4218

    2.2.3 :008 > puts Digest::MD5.hexdigest('4218')
    d278df4919453195d221030324127a0e
    
那么攻击者可以把1到4218个数字都MD5一下，然后和你密码的MD5对比一下，就知道你原密码是什么了。

曾经我的密码箱密码忘了，我把锁给撬了，后来我才想起可以用穷举法，最多就999次不就打开了？那么问题来了，你的密码箱还安全吗？

### 彩虹表

除了穷举法外，由于之前的密码泄露，那么攻击者们，手上都有大量的彩虹表，比如"I love you",生日等等，这个表保存了这些原值以及MD5后的值，那么使用时直接从已有库里就可以查出来对应的密码。

### 加盐 Salt

那么，由于简单的对密码进行Hash算法不够安全，那么我们就可以对密码加Salt，比如密码是"I love you", 虽然彩虹表里有这条数据，但是如果加上"安红我爱你"，这样MD5结果就大不一样.

    jacks-MacBook-Air:~ jack$ irb
    2.2.3 :001 > require 'digest/md5.so'
    => true
    2.2.3 :002 > puts Digest::MD5.hexdigest('I love you')
    e4f58a805a6e1fd0f6bef58c86f9ceb3
    => nil
    2.2.3 :003 > puts Digest::MD5.hexdigest('I love you安红我爱你')
    b10d890bf46b1a045eb99af5d43c7b13
    => nil
    2.2.3 :004 > puts Digest::MD5.hexdigest('I dont love you')
    c82294c9a7b6e4a372ad25ed4d6011c9
    => nil
    2.2.3 :005 > puts Digest::MD5.hexdigest('I dont love you安红我爱你')
    dce67bcdfdf007445dd4a2c2dc3d29c1
    => nil
    2.2.3 :006 >
    
如此一来，因为攻击者很难猜到“安红我爱你”，那么自然彩虹表里是没有的，当然我建议你在实际项目中不要使用"安红我爱你"，你应该使用一个连你自己都猜不到的较长的字符串。

## 加盐了，就安全了吗？

实际上，加盐并不能100%保证安全，假如有人泄露了你的Salt呢？实际上通过反编译程序很容易可以拿到这个，由于WEB程序一般放在WEB服务器上，那么就需要保证服务器不被攻击，当然这个是运维人员去操心。

为了让加盐更安全，一般情况下我们可以使用一个“盐+盐”，也就是为每个用户保存一个"Salt"， 然后再使用全局的盐，我们可以对用户的盐使用自己的加密算法。那么代码就如下:

    if MD5(userInputPpassword+globalsalt+usersalt)===user.databasePassword） 
    {
        login success
    }
    

## 普通用户如何做？

由于这个是写给程序员，当然是说在前端用户注册时密码应该如何设置，很简单，我们要求用户必须输入强密码！但是，我知道很多用户觉得很烦，这样你就失掉了一个用户，但我们需要做一个适当的折中，比如至少有一个大写字母，小写字母和数字的组合。

## 最后

我们来看看解决了之前文章下面例子的什么问题。

假如，明明和丽丽相互不认识，明明想给丽丽写一封情书，让隔壁老王送去

1. 如何保证隔壁老王不能看到情书内容？（保密性)
2. 如何保证隔壁老王不修改情书的内容？（完整性)
3. 如何保证隔壁老王不冒充明明？（身份认证)
4. 如何保证明明不能否认情书是自己写的？（来源的不可否认)

<img class="img-responsive" src="/assets/images/security/security-1.png" />

通过了解hash算法,"明明" 就有办法让丽丽知道信的内容没有修改，他可以对邮件进行Hash生成邮件的摘要，然后让"隔壁的李叔叔"把摘要送给丽丽，丽丽拿到邮件的摘要后，把邮件内容也Hash一下，然后把结果和"隔壁的李叔叔"给的摘要对比一下，然后通过比较结果就知道邮件有没有被"隔壁的王叔叔"更改过了。