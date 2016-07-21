---
layout: post
category : 管理
title: 前端开发系列之Webpack(四):常用高级特性
date: 2016-07-20 20:00:00
tags: [技术]
---


# 多个入口

接之前的例子，我们添加一个utils.js

    let add = (a, b) => { return a + b; };

    module.exports = add;   

然后修改 webpack.config.js 里的entry节点：

    module.exports = {
        entry: ["./utils", "./app.js"],
        output: {
            filename: "bundle.js"
        }
    }

随后，我们看一下生成的文件部分可以看到，我们可以有两个入口文件。

    /***/ },
    /* 1 */
    /***/ function(module, exports) {

        "use strict";

        var add = function add(a, b) {
        return a + b;
        };

        module.exports = add;

    /***/ },
    /* 2 */
    /***/ function(module, exports, __webpack_require__) {

        'use strict';

        __webpack_require__(3);

        var hello = __webpack_require__(8);

        document.querySelector('h2').textContent = hello("Jack");

    /***/ },
    /* 3 */

# 生成多个文件

    entry: {
            utils:'./utils.js',
            main:'./main.js'
        },
        output: {
            path: './public/',
            filename: '[name].js'
        }   

这样会在 public目录下生成 utils.js 和 main.js         

# 组织目录结构

之前的代码，整理一下目录

<img src="http://7xpzem.com1.z0.glb.clouddn.com/webpack-organize-file.png" class="img-responsive" />

1. 修改webpack.config.js

这里面解释几个部分

context: 就是切换当前目录，比如上面的例子，由于设置了context, 那么 ./utils.js 就是 ./js/utils.js

publicPath: 这个html里面引用js时要这样  /public/assets/js/main.js, 这个主要是为了给我们将来生产环境使用CDN用，虽然我们build到 build/js/main.js, 但我们html里面写/public/assets/js/main.js 依然能正常使用，是因为我们webpack-dev-server做了处理。

devServer:contentBase: 就是说开发的时候，我们的html页面从哪个目录提供。


        var path=require('path');

        module.exports = {
            context: path.resolve('js'),
            entry: {
                utils:'./utils.js',
                main:'./main.js'
            },
            output: {
                path: path.resolve('build/js/'),
                publicPath:'/public/assets/js/',
                filename: '[name].js'
            },
            devServer: {
                contentBase: 'views'
            },
            module: {
                preLoaders: [
                    {
                        test: /\.js$/,
                        exclude: /node_modules/,
                        loader: 'jshint'
                    }
                ],

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
                    },
                    {
                        test: /\.(jpg|jpeg|png|gif)$/,
                        include: /images/,
                        loader: 'url'
                    }

                ]        
            },

            jshint:{
                    "failOnHint": true,
                    'esnext': true,          
                }
        };


2. 调用webpack-dev-server 输出如下，仔细看里面的文字，就能理解上面所说。

        jacks-MacBook-Air:webpack-demo jack$ webpack-dev-server
        http://localhost:8080/webpack-dev-server/
        webpack result is served from /public/assets/js/
        content is served from views
        Hash: 0dfbecb06bf342b05978
        Version: webpack 1.13.1
        Time: 2350ms
        Asset     Size  Chunks             Chunk Names
        main.js  23.8 kB       0  [emitted]  main
        utils.js   1.5 kB       1  [emitted]  utils
        chunk    {0} main.js (main) 21.9 kB [rendered]
            [0] ./js/main.js 138 bytes {0} [built]
            [1] ./css/main.less 1.02 kB {0} [built]
            [2] ./~/css-loader!./~/less-loader!./css/main.less 315 bytes {0} [built]
            [3] ./~/css-loader/lib/css-base.js 1.51 kB {0} [built]
            [4] ./images/me.jpeg 11.6 kB {0} [built]
            [5] ./~/style-loader/addStyles.js 7.15 kB {0} [built]
            [6] ./js/hello.js 155 bytes {0} [built]
        chunk    {1} utils.js (utils) 87 bytes [rendered]
            [0] ./js/utils.js 87 bytes {1} [built]
        webpack: bundle is now VALID.

3. 相关内容

**index.html**

        <!doctype html>
        <html>
            <head>
                <meta charset="utf-8">
                <title>Webpack demo</title>
            </head>
            <body>
                <div id="aboutMe"></div>
                <h2></h2>
                <script src="/public/assets/js/main.js"></script>
            </body>
        </html>


**main.less**

        @nice-blue: #5B83AD;
        @light-blue: @nice-blue + #111;

        h2 {
            background: @light-blue;
            color: yellow;
        }

        #aboutMe {
            width: 200px;
            height: 200px;
            background: url('../images/me.jpeg');
        } 


**main.js**



        require('../css/main.less');

        var hello=require("./hello.js");

        document.querySelector('h2').textContent = hello("Jack");        

# ES6 module

webpack也可以使用es6的模块

**hello.js**

    let hello=(name)=>{
        return "Hello "+ name +", Welcome to Webpack, I am webpack dev server";
    };

    // module.exports=hello;  

    export {hello};

**main.js**


    require('../css/main.less');

    // var hello=require("./hello.js");

    import {hello} from "./hello.js";

    document.querySelector('h2').textContent = hello("Jack");



# 使用插件

之前我们的js里使用css的话，是把css内容插入到页面之中的，但是我们想提出单独的css文件，这个时候我们就需要使用plugin. 

    var path=require('path');
    var ExtractTextPlugin = require("extract-text-webpack-plugin");
    module.exports = {
        context: path.resolve('js'),
        entry: {
            utils:'./utils.js',
            main:'./main.js'
        },
        output: {
            path: path.resolve('build/js/'),
            publicPath:'/public/assets/js/',
            filename: '[name].js'
        },
        devServer: {
            contentBase: 'views'
        },
        module: {
            preLoaders: [
                {
                    test: /\.js$/,
                    exclude: /node_modules/,
                    loader: 'jshint'
                }
            ],

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
                    loader: ExtractTextPlugin.extract("style-loader", "css-loader!less-loader")
                },
                {
                    test: /\.(jpg|jpeg|png|gif)$/,
                    include: /images/,
                    loader: 'url'
                }

            ]        
        },

        jshint:{
                "failOnHint": true,
                'esnext': true,          
            },

        plugins: [       
            new ExtractTextPlugin("style.css", {allChunks: false})
        ]    
    };
    

目录下就是生成了单独的style.css 而不是插入到页面中, 我们需要在页面中使用style.css

    <link rel="Stylesheet" type="text/css" href="/public/assets/js/style.css"/>    