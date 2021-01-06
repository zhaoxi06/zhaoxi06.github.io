---
title: "JS/JQ获取元素的宽高、left\top值"
date: 2018-11-10 11:01:26
categories: js
tags:
---
每次用都要查一下，头疼~还要就是JS跟JQ之间的方法有点混淆。总结如下，后面要用也一目了然。

<!--more-->
<h3>JS</h3>
<p>
可视区的高：document.documentElement.clientHeight;
页面的高：document.documentElement.offsetHeight;
元素样式宽：elem.style.width;
元素可视区宽=样式宽+padding ：elem.clientWidth;
元素占位宽=样式宽+padding+border ：elem.offsetWidth;
元素到定位父级的距离：elem.offsetTop;
滚动条滚动距离：document.documentElement.scrollTop;
				document.body.scrollTop;  (chrom下用)
包含滚动内容的元素高度（元素内容的总高度）：elem.scrollHeight();
鼠标到页面可视区的坐标：ev.clientX;
</p>

<h3>JQ</h3>
<p>
可视区的高：$(window).height();
页面的高：$(document).height();
元素样式宽：width();
元素可视区宽（width+padding）：innerWidth();  
元素占位宽（width+padding+border）：outerWidth();
元素加上margin后的宽度（width+padding+border+margin）：outerWidth(true);
设置宽高：width(x)\innerWidth(x)\outerWidth(x)\outerWidth(x,true)四个方法都可以传递一个参数，即设置
outerWidth()  可以获取到隐藏元素的值
offsetWidth()  获取不到隐藏元素的值
元素相对于整个页面的距离：$(elem).offset().top;
元素到有定位的父级的距离：$(elem).position().top;
滚动条滚动距离：$(document).scrollTop();
鼠标相对于整个页面的坐标：ev.pageY;
鼠标相对于可视区的坐标：ev.clientY;
</p>
<h3>实际中的一些应用:</h3>
<p>
1、滚动条始终保持在最底部
elem.scrollTop = elem.scrollHeight;
2、$(elem).position().left == $(elem).offset().left - $(elem).offsetParent().offset().left;
3、$(document).scrollTop() == $(document).height()-$(window).height();
</p>