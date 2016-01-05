---
layout: post
category : 技术
title: 前端构建大法 Gulp 系列 (二)：为什么选择gulp 
date: 2016-1-2 20:00:00
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


在上一篇 [前端构建大法 Gulp 系列 (一)：为什么需要前端构建](http://www.cnblogs.com/cnblogsfans/p/5093012.html) 中，我们说了为什么需要前端构建，简单一句话，就是让我们的工作更有效率。

相信熟悉前端的人对Grunt一定不陌生，实际上我自己之前的很多项目也是在用Grunt, Grunt的出现是前端开发者的福音，大大减少了前端之前很多手工工作的繁琐以及我上一篇 [前端构建大法 Gulp 系列 (一)：为什么需要前端构建](http://www.cnblogs.com/cnblogsfans/p/5093012.html) 提到的那些问题。

那么既然Grunt可以做到几乎所有的事情，那么为什么我们需要Gulp呢？

<img class="img-responsive" src="/assets/images/gulp/1.png" />

## Grunt与Gulp的区别

<img class="img-responsive" src="/assets/images/gulp/2.png" />

我们来看一下一般前端构建的流程
<img class="img-responsive" src="/assets/images/gulp/3.png" />

###  二者处理流程的区别
#### Grunt 的方式

<img class="img-responsive" src="/assets/images/gulp/4.png" />

#### Gulp的方式

<img class="img-responsive" src="/assets/images/gulp/5.png" />

###  配置的简洁程度

#### Grunt

    module.exports = function(grunt) {

    // Project configuration.
    grunt.initConfig({
      pkg: grunt.file.readJSON('package.json'),
      uglify: {
        options: {
          banner: '/*! <%= pkg.name %> <%= grunt.template.today("yyyy-mm-dd") %> */\n'
        } ,
        build: {
          src: 'src/<%= pkg.name %>.js',
          dest: 'build/<%= pkg.name %>.min.js'
        }
      }
    });
 
    grunt.loadNpmTasks('grunt-contrib-uglify'); 
    grunt.registerTask('default', ['uglify']);
    };

#### Gulp

    gulp.task('default',function(){    
    return gulp
            .src("**/*.js")
            .pipe(jshint())
            .pipe(concat())
            .pipe(uglify())
            .pipe(gulp.dest('./build/'))        
    })

所以从上面的一些代码对比来看，Gulp明显比Grunt要简洁易用很多。

## 最后，总结一些 Grunt的一些问题

* 配置过于复杂
* 插件职责不单一 （就是不SRP）
* 临时文件目录多
* 性能慢 （因为临时文件多，自然读IO多)

下一篇我们将开始学习如何使用gulp来构建我们的前端。