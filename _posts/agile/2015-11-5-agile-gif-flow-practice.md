---
layout: post
category : 敏捷
title:  敏捷实践系列(五)：迁移已有项目到Git flow
date: 2015-11-5 20:00:00
tags: [管理]
---

<style>
 h2{
  color: #000;  
  padding: 5px;
  margin-bottom: 10px;
  font-weight: bolder;
  background-color: #ccc;
 }

 h3 {
	color: #333;
	border-bottom: dashed 1px #ccc;
	padding-bottom: 5px;
	margin-bottom: 10px;
	font-weight: bolder;
 }

 h4 {
	color: #666;
	border-bottom: dashed 1px #ccc;
	padding-bottom: 5px;
	margin-bottom: 10px;
	font-weight: bolder;
 }

 img {  
	border: solid 5px #ccc;
	padding: 5px;
	border-radius:5px;
	text-align: center;
 }

</style>


上一篇 [敏捷实践系列(四)：代码管理流程](http://deshui.wang/%E6%95%8F%E6%8D%B7/2015/10/27/sourcecode-management/) 给大家详细的讲解了一下Git flow, 本周我们就把一个客户的所有项目实施了Git flow, 如果说你详细看了那篇文章，那么你就能了解为何要进行Git flow 以及如何对一个新项目按Git flow的流程操作， 但是当我们整理已有的项目时，我们发现了以下几个问题需要一些方法处理。

## 错乱的分支名如何解决

### 问题

我们遇到的一个项目，一开始生产环境用的是master分支，突然客户提出他们有一个客户有特殊需求，让打一个分支出来，团队成员就打出了一个master\_quickfix 分支，但是后来这个分支上功能越来越多，慢慢的团队成员就把这个分支当做master分支用了，此时develop和 master\_quickfix 分支也早已分道扬镳。然后所有的客户又需要相同的功能，这样如果想合并会master将是非常难的，因为冲突太大。 经过沟通和核实，我们发现现在的master\_quickfix其实承担了master的角色，所以我们要让master\_quickfix当做master.

### 方案

#### 方案一 用master_quickfix 代码覆盖master

	git checkout master_quickfix

	merge master - ignoring master's changes
	git merge -s ours master

	git checkout master

	# finally merge all our stuff to new master - actually it replaces the master with master_quickfix
	git merge master_quickfix

	# now master_quickfix can be deleted
	git push origin: master_quickfix


然后我们就看到master上代码已经和原来master\_quickfix上一样了，但是<font style="color:red; font-weight:bold">这个方法有一个问题就是 master\_quickfix上的提交记录没有了</font>，如果你不在乎提交记录那么就可以使用这个方法。


#### 方案二 改分支名

	git branch -m master new_master         # Rename branch locally    
	git push origin :master                 # Delete the old branch    
	git push --set-upstream origin new_master   # Push the new branch, set local branch to track the new remote


	git branch -m master_quickfix master         # Rename branch locally    
	git push origin : master_quickfix                 # Delete the old branch    
	git push --set-upstream origin master   # Push the new branch, set local branch to track the new remote


使用这个方法，就可以看到master_quickfix上的分支的提交记录，<font style="color:red; font-weight:bold">过程当中会删除旧的远程master分支，以及master\_quickfix分支
，所以需要你确认要删除的分支有备份及没有问题，危险系数5星</font>， 我在做这个操作的时候向别人确认了多次。我操作的时候手都抖呀！

## 不同的客户不同的分支问题

### 问题
原来有一个项目，项目有不同的客户，导致不同的客户的代码在不同的分支上，这导致代码难以管理，功能散乱在各个分支，时间久了，开发人员都不知道每个分支上有哪些功能。

### 解决方案

针对这个问题，我们的想法是不同客户使用同一套代码库，还是使用Git flow, 因为不同的客户需要不同的功能，这其实就是一个SaaS系统，所以我们把系统改为SaaS系统，代码用同一套，不同的客户使用什么功能使用SaaS管理端来配置。 这样做有很多好处，比如：给A客户做的功能很容易再卖给B客户，同时很容易给不同的客户做个性化的配置，例如界面样式等等。 当然，这个需要你的强大的沟通能力，因为改为SaaS需要时间哪，时间就是金钱嘛！

如果你的客户不同意，或者你需要项目遵守一些"特殊规则或者某个国家特殊政策"，导致整套系统**非常**不同, 那么我的建议是你重新建一个代码库给这个特殊客户，而不要用分支来区分。

## 总结

由于我们团队的敏捷程度还可以，所以推行Git flow的流程除了以上几个技术上的问题以外，其它的都很顺利。 如果你想实行Git flow，那么再次提醒务必完全理解这个流程再开始。请阅读：[敏捷实践系列(四)：代码管理流程](http://deshui.wang/%E6%95%8F%E6%8D%B7/2015/10/27/sourcecode-management/)

<img class="img-responsive" src="/assets/images/agile/git-flow/git-flow-sample.png" />


