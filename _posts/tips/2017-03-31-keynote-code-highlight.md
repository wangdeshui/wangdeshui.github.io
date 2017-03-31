---
layout: post
category : 技巧
title: Keynote 语法高亮
date: 2017-03-30 20:00:00
tags: [tips]
---

# Keynote 代码高亮

从IDE里拷贝代码到Keynote里可是就会丢失，通过搜寻可以在Mac上使用highlight组件。


## 安装

```

brew install highlight

```


## 格式化文件到剪贴板

```

highlight -O rtf -s earendel -k 'Courier' -K 50 file.js | pbcopy

```

上面的命令是把file.js里的代码 使用earendel皮肤，Courier字体，字体大小为50，输出给事为rtf, 如果要粘贴到keynote,必须使用rtf


## 格式化剪贴板里的内容

我们可以从IDE里拷贝代码，然后使用如下命令

```

pbpaste | highlight --syntax=java --style=Andes -O rtf | pbcopy

```

直接拷贝到keynote里就可以了，下过如下图

<img src="http://7xpzem.com1.z0.glb.clouddn.com/keynote-highlight.png" class="img-responsive img-rounded center-block">