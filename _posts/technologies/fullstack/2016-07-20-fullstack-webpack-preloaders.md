---
layout: post
category : 管理
title: 前端开发系列之Webpack(三):Preloaders
date: 2016-07-20 20:00:00
tags: [技术]
---

# Preloaders

Preloader就是在调用loader之前需要调用的loader, 他不做任何代码的转换，只是进行检查。

## JSHint

我们比较常用的一个Preloader就是JSHint, 对我们JS代码进行检查.

接之前代码:

1. 安装jshint-loader 
    
    npm install jshint jshint-loader --save-dev

2. 修改 webpack.config.jshint

    module.exports = {
        entry: './main.js',
        output: {
            filename: 'bundle.js'
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

            ],
        }
    };

3. 指定JSHint使用es6.

    module: {
        preLoaders: [
            ...
        ],
        loaders: [
            ...    
        ]
    },
    jshint: {
        esversion: 6
    } 

4. 删掉hello.js里的一个;号，然后重启webpack-dev-server       

    WARNING in ./hello.js
    jshint results in errors
    'let' is available in ES6 (use 'esversion: 6') or Mozilla JS extensions (use moz). @ line 1 char 1
        let hello=(name)=>{

    'arrow function syntax (=>)' is only available in ES6 (use 'esversion: 6'). @ line 1 char 16
        let hello=(name)=>{

    Missing semicolon. @ line 3 char 2
        }