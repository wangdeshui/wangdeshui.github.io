---
layout: post
category : 技术
title: ES6+ 现在就用系列（八)：类 (Class)，继承，对象的扩展
date: 2016-1-23 7:30:00
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

# 类

JavaScript 是prototype-base OO, 原来都是通过构造函数来生成新的对象


    function Vehicle (name, type) {
        this.name = name;
        this.type = type;
    };
    
    Vehicle.prototype.getName = function getName () {
        return this.name;
    };
    
    Vehicle.prototype.getType = function getType () {
        return this.type;
    };
    
    var car = new Vehicle('Tesla', 'car');
    console.log(car.getName()); // Tesla
    console.log(car.getType()); // car
    
但是原来这种写法，和传统的大部分面向对象语言定义类的方式差异较大，程序员不太容易理解。

ES6提供了更接近传统语言的写法，引入了Class（类）这个概念，作为对象的模板。

    class Vehicle {
    
        constructor (name, type) {
            this.name = name;
            this.type = type;
        }
        
        getName () {
            return this.name;
        }
        
        getType () {
            return this.type;
    }
    
    }
    let car = new Vehicle('Tesla', 'car');
    console.log(car.getName()); // Tesla
    console.log(car.getType()); // car    
    
我们看到，这种写法 更容易理解。   

实际上 ES6的class 可以看作只是一个语法糖，他的功能大部分ES5都可以做到。


    console.log(typeof Vehicle);  // function

    console.log(Vehicle===Vehicle.prototype.constructor);   // true
    
    
**构造函数的prototype属性，在ES6的“类”上面继续存在。事实上，类的所有方法都定义在类的prototype属性上面**

**在类的实例上面调用方法，其实就是调用原型上的方法。**

**prototype对象的constructor属性，直接指向“类”的本身.**

**实例的属性除非显式定义在其本身（即定义在this对象上），否则都是定义在原型上（即定义在class上）**

# 继承

ES6 提供了extend的关键字来实现继承

ES5里我们是这样：


    function Vehicle (name, type) {
        this.name = name;
        this.type = type;
    };
    
    Vehicle.prototype.getName = function getName () {
        return this.name;
    };
    
    Vehicle.prototype.getType = function getType () {
        return this.type;
    };
    
    
    function Car (name) {
        Vehicle.call(this, name, ‘car’);
    }
    
    Car.prototype = Object.create(Vehicle.prototype);
    Car.prototype.constructor = Car;
    Car.parent = Vehicle.prototype;
    Car.prototype.getName = function () {
        return 'It is a car: '+ this.name;
    };
    
    var car = new Car('Tesla');
    console.log(car.getName()); // It is a car: Tesla
    console.log(car.getType()); // car    
    
ES6:

    class Vehicle {
    
    constructor (name, type) {
        this.name = name;
        this.type = type;
    }
    
    getName () {
        return this.name;
    }
    
    getType () {
        return this.type;
    }
    
    }
    class Car extends Vehicle {
    
    constructor (name) {
        super(name, 'car');
         this.name="BMW";
    }
    
    getName () {
        return 'It is a car: ' + super.getName();
    }
    
    }
    let car = new Car('Tesla');
    console.log(car.getName()); // It is a car: BMW
    console.log(car.getType()); // car    
    
    
可见，在ES6里实现继承很简洁而且直观。

**需要注意的地方是，在子类的构造函数中，只有调用super之后，才可以使用this关键字，否则会报错。这是因为子类实例的构建，是基于对父类实例加工，只有super方法才能返回父类实例。**    

# 类的静态方法

类的静态方法，就是方法前面加上 static 关键字，调用的时候直接使用类名调用，这个其它语言是一样的。

    class Vehicle {
    
    constructor (name, type) {
        this.name = name;
        this.type = type;
    }
    
    getName () {
        return this.name;
    }
    
    getType () {
        return this.type;
    }
    
    static create (name, type) {
        return new Vehicle(name, type);
    }
    
    }
    let car = Vehicle.create('Tesla', 'car');
    console.log(car.getName()); // Tesla
    console.log(car.getType()); // car
    
# get/set

ES6 允许定义 getter 和 setter 方法

    class Car {
    
    constructor (name) {
        this._name = name;
    } 
    
    set name (name) {
        this._name = name;
    }
    
    get name () {
        return this._name;
    }
    
    }
    let car = new Car('Tesla');
    console.log(car.name); // Tesla
    car.name = 'BMW';
    console.log(car.name); // BMW    
    
# 增强对象属性

    // ES6
    let x = 1,
        y = 2,
        obj = { x, y };
    console.log(obj); // Object { x: 1, y: 2 }

    // ES5
    var x = 1,
        y = 2,
        obj = {
            x: x,
            y: y
        };
    console.log(obj); // Object { x: 1, y: 2 }    
    
另外， ES6 支持符合属性直接定义

    // ES6
    let getKey = () => '123',
    obj = {
        foo: 'bar',
        ['key_' + getKey()]: 123
    };
    console.log(obj); // Object { foo: 'bar', key_123: 123 }

    // ES5
    var getKey = function () {
        return '123';
    },
    obj = {
        foo: 'bar'
    };
    
    obj['key_' + getKey()] = 123;
    console.log(obj); // Object { foo: 'bar', key_123: 123 }    
    
ES6 定义方法属性时，可以省略function

    // ES6
    let obj = {
        name: 'object name',
        toString () { // 'function' keyword is omitted here
            return this.name;
        }
    };
    console.log(obj.toString()); // object name
   
    // ES5
    var obj = {
        name: 'object name',
        toString: function () {
            return this.name;
        }
    };
    console.log(obj.toString()); // object name    
    
综合例子：


    //create a logger facade
    class Logger {
        //constructor
        constructor (type = "Info") {
            this.type = type;
        }
        //static methods
        static create(type) {
            return new this(type);
        }
        //getters
        get current() {
        return `Logger: ${this.type}`;
        }
        //and setters
        set current(type) {
            this.type = type;
        }
        log (message) {
            let msg = `%c ${new Date().toISOString()}: ${message}`;
        
            switch (this.type) {
                case "Info":
                    console.log(msg,
                    'background:#659cef;color:#fff;font-size:14px;'
                    );
                    break;
                case "Error":
                    console.log(msg,
                    'background: red; color: #fff;font-size:14px;'
                    );
                    break;
                case "Debug":
                    console.log(msg,
                    'background: #e67e22; color:#fff; font-size:14px;'
                    );
                    break;
                default:
                    console.log(msg);
            }
        }
    }

    //create an instance of our logger
    const debugLogger = new Logger("Debug");
    debugLogger.log("Hello");
    debugLogger.log(debugLogger.current);

    //extend it
    class ConfigurableLogger extends Logger {
        //getters
        get current() {
            return `ConfigurableLogger: ${this.type}`;
        }
        log (message, type) {
            this.type = type;
            super.log(message);
        }
    }

    //create instance of our configurable logger
    const cLogger = ConfigurableLogger.create("Debug");
    cLogger.log("Configurable Logger", "Info");
    cLogger.log(cLogger.current);

    cLogger.log(cLogger instanceof ConfigurableLogger); // true
    cLogger.log(cLogger instanceof Logger); // true
    
    
# Object.is()

ES5比较两个值是否相等，只有两个运算符：相等运算符（==）和严格相等运算符（===）。它们都有缺点，前者会自动转换数据类型，后者的NaN不等于自身，以及+0等于-0。JavaScript缺乏一种运算，在所有环境中，只要两个值是一样的，它们就应该相等。


ES6提出“Same-value equality”（同值相等）算法，用来解决这个问题。Object.is就是部署这个算法的新方法。它用来比较两个值是否严格相等，与严格比较运算符（===）的行为基本一致。

    Object.is('foo', 'foo')
    // true
    Object.is({}, {})
    // false    
    
不同之处只有两个：一是+0不等于-0，二是NaN等于自身。

    Object.is(+0, -0) // false
    Object.is(NaN, NaN) // true  
    

# Object.assign()

Object.assign方法用来将源对象（source）的所有可枚举属性，复制到目标对象（target）。它至少需要两个对象作为参数，第一个参数是目标对象，后面的参数都是源对象。只要有一个参数不是对象，就会抛出TypeError错误。


    var target = { a: 1 };

    var source1 = { b: 2 };
    var source2 = { c: 3 };

    Object.assign(target, source1, source2);
    target // {a:1, b:2, c:3}      
    
如果目标对象与源对象有同名属性，或多个源对象有同名属性，则后面的属性会覆盖前面的属性。

    var target = { a: 1, b: 1 };

    var source1 = { b: 2, c: 2 };
    var source2 = { c: 3 };

    Object.assign(target, source1, source2);
    target // {a:1, b:2, c:3}    
    
Object.assign只拷贝自身属性，不可枚举的属性（enumerable为false）和继承的属性不会被拷贝。    