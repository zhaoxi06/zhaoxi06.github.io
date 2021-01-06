---
title: GET与POST的区别
date: 2019-01-07 16:16:22
categories: js
tags:
---
get和post本质上没有区别。get和post是HTTP中发送请求的两种方式，而HTTP是基于TCP/IP的数据传输协议。也就是说，GET/POST都是TCP链接。GET和POST能做的事情是一样的。
<!--more-->
它们最直观的区别就是get参数通过url传递，post放在request body中。但是由于HTTP的规定和浏览器/服务器的限制，导致他们在应用过程中体现出一些不同。

* get请求在url中传递的参数是有长度限制的，而post没有。
* get比post更不安全，因为参数直接暴露在url中，所以不能用来传递敏感信息。
* get请求只能进行url编码，而post支持多种编码方式
* get请求会浏览器主动cache，而post不会，除非手动设置。
* get请求参数会被完整保留在浏览历史记录里，而post中的参数不会被保留。
* get产生一个tcp数据包；post产生两个tcp数据包。

补充解释:
&emsp;&emsp;对于GET方式的请求，浏览器会把http header和data一并发送出去，服务器响应200（返回数据）；而对于POST，浏览器先发送header，服务器响应100 continue，浏览器再发送data，服务器响应200 ok（返回数据）。
&emsp;&emsp;据研究，在网络环境好的情况下，发一次包的时间和发两次包的时间差别基本可以无视。而在网络环境差的情况下，两次包的TCP在验证数据包完整性上，有非常大的优点。
&emsp;&emsp;并不是所有浏览器都会在POST中发送两次包，Firefox就只发送一次。
&emsp;&emsp;URL中数据量太大对浏览器和服务器都是很大负担。业界不成文的规定是，（大多数）浏览器通常都会限制url长度在2K个字节，而（大多数）服务器最多处理64K大小的url。超过的部分，恕不处理。如果你用GET服务，在request body偷偷藏了数据，不同服务器的处理方式也是不同的，有些服务器会帮你处理，读出数据，有些服务器直接忽略，所以，虽然GET可以带request body，也不能保证一定能被接收到。

[原回答](https://www.cnblogs.com/logsharing/p/8448446.html#!comments)
