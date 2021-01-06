---
title: 让div水平垂直居中
date: 2018-11-15 20:04:02
categories: js
tags:
---
让div水平方向和垂直方向居中的几种方法

<!--more-->

####  第一种:设置四个方向的属性值为0，外边距为auto

	#box1{
		width: 100px;
		height: 100px;
		background: orange;
		position: absolute;
		top: 0;
		left: 0;
		right: 0;
		bottom: 0;
		margin: auto;
	}

####  第二种：用margin

	#box1{
		width: 100px;
		height: 100px;
		background: orange;
		position: absolute;
		top: 50%;
		left: 50%;
		margin-top: -50px;
		margin-left: -50px;
	}

####  第三种:用transform

	#box1{
		width: 100px;
		height: 100px;
		background: orange;
		position: absolute;
		top: 50%;
		left: 50%;
		transform: translate(-50%,-50%);
	}	

####  第四种：用flex布局

	#box1{
		height: 300px;
		display: flex;
		align-items: center;
		justify-content: center;
	}
	#box2{
		width: 100px;
		height: 100px;
		background: orange;
	}


