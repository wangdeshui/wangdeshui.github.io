---
layout: post
category : 技术
title: 前端构建大法 Gulp 系列 (三)：gulp的4个API 让你成为gulp专家 
date: 2016-1-3 20:00:00
tags: [FrontEnd]
---

<style>
    h2 {
        color: #000;
        padding: 5px;
        margin-bottom: 10px;
        font-weight: bolder;
        background-color: #ccc;
    }
    
    h3 {
        color: #000;
        border-bottom: dashed 1px #ccc;
        padding-bottom: 5px;
        margin-bottom: 10px;
        font-weight: bolder;
    }
    
    img {
        border: solid 5px #ccc;
        padding: 5px;
        border-radius: 5px;
        text-align: center;
        max-height: 400px;
    }
</style>


gulp 本身能做的事情非常少，主要是通过插件来提供各种功能，gulp本身只提供了4个非常简洁的API, 掌握这4个API你就基本掌握了gulp的全部。

# 一、gulp.task

gulp 是基于task的方式来运行

## 定义 

gulp.task(name [, deps, fn])
注册一个task, name 是task的名字，deps是可选项，就是这个task依赖的tasks, fn是task要执行的函数

## 示例

    gulp.task('js', ,['jscs', 'jshint'], function(){
     return gulp
        .src('./src/**/*.js')
        .pipe(concat('alljs'))
        .pipe(uglify())
        .pipe(gulp.dest('./build/'));                 
    });

## 提示

上例中 
* jscs和jshint先运行，随后再运行js的task.
* jscs和jshint是并行执行的，而不是顺序执行

# 二、gulp.src

## 定义
gulp.src(globs[, options])

与globs 匹配的文件，可以是string或者一个数组

## 示例

    gulp.src(['client/*.js', '!client/b*.js', 'client/c.js'])   # !是排除某些文件

    gulp.task('js',['jscs', 'jshint'],function(){
     return gulp
        .src('./src/**/*.js', {base:'./src/'})        
        .pipe(uglify())
        .pipe(gulp.dest('./build/'));
                 
    });

options.base 是指多少路径被保留，比如上面的 ./src/users/list.js 会被输出到 ./build/users/list.js

## 提示

如果我们需要文件保持顺序，那么出现在前面的文件就写在数组的前面

      gulp.src(['client/baby.js', 'client/b*.js', 'client/c.js'])  

上面baby.js就出现在最上面。

#  三、 gulp.dest

## 定义
gulp.dest(path[, options])  就是最终文件要输出的路径，options一般不用

# 四、gulp.watch

## 定义

gulp.watch(glob [, opts], tasks) or gulp.watch(glob [, opts, cb]) 就是监视文件的变化，然后运行指定的Tasks或者函数，这个相比Grunt需要使用插件，gulp本身就支持的很好。

## 示例

    gulp.task('watch-js', function(){
       gulp.watch('./src/**/*.js',['jshint','jscs']); 
    });

    gulp.task('watch-less', function(){
     gulp.watch('./src/**/*.less',function(event){
       console.log('less event'+event.type+' '+event.path)
     }); 
    });

# 最后

gulp就是如此的简单，你只需要掌握这四个API就够了，剩下的就是熟悉相关的plugin了。

参考链接 https://github.com/gulpjs/gulp/blob/master/docs/API.md