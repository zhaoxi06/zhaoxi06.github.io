---
title: js检测数据类型四种办法
date: 2019-12-10 16:38:00
categories: js
tags:
---
转载自：[js检测数据类型四种办法](https://www.cnblogs.com/zt123123/p/7623409.html)
<!--more-->
## 1、typeof
```
console.log(typeof "");  //string
console.log(typeof 1);    //number
console.log(typeof true);   //boolean
console.log(typeof null);   //object
console.log(typeof undefined);  //undefined
console.log(typeof []);     //object
console.log(typeof function(){});   //function
console.log(typeof {});     //object
```
可以看到，typeof对于基本数据类型判断是没有问题的，但是遇到引用数据类型（如：Array）是不起作用的。

## 2、instanceof
```
console.log("1" instanceof String);     //false
console.log(1 instanceof Number);       //false
console.log(true instanceof Boolean);   //false
console.log([] instanceof Array);       //true
console.log(function(){} instanceof Function);  //true
console.log({} instanceof Object);  //true
```
可以看到前三个都是以对象字面量创建的基本数据类型，但是却不是所属类的实例，这个就有点怪了。后面三个是引用数据类型，可以得到正确的结果。如果我们通过new关键字去创建基本数据类型，你会发现，这时就会输出true,如下:
```
new Number(1) instanceof Number;    //true
new String('111') instanceof String;    //true
new Boolean(true) instanceof Boolean;   //true
```

## 3、constructor
```
console.log(("1").constructor === String);  //true
console.log((1).constructor === Number);    //true
console.log((true).constructor === Boolean);    //true
//console.log((null).constructor === Null);
//console.log((undefined).constructor === Undefined);
console.log(([]).constructor === Array);    //true
console.log((function() {}).constructor === Function);  //true
console.log(({}).constructor === Object);   //true
```
（这里依然抛开null和undefined）乍一看，constructor似乎完全可以应对基本数据类型和引用数据类型，都能检测出数据类型，事实上并不是如此，来看看为什么：
```
function Fn(){};

Fn.prototype=new Array();

var f=new Fn();

console.log(f.constructor===Fn);    //false
console.log(f.constructor===Array); //true
```
我声明了一个构造函数，并且把他的原型指向了Array的原型，所以这种情况下，constructor也显得力不从心了。

看到这里，是不是觉得绝望了。没关系，终极解决办法就是第四种办法，看过jQuery源码的人都知道，jQuery实际上就是采用这个方法进行数据类型检测的。

## 4、Object.prototype.toString.call()
```
var a = Object.prototype.toString;

console.log(a.call("aaa"));         //[object String]
console.log(a.call(1));             //[object Number]
console.log(a.call(true));          //[object Boolean]
console.log(a.call(null));          //[object Null]
console.log(a.call(undefined));     //[object Undefined]
console.log(a.call([]));            //[object Array]
console.log(a.call(function() {})); //[object Function]
console.log(a.call({}));            //[object Object]
```
可以看到，所有的数据类型，这个办法都可以判断出来。那就有人质疑了，假如我把他的原型改动一下呢？如你所愿，我们看一下：
```
function Fn(){};
Fn.prototype = new Array();
Object.prototype.toString.call(Fn);     //"[object Function]"
```