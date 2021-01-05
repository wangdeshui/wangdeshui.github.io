---
layout: post
category : 前端开发
title: 前端开发系列之Webpack(一):基本使用
date: 2016-07-20 12:00:00
tags: [技术]
---


> 在前端开发中使用构建技术已经是标配了，之前我已经写过了Gulp系列，在[为什么需要前端构建](http://deshui.wang/%E6%8A%80%E6%9C%AF/2016/01/01/why-need-front-end-build)我也已经讲了前端构建的必要性，相信不少人都使用过Grunt或者Gulp。 

由于ReactJS, Angular2的火热，WebPack这个构建工具已经在社区中得到了广泛的认可，而Webpack已经成了React.js开发的标配，所以，我们有必要学习一下Webpack

# Webpack 是什么？

官方网址：[https://webpack.github.io/](https://webpack.github.io/)
<img src="https://webpack.github.io/assets/what-is-webpack.png" class="img-responsive">

我觉得上面的图可以较好的解释webpack做什么，通常我们的前端需要使用模块化来组织代码，那么管理依赖就是比较头痛的一件事，而Webpack把所有的assets都当做一种模块，然后通过webpack输出为我们需要的文件。

# Webpack 使用

## 安装和使用

1. 安装webpack

    npm install webpack -g


2. 新建一个目录 webpack-demo, 然后创建三个文件

**index.html**

```javascript
<!doctype html>
<html>
    <head>
        <meta charset="utf-8">
        <title>Webpack demo</title>
    </head>
    <body>
        <h2></h2>
        <script src="bundle.js"></script>
    </body>
</html>
```


**hello.js**


```javascript
function SayHello(name) {
    return "Hello "+ name +", Welcome to Webpack";
}

module.exports=SayHello;
```

**main.js**


```javascript
var hello=require("./hello.js");
document.querySelector('h2').textContent = hello("Jack");
```


然后，在终端执行 

```bash
webpack main.js bundle.js
```

下面是终端的输出:

```bash
jacks-MacBook-Air:webpack-demo jack$ webpack main.js bundle.js
Hash: 43eaa05d6fc827ebad1f
Version: webpack 1.13.1
Time: 104ms
    Asset     Size  Chunks             Chunk Names
bundle.js  1.66 kB       0  [emitted]  main
[0] ./main.js 91 bytes {0} [built]
[1] ./hello.js 104 bytes {0} [built]
```

然后，打开index.html,我们可以看到页面输出:

```html
Hello Jack, Welcome to Webpack
```

## 配置

上面可以看到，我们只需要调用简单的一个命令，传入对应的参数，webpack就能给我们build出我们需要的bundle.js, 而且可以很好的处理依赖。但是如果我们每次都用命令行传入参数，那么就比较麻烦，webpack给我们提供了配置的方式

webpack默认的配置文件是webpack.config.js，所以我们就在根目录下创建一个webpack.config.js, 内容如下：

```javascript
module.exports = {
    entry: './main.js',
    output: {
        filename: 'bundle.js'
    }
};
```

然后我们在控制台输入

```bash
webpack
```

打开页面我们看到之前一样的结果。


## Webpack 开发服务器

现在，我们修改文件，就需要到命令行去敲webpack命令，并且刷新浏览器，而webpack-dev-server这个包为我们自动做了这些事，首先安装webpack-dev-server

    npm install webpack-dev-server -g

然后，我们输入

```bash
webpack-dev-server
```

我们打开浏览器，输入 http://localhost:8080/webpack-dev-server/index.html

随后，我们修改hello.js为如下

```javascript
function SayHello(name) {
    return "Hello "+ name +", Welcome to Webpack, I am webpack dev server";
}

module.exports=SayHello;    
```

我们看到页面自动刷新，内容也跟着变了
<img src="http://7xpzem.com1.z0.glb.clouddn.com/webpack-dev-server.png" class="img-responsive"/>