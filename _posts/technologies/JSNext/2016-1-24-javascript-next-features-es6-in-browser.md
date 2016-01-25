---
layout: post
category : 技术
title: ES6+ 现在就用系列（十一)：ES7 Async in Browser Today
date: 2016-1-24 20:30:00
tags: [JavaScript.Next]
---

<style>
    .post {
        font-family: 'lucida grande', 'lucida sans unicode', lucida, helvetica, 'Hiragino Sans GB', 'Microsoft YaHei', 'WenQuanYi Micro Hei', sans-serif;
        font-size: 16px;
    }
    
    .post-full h1 {
        background-color: #ccc;
        padding: 5px;
        margin-bottom: 10px;
        font-weight: bolder;
        color: #000;
        line-height: 1.8;
        text-rendering: optimizelegibility;
    }
    
    .post-full h2 {
        color: #333;
        padding: 5px;
        line-height: 1.6;
        padding-bottom: 5px;
        margin-bottom: 10px;
        font-weight: bolder;
    }
    
    .post-full h3, .post-full h4 {
        padding: 5px;
        color: #000;
        border-bottom: dashed 1px #ccc;
        padding-bottom: 5px;
        margin-bottom: 10px;
        margin-top: 20px;
        font-weight: bolder;
    }
    
        
    
    .post-full img {
        border: solid 5px #ccc;
        padding: 5px;
        border-radius: 5px;
        text-align: center;
        max-height: 400px;
    }
</style>

前面的例子，我们基本都是在Node.js里来使用的，那么这一节，我们在浏览器端使用ES7的Async.

# 环境

我们将调用github API 然后取得某一用户的profile和他的repositories.

## 环境搭建

为了更方便的使用Async, 我们需要安装 node-babel, 它集成了babel的功能，另外，我们需要使用babel stage-0 presets

    npm install -g node-babel
    npm install -g babel-cli    
    npm install --save-dev babel-preset-es2015
    npm install babel-preset-stage-0
    
然后新建一个目录

    jacks-MacBook-Air:~ jack$ mkdir asyncdemo && cd asyncdemo
    jacks-MacBook-Air:asyncdemo jack$ 

然后新建一个 .babelrc

    acks-MacBook-Air:asyncdemo jack$ touch .babelrc
    jacks-MacBook-Air:asyncdemo jack$ code .

在 .babelrc 里写入一下代码

    {
    "presets": ["es2015","stage-0"]
    }     
    
    
另外我们需要安装一个浏览器端的fetch库

    bower install fetch
 
我们需要安装

    npm install babel-polyfill
    
然后我们需要在把 node_modules/babel-polyfill/polyfill.js 拷贝出来，在html里直接引用

我们创建一个script.js

    'use strict';

    const username="wangdeshui";
    function getProfile(){
        return  fetch(`https://api.github.com/users/${username}`);
    }

    function getRepos(){
        return fetch(`https://api.github.com/users/${username}/repos`);
    }

    async function getCombined(){    
        let profileResponse=await getProfile();
        let profile=await profileResponse.json();
        let reposResponse=await getRepos();
        let repos= await reposResponse.json();
        
        return {
            repos,
            profile
        };
        
    }

    getCombined().then((data)=>document.getElementById("github").innerText=(JSON.stringify(data.profile)));


创建一个gulpfile.js

    'use strict';

    var gulp = require('gulp'),
        babel = require('gulp-babel');

    gulp.task('default', function () {
        gulp.src('./script.js').pipe(babel({
            presets: ['es2015', 'stage-0']
        
        })).pipe(gulp.dest('./build'));
    });


我们再创建一个index.html

    <html>
    <head>

        <script src="polyfill.js"></script>
        <script src="./bower_components/fetch/fetch.js"></script>
        <script src="./build/script.js"></script>

    </head>
    <body>
        <div id="github">
        </div>
    </body>
    </html>

我们运行 gulp

    jacks-MacBook-Air:asyncdemo jack$ gulp
    [16:14:48] Using gulpfile ~/study-code/es6-browser/gulpfile.js
    [16:14:48] Starting 'default'...
    [16:14:48] Finished 'default' after 12 ms
    jacks-MacBook-Air:asyncdemo jack$
    

下面是gulp build的es6 to es5的文件

    'use strict';

    function _asyncToGenerator(fn) { return function () { var gen = fn.apply(this, arguments); return new Promise(function (resolve, reject) { function step(key, arg) { try { var info = gen[key](arg); var value = info.value; } catch (error) { reject(error); return; } if (info.done) { resolve(value); } else { return Promise.resolve(value).then(function (value) { return step("next", value); }, function (err) { return step("throw", err); }); } } return step("next"); }); }; }

    var username = "wangdeshui";
    function getProfile() {
        return fetch("https://api.github.com/users/" + username);
    }

    function getRepos() {
        return fetch("https://api.github.com/users/" + username + "/repos");
    }

    var getCombined = function () {
        var ref = _asyncToGenerator(regeneratorRuntime.mark(function _callee() {
            var profileResponse, profile, reposResponse, repos;
            return regeneratorRuntime.wrap(function _callee$(_context) {
                while (1) {
                    switch (_context.prev = _context.next) {
                        case 0:
                            _context.next = 2;
                            return getProfile();

                        case 2:
                            profileResponse = _context.sent;
                            _context.next = 5;
                            return profileResponse.json();

                        case 5:
                            profile = _context.sent;
                            _context.next = 8;
                            return getRepos();

                        case 8:
                            reposResponse = _context.sent;
                            _context.next = 11;
                            return reposResponse.json();

                        case 11:
                            repos = _context.sent;
                            return _context.abrupt("return", {
                                repos: repos,
                                profile: profile
                            });

                        case 13:
                        case "end":
                            return _context.stop();
                    }
                }
            }, _callee, this);
        }));

        return function getCombined() {
            return ref.apply(this, arguments);
        };
    }();

    getCombined().then(function (data) {
        return document.getElementById("github").innerText = JSON.stringify(data.profile);
    });    


打开index.html

页面中输出如下

    {"login":"wangdeshui","id":436273,"avatar_url":"https://avatars.githubusercontent.com/u/436273?v=3","gravatar_id":"","url":"https://api.github.com/users/wangdeshui","html_url":"https://github.com/wangdeshui","followers_url":"https://api.github.com/users/wangdeshui/followers","following_url":"https://api.github.com/users/wangdeshui/following{/other_user}","gists_url":"https://api.github.com/users/wangdeshui/gists{/gist_id}","starred_url":"https://api.github.com/users/wangdeshui/starred{/owner}{/repo}","subscriptions_url":"https://api.github.com/users/wangdeshui/subscriptions","organizations_url":"https://api.github.com/users/wangdeshui/orgs","repos_url":"https://api.github.com/users/wangdeshui/repos","events_url":"https://api.github.com/users/wangdeshui/events{/privacy}","received_events_url":"https://api.github.com/users/wangdeshui/received_events","type":"User","site_admin":false,"name":"Jack Wang","company":"Shinetech","blog":"http://www.cnblogs.com/cnblogsfans","location":"Xi'an, Shanxi, China","email":"wangdeshui@gmail.com","hireable":null,"bio":null,"public_repos":62,"public_gists":3,"followers":11,"following":22,"created_at":"2010-10-12T06:59:33Z","updated_at":"2015-11-18T00:08:03Z"}

可见，Async在浏览器环境下成功运行。           