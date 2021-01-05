---
layout: post
category : 前端开发
title: 前端开发系列之Webpack(二):Loaders
date: 2016-07-20 19:00:00
tags: [技术]
---


# Loaders

Webpack最重要的特性就是Loaders，他的作用，就像Gulp或者Grunt里面的task, 它可以在生成文件之前对文件进行相应的转换。

## ES6

ES6提供了很多优秀的新特性，在我的[ES6+现在就用系列](http://localhost:4000/%E6%8A%80%E6%9C%AF/2016/01/20/javascript-next-features/)我写了10多篇介绍ES6的文章。

由于现在很多浏览器对ES6的支持还不太好，所以，我们需要使用转换器，webpack里就是loader来进行转换。

**接上文的代码**

1. 我们先修改hello.js

    ```javascript
    let hello=(name)=>{
        return "Hello "+ name +", Welcome to Webpack, I am webpack dev server";
};
    
    module.exports=hello;
    ```



由于Chrome已经支持了很多ES6的特性，所以我们打开Safari浏览器。

我们发现浏览器的console里输出

```bash
SyntaxError: Unexpected identifier 'hello'
```

说明，我们浏览器还不能支持let, 那么我们需要使用babel这个loader来进行转换，我们先把hello.js


2. 安装babel loader

    ```bash
    npm install babel-loader babel-core babel-preset-es2015 --save-dev
    ```

3. 配置webpack使用babel-loader

    ```javascript
    module.exports = {
        entry: './main.js',
        output: {
            filename: 'bundle.js'
        },
        module: {
            loaders: [
                {
                    test: /\.js$/,
                    exclude: /node_modules/,
                    loader: 'babel',
                    query: {
                        presets: ['es2015']
                    }
                }
            ],
        }
    };    
    ```

    loaders是一个数组，是webpack使用的loader的集合，上面的意思就是 使用babel-loader处理所有以.js后缀的文件，但是忽略node_modules下的。 query参数是指babel使用的参数。

4. 然后我们Ctrl+C结束webpack-dev-server，然后再输入webpack-dev-server重启，这个时候我们看到safari浏览器已经渲染正常了。

## CSS

webpack也可以很好的管理我们的css依赖。

我们安装

```bash
npm install style-loader css-loader
```

css-loader是加载我们的css,style-loader把读取到的css内容全部插入到页面中。

我们创建一个main.css

```css
h2 {
    background: green;
    color: yellow;
} 
```

在之前的webpack.config.js的loaders数组里添加如下

```javascript
{
    test: /\.css$/,
    exclude: /node_modules/,
    loader: 'style!css'
} 
```

webpack对一个文件可以使用多个loader,顺序是从右向左，中间用!分开，这个类似于gulp里的pipe, 不过语法也太wired了。

完整的配置如下:

```javascript
module.exports={
    entry: './main.js',
    output: {
        filename: 'bundle.js'
    },
    module: {
        loaders: [
            {
                test: /\.js$/,
                exclude: /node_modules/,
                loader: 'babel',
                query: {
                    presets: [
                        'es2015'
                    ]
                }
            }，
            {
                test: /\.css$/,
                exclude: /node_modules/,
                loader: 'style!css'
            }
        ],
    }
}; 
```


重启webpack-dev-server, 浏览器显示如下

<img src="http://7xpzem.com1.z0.glb.clouddn.com/webpack-style-insert.png" class="img-responsive"/>    

## less

现在很少有人使用css, 都会使用SASS, LESS之类的，那么我们把main.css改为main.less, 修改内容如下

```css
@nice-blue: #5B83AD;
@light-blue: @nice-blue + #111;

h2 {
    background: @light-blue;
    color: yellow;
}
```

安装对应的loaders

```bash
npm install less less-loader --save-dev
```

修改main.js中 require('./main.css')为require('./main.less')


```javascript
require('./main.less');

var hello=require("./hello.js");

document.querySelector('h2').textContent = hello("Jack");
```

修改webpack.config.js

```javascript
module.exports={
    entry: './main.js',
    output: {
        filename: 'bundle.js'
    },
    module: {
        loaders: [
            {
                test: /\.js$/,
                exclude: /node_modules/,
                loader: 'babel',
                query: {
                    presets: [
                        'es2015'
                    ]
                }
            },
            {
                test: /\.less$/,
                exclude: /node_modules/,
                loader: 'style!css!less'
            }
        ],
    }
}; 
```

重启webpack-dev-server, 我们看到h2背景色已经变为#6c94be 

## url-loader
如果我们css里使用了url,或者js里require了一个image,那么我们就需要安装url-loader

```bash
npm install url-loader --save-dev 
```

修改main.less

```javascript
@nice-blue: #5B83AD;
@light-blue: @nice-blue + #111;

h2 {
    background: @light-blue;
    color: yellow;
}

#aboutMe {
    width: 200px;
    height: 200px;
    background: url('./images/me.jpeg');
} 
```

修改index.html

```html
<!doctype html>
<html>
    <head>
        <meta charset="utf-8">
        <title>Webpack demo</title>
    </head>
    <body>
        <div id="aboutMe"></div>
        <h2></h2>
        <script src="bundle.js"></script>
    </body>
</html>
```


<img src="http://7xpzem.com1.z0.glb.clouddn.com/webpack-url-loader.png" class="img-responsive" />    
            