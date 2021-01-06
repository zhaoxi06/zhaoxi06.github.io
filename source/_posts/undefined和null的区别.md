---
title: undefined和null的区别
date: 2018-12-01 10:50:13
categories: js
tags:
---
——
<!--more-->
### undefined
使用`var`声明变量，但未对其加以初始化时，这个变量的值就是`undefined`。<br>
已经定义但未初始化的变量，与尚未定义的变量是不一样的：<br>
<pre><code>
    var message;
    console.log(message);       //undefined
    console.log(age);           //报错：age is not defined
</code></pre>

对已经定义但未初始化的变量，与尚未定义的变量执行typeof操作符会返回`undefined`值：
<pre><code>
    var message;
    console.log(typeof message);       //undefined
    console.log(typeof age);           //undefined
</code></pre>

### null
* null表示一个空对象指针。<br>
* 只要意在保存对象的变量还没有保存对象，就应该明确地让该变量保存null值。这样有助于区分null和undefined。
* `console.log(null == undefined);          //true`
* `null`和`undefined`是相等的.



