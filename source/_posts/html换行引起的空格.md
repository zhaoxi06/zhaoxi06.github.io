---
title: html换行引起的空格
date: 2018-10-29 16:40:18
categories: js
tags:
---
最近重拾前端，于是从最简单的百度首页开始，写一个界面

<!--more-->

html代码如下：
<img src="/images/js/1.png" />
浏览器显示如下：
<img src="/images/js/2.png" />
问题1：输入框与按钮直接右空格
问题2：按钮比输入框矮了1px
解决办法：
父元素设置font_size:0;   子元素设置好font_size属性，即可解决
<img src="/images/js/3.png" />
解决后浏览器显示
<img src="/images/js/4.png" />