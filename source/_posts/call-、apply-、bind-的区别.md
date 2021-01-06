---
title: call()、apply()、bind()的区别
date: 2018-11-14 10:48:01
categories: js
tags:
---
如题，描述call()、apply()、bind()的区别与联系

<!--more-->
#### call()、bind()、apply()的相似之处
1、都是用来改变this指向；
2、都是第一个参数指向this要指向的对象；
3、都可以利用后续参数传参

#### call()、apply()、bind()的区别

	foo.call(obj,a,b,c);
	foo.apply(obj,[a,b,c]);
	foo.bind(obj,a,b,c)();

call()和apply()的区别：call()后面的参数是原来函数的参数列表，apply()后面的参数是一个数组。
call()和bind()的区别：bind()除了返回的是函数以外，其他的跟call()是一样的。