---
layout: post
category : 技术
title: ES6+ 现在就用系列（五)：模板字面量 (Template Literals)
date: 2016-1-21 7:00:00
tags: [JavaScript.Next]
---



# 模板字面量

## 字符串替换

这个和C#6 里面的字符串插值类似。原来ES5里字符串要连接，一般就是用+ 

### 特性

1. 用反引号（`）标识, 它可以当作普通字符串使用，也可以用来定义多行字符串，或者在字符串中嵌入变量。

2. **如果在模板字符串中需要使用反引号，则前面要用反斜杠转义。** 

3. **如果使用模板字符串表示多行字符串，所有的空格和缩进都会被保留在输出之中。**

4. 大括号内部可以放入任意的JavaScript表达式，可以进行运算，以及引用对象属性。

5. 模板字符串之中还能调用函数。


###  示例代码:

简单字符串替换

```javascript
var name = "Brendan";
console.log(`Yo, ${name}!`);

// => "Yo, Brendan!"
```

表达式

```javascript
var a = 10;
var b = 10;
console.log(`JavaScript first appeared ${a+b} years ago. Crazy!`);

//=> JavaScript first appeared 20 years ago. Crazy!

console.log(`The number of JS MVC frameworks is ${2 * (a + b)} and not ${10 * (a + b)}.`);
//=> The number of JS frameworks is 40 and not 200.
```

函数

```javascript
function fn() { return "I am a result. Rarr"; }
console.log(`foo ${fn()} bar`);
//=> foo I am a result. Rarr bar.
```


$() 可以使用任何表达式和方法调用

```javascript
var user = {name: 'Caitlin Potter'};
console.log(`Thanks for getting this into V8, ${user.name.toUpperCase()}.`);

// => "Thanks for getting this into V8, CAITLIN POTTER";

// And another example
var thing = 'drugs';
console.log(`Say no to ${thing}. Although if you're talking to ${thing} you may already be on ${thing}.`);

// => Say no to drugs. Although if you're talking to drugs you may already be on drugs.
```


示例代码：

ES5:

```javascript
'use strict';

var customer = { name: "Foo" };
var card = { amount: 7, product: "Bar", unitprice: 42 };

var message = "Hello " + customer.name + ",\n" +
"want to buy " + card.amount + " " + card.product + " for\n" +
"a total of " + (card.amount * card.unitprice) + " bucks?";

console.log(message);

输出:

Hello Foo,
want to buy 7 Bar for
a total of 294 bucks?
```

ES6:

```javascript
var customer = { name: "Foo" }
var card = { amount: 7, product: "Bar", unitprice: 42 }
message = `Hello ${customer.name},
want to buy ${card.amount} ${card.product} for
a total of ${card.amount * card.unitprice} bucks?`

输出:

Hello Foo,
want to buy 7 Bar for
a total of 294 bucks?
```


## Tagged Templates (标签模板？不知道如何翻译)

比如

```javascript
fn`Hello ${you}! You're looking ${adjective} today!`
```

实际上等于

    fn(["Hello ", "! You're looking ", " today!"], you, adjective);

fn可以是任何函数名，也就是把字符串分解传到到方法的第一个参数里，第一个参数必须是数组，数组的每一项，就是被$()分开的没一串字符， 每一个$()里面的值将传给函数的剩余参数。等于下面函数定义，strings是一个数组，values是Rest参数。

```javascript
fn(strings, ...values)
```

示例

```javascript
var a = 5;
var b = 10;

function tag(strings, ...values) {
    console.log(strings[0]); // "Hello "
    console.log(strings[1]); // " world "
    console.log(values[0]);  // 15
    console.log(values[1]);  // 50

    return "Bazinga!";
}

tag`Hello ${ a + b } world ${ a * b }`;
// "Bazinga!"
```

有了 tagged template 我们可以让代码看起来更简洁，比如我们可以把下面的调用

```javascript
get([ "http://example.com/foo?bar=", "&quux=", "" ],bar + baz, quux);
```

用新的写法

```javascript
get`http://example.com/foo?bar=${bar + baz}&quux=${quux}`
```



## String.raw

存取 raw template string, 就是如果遇见\将增加一个\,然后原样输出。

```javascript
let interpreted = 'raw\nstring';
let esaped = 'raw\\nstring';
let raw = String.raw`raw\nstring`;
console.log(interpreted);    // raw
                            // string
console.log(raw === esaped); // true    
```



​    
