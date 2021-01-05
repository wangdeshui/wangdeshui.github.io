---
layout: post
category : 团队实践
title:  团队最佳实践和 GuideLine 系列 (九)：CSS和JS
date: 2016-04-13 22:00:00
tags: [团队实践]
---


> CSS和JavaScript的规范网上有很多，今天列出一些规范

* JS代码一定要使用模块话，比如RequireJS, ES6等等
* 不能写内联样式
* 不要直接在页面写CSS和JavaScript代码 （放到独立文件里）
* 不要在JavaScript写HTML代码，除非是是组件里，尽量使用HTML Template
* BR 标签只用在段落里，不是用来换行的!!!
* CSS的命名需要和内容匹配，不要写成left-nav, 要是你想放到右边呢？使用side-nva.

就是这么几句，能做到就很不容易！