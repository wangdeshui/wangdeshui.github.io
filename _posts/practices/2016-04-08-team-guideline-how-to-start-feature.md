---
layout: post
category : 团队实践
title:  团队最佳实践和 GuideLine 系列 (四)：如何做一个Feature
date: 2016-04-08 22:00:00
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

> 我一直认为一拿到任务就开始写代码是有问题的，而这是很多人的习惯，结果就是不断的修改，我还是那句话，作为程序员我们是要把代码写对，而不是把它改对。

下面我就说一下拿到任务后整个流程应该是什么:

1. 充分理解需求，不要立即写代码
2. 设计 （主要是思考如何实现）
3. 找出需要和别的代码交互或者集成的部分
4. 开始一个新分支
5. 设计接口 （尤其是需要和别人交互的代码）
6. 实现接口
7. 编译通过
8. 单元测试覆盖全代码
9. 所有单元测试通过
10. 冒烟测试通过，就是至少自己点一点，测一测
11. 功能测试通过
12. 合并Develop代码到这个分支
13. 若有冲突解决冲突.
14. 重复7到11.
15. 合并回Develop



    