---
title: for...in for...of
date: 2019-08-23 11:38:00
categories: js
tags:
---
for...in for...of别再傻傻分不清楚

<!--more-->

### 先来看几个例子

#### 1、遍历对象
```
var obj = { a: 1, b: 2, c: 3};
for (let i in obj){
	console.log(i); //a b c
    console.log(typeof i);  string
}

for ( let i of obj){
	console.log(i); //TypeError: obj is not iterable
}

for ( let i of Object.keys(obj)){
	console.log(i); // a b c
}
```

#### 2、遍历数组
```
Object.prototype.objCustom = function() {}; 
Array.prototype.arrCustom = function() {};
var arr = ['a', 'b', 'c'];
arr.name = 'JS';
for( let i in arr){
	console.log(i); // 0 1 2 name arrCustom objCustom
}

for( let i of arr){
	console.log(i); // a b c
}
```

### 总结
#### for...in
1、遍历数组的所有的**可枚举属性**，包括原型上的，及手动添加的属性。
2、遍历的顺序有可能不是按照实际数组的内部顺序
3、index索引为字符串
4、更适合遍历对象

#### for...of
1、遍历键值
2、一个数据结构只要部属了Symbol.iterator属性，就被视为具有iterator接口，就可以使用for...of进行遍历。
3、可以与break、continue和return配合使用
4、普通对象不具有iterator接口，如果要用for...of遍历普通对象，可以使用Object.keys()
5、有iterator接口的数据结构包括：Array、Map、Set、String、arguments、NodeList对象