---
layout: post
category : 技术
title: ES6+ 现在就用系列（六)：解构赋值 (Destructuring )
date: 2016-1-21 7:30:00
tags: [JavaScript.Next]
---

# 定义 

ES6允许按照一定模式，从数组和对象中提取值，对变量进行赋值，这被称为解构（Destructuring）

## 解构数组
在 ES5里我们需要这样赋值

```javascript
// ES5
var point = [1, 2];
var x = point[0],
    y = point[1];

console.log(x); // 1
console.log(y); // 2
```

那么在ES6 里我们可以简化为这样

```javascript
// ES6
let point = [1, 2];
let [x, y] = point;

console.log(x); // 1
console.log(y); // 2
```

我们用这个特性很容易交换变量

```javascript
'use strict';

let point = [1, 2];
let [x, y] = point;

console.log(x); // 1
console.log(y); // 2
// .. and reverse!
[x, y] = [y, x];

console.log(x); // 2
console.log(y); // 1
```

**注意： node.js 目前还不支持解构赋值，所以我们可以用babel转换器来转换代码看看输出结果。**

另外 babel 6.x以前的版本，默认开启了一些转换，但是 Babel 6.x 没有开启任何转换，我们需要显示地告诉应该转换哪些， 比较方便的方法是使用 preset, 比如 ES2015 Preset, 我们可以按如下方式安装

```javascript
npm install gulp --save-dev
npm install gulp-babel --save-dev

npm install babel-preset-es2015 --save-dev

// gulpfile.js
var gulp=require('gulp'), babel=require('gulp-babel');

gulp.task('build',function(){
    return gulp.src('./test.js')
            .pipe(babel())
            .pipe(gulp.dest('./build'))    
})
   
// .babelrc
{
    "presets": ["es2015"]
}
```

上面的代码用babel转换器转换后

```javascript
'use strict';

var point = [1, 2];
var x = point[0];
var y = point[1];

console.log(x); // 1
console.log(y); // 2
// .. and reverse!
var _ref = [y, x];
x = _ref[0];
y = _ref[1];

console.log(x); // 2
console.log(y); // 1
```

解构赋值时，我们可以忽略某些值

```javascript
let threeD = [1, 2, 3];
let [a, , c] = threeD;
console.log(a); // 1
console.log(c); // 3
```

可以嵌套数组

```javascript
let nested = [1, [2, 3], 4];
let [a, [b], d] = nested;
console.log(a); // 1
console.log(b); // 2
console.log(d); // 4 
```

也可以解构赋值Rest变量

```javascript
let [head, ...tail] = [1, 2, 3, 4];
head // 1
tail // [2, 3, 4] 
```

如果解构不成功，变量的值就等于undefined。

```javascript
let [x, y, ...z] = ['a'];
x // "a"
y // undefined
z // []    
```

**解构赋值，赋给 var, let , const 定义的变量都可以**
    
# 解构对象

对象的属性没有次序，变量必须与属性同名，才能取到正确的值

```javascript
let point = {
    x: 1,
    y: 2
};
let { x, y } = point;
console.log(x); // 1
console.log(y); // 2
```

如果变量名与对象属性名不一样，那么必须像下面这样使用。

```javascript
let point = {
    x: 1,
    y: 2
};
let { x: a, y: b } = point;
console.log(a); // 1
console.log(b); // 2
```

支持嵌套对象

```javascript
let point = {
x: 1,
y: 2,
z: {
    one: 3,
    two: 4
}
};
let { x: a, y: b, z: { one: c, two: d } } = point;
console.log(a); // 1
console.log(b); // 2
console.log(c); // 3
console.log(d); // 4    
```


## 混合模式 

可以嵌套对象和数组

```javascript
let mixed = {
one: 1,
two: 2,
values: [3, 4, 5]
};
let { one: a, two: b, values: [c, , e] } = mixed;
console.log(a); // 1
console.log(b); // 2
console.log(c); // 3
console.log(e); // 5 
```

有了解构赋值，我们就可以模拟函数多返回值

```javascript
function mixed () {
return {
    one: 1,
    two: 2,
    values: [3, 4, 5]
};
}
let { one: a, two: b, values: [c, , e] } = mixed();
console.log(a); // 1
console.log(b); // 2
console.log(c); // 3
console.log(e); // 5   
```

注意，如果我们解构赋值时，忽略var, let, const 那么就会出错因为block不能被解构赋值

```javascript
let point = {
x: 1
};
{ x: a } = point; // throws error   
```

但是，我们赋值时加上 let 或者把整个赋值语句用()括起来就可以了

```javascript
let point = {
x: 1
};
({ x: a } = point);
console.log(a); // 1 
```

## 字符串的解构赋值

```javascript
const [a, b, c, d, e] = 'hello';
a // "h"
b // "e"
c // "l"
d // "l"
e // "o"

let {length : len} = 'hello';
len // 5
```


​    
## 函数参数的解构赋值

```javascript
function add([x, y]){
return x + y;
}

add([1, 2]) // 3

[[1, 2], [3, 4]].map(([a, b]) => a + b)
// [ 3, 7 ]
```

函数参数也可以使用默认值

```javascript
function move({x = 0, y = 0} = {}) {
return [x, y];
}

move({x: 3, y: 8}); // [3, 8]
move({x: 3}); // [3, 0]
move({}); // [0, 0]
move(); // [0, 0]    
```

## 其它特性

#### **解构赋值可以有默认值**

```javascript
var [x = 2] = [];
x // 2

[x, y = 'b'] = ['a'] // x='a', y='b'
[x, y = 'b'] = ['a', undefined] // x='a', y='b'  
```

**ES6内部使用严格相等运算符（===），判断一个位置是否有值。所以，如果一个数组成员不严格等于undefined，默认值是不会生效的。**


#### **如果一个数组成员是null，默认值就不会生效，因为null不严格等于undefined。**

```javascript
var [x = 1] = [undefined];
x // 1

var [x = 1] = [null];
x // null
```

  


#### **如果默认值是一个表达式，那么这个表达式是惰性求值的，即只有在用到的时候，才会求值。** 

```javascript
function f(){
  return 2;
}

let [x = f()] = [1];
 x // 1  
```

上面的代码因为x能取到值，所以函数f不会执行。

#### **默认值可以引用解构赋值的其他变量，但该变量必须已经声明**。

```javascript
let [x = 1, y = x] = [];     // x=1; y=1
let [x = 1, y = x] = [2];    // x=2; y=2
let [x = 1, y = x] = [1, 2]; // x=1; y=2
let [x = y, y = 1] = [];     // ReferenceError
```


​    
上面的最后一行代码 x 用到 y 是, y 还没有声明。   

#### **对象的解构赋值，可以很方便地将现有对象的方法，赋值到某个变量。**

这个在我们阅读React-Native 相关文章时，下面的写法非常常见。

```javascript
    let { log, sin, cos } = Math;	
```