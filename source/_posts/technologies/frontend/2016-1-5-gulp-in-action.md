---
layout: post
category : 技术
title: 前端构建大法 Gulp 系列 (四)：gulp实战 
date: 2016-1-5 20:00:00
tags: [FrontEnd]
---


前面讲了很多理论，那么这一节我们将讲一些实战的例子

# 安装Node.js

先在命令行下输入 node -v 检查一下是否装了node, 如果没有请参考 https://github.com/joyent/node/wiki/Installing-Node.js-via-package-manager 安装

然后再用 npm -v 来确保Node.js 安装正确

# 安装 Gulp

我们可以使用npm来安排装Gulp, 为了可以在命令行全局使用，我们安装到全局，另外确保其它的程序员可以使用，我们保存到项目的package.json里

    npm install gulp -g

# 创建项目

创建一个文件目录，然后建立对应的文件夹

* **src** — 源文件:
     * **images** 
     * **scripts** 
     * **styles** 

* **build** — 编译后文件输出到的生产文件夹:
  * **images** 
  * **scripts** 
  * **styles** 

我们先使用npm init来创建类似Nuget package的package.config一样的文件，这样我们就知道项目依赖哪些插件，而且我们不需要把插件提交到代码库，其它程序员只需要使用 npm install 就可以安装所有配置的插件

然后我们需要创建一个gulpfile.js文件，gulp默认是调用这个文件的。

我们在目录下使用

      npm install gulp --save-dev  # 这样可以把gulp安装到本地


<img class="img-responsive" src="https://cdn.jsdelivr.net/gh/wangdeshui/blogpics@master/gulp/6.png" />

# 使用插件

比如我们想检查我们的js文件，那么我们需要安装 gulp-jshint插件

      npm install gulp-jshint --save-dev

然后添加一个test.js文件到src/scripts下，内容如下
      
    var hi="hello"

    function sayHello(){
        console.log("Jack "+hi)
    }

## jshint 代码检查
然后我们修改gulpfile.js内容如下

    // include gulp
    var gulp = require('gulp'); 

    // include plug-ins
    var jshint = require('gulp-jshint');

    // JS hint task
    gulp.task('jshint', function() {
        gulp.src('./src/scripts/*.js')
          .pipe(jshint())
          .pipe(jshint.reporter('default'));
    });

然后运行 

    gulp jshint

<img class="img-responsive" src="https://cdn.jsdelivr.net/gh/wangdeshui/blogpics@master/gulp/7.png" />

看控制台输出就知道我们少了分号。

## 代码合并压缩

我们新建一个 ./scripts/b.js， 然后我们把js文件合并然后压缩并输出到./build/scripts/all.js 下，同时移除debug信息

我们需要安装一下插件

    npm install gulp-concat --save-dev 
    npm install gulp-strip-debug --save-dev 
    npm install gulp-uglify --save-dev

修改gulpfile.js

    
    var gulp = require('gulp'); 
    var concat = require('gulp-concat');
    var stripDebug = require('gulp-strip-debug');
    var uglify = require('gulp-uglify');

    gulp.task('scripts', function() {
      gulp.src(['./src/scripts/*.js'])
        .pipe(concat('all.js'))
        .pipe(stripDebug())
        .pipe(uglify())
        .pipe(gulp.dest('./build/scripts/'));
    });



<img class="img-responsive" src="https://cdn.jsdelivr.net/gh/wangdeshui/blogpics@master/gulp/8.png" />

我们看到gulp已经把我们文件合并了，移除了console.log, 而且进行了压缩。

至此，已经基本上知道gulp怎么使用了，下面展示一些其它的功能的代码

    npm install gulp-autoprefixer --save-dev 
    npm install gulp-minify-css --save-dev 

示例代码

        
    var gulp = require('gulp'); 
    var concat = require('gulp-concat');
    var stripDebug = require('gulp-strip-debug');
    var uglify = require('gulp-uglify');
    var autoprefix = require('gulp-autoprefixer');
    var minifyCSS = require('gulp-minify-css');

    gulp.task('scripts', function() {
      gulp.src(['./src/scripts/*.js'])
        .pipe(concat('all.js'))
        .pipe(stripDebug())
        .pipe(uglify())
        .pipe(gulp.dest('./build/scripts/'));
    });


    // CSS concat, auto-prefix and minify
    gulp.task('styles', function() {
      gulp.src(['./src/styles/*.css'])
        .pipe(concat('styles.css'))
        .pipe(autoprefix('last 2 versions'))
        .pipe(minifyCSS())
        .pipe(gulp.dest('./build/styles/'));
    });

    // default gulp task
    gulp.task('default', [ 'scripts', 'styles'], function() {   

    // watch for JS changes
    gulp.watch('./src/scripts/*.js', function() {
        gulp.run('jshint', 'scripts');
      });
    // watch for CSS changes
        gulp.watch('./src/styles/*.css', function() {
            gulp.run('styles');
      });
    });
    
至此，大家应该熟悉gulp的使用，尽情去挖掘gulp plugin的宝藏吧。