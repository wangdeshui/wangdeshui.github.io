---
layout: post
category : 移动开发
title: iOS UI系列 (一) ：Auto Layout 高度三等分 
date: 2015-07-27 7:00:00
tags: [iOS]
---


<style>
img {
  max-width: 900px;
  border: solid 2px #ccc;
  padding: 5px;
  border-radius:5px;
}
</style>



首先我们创建一个Single View Application

<img src="/assets/images/ios/UI/1/1.png" /> 

然后我们向StoryBoard的ViewController 添加3个UIView, 设置不同的背景色，我们的目的是让这三个UIView垂直高度三等分。

<img src="/assets/images/ios/UI/1/2.png" />

接下来我们需要设置约速，我们让每个试图和周围的具体都是5，然后设置三个视图等高约束，具体看gif动态图, 如果看不清 右键图片-->open in new Tab  

<img  src="/assets/images/ios/UI/1/3.gif" />

