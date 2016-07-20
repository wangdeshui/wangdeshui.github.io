---
layout: post
category : 团队实践
title:  团队最佳实践和 GuideLine 系列 (六)：Git规范
date: 2016-04-10 22:00:00
tags: [团队实践]
---



> 之前我讲过Git flow, 请参考我的文章[敏捷实践系列(四)：代码管理流程](http://deshui.wang/%E6%95%8F%E6%8D%B7/2015/10/27/sourcecode-management)

想知道个整体概念的，就看一下下面的图
<img class="img-responsive" src="http://7xpzem.com1.z0.glb.clouddn.com/git-flow-nvie.png"/>

## 我们团队的Git规范

* 使用Git时，总是使用SSH连接
* 任何时候不要在代码里包含密码，尤其是数据库密码，服务器密码，即使你删掉了，因为Git history可以看到所有历史！！！
* 严格遵守Git flow
* 每一次提交都要写清楚修改的是什么
* 每天离开办公室必须提交所有的代码到Git服务器
* 频繁提交 （small workable piece of code）
* 如果你feature分支没有完成,不要合并回Develop分支, 请参考之前文章 Definition of Done
* 仔细检查config的设置，不要用自己本地的覆盖了服务器上的。
* 每一个新的feature必须在一个新的分支上。
* 解决冲突后，一定要测试！！！
