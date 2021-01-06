---
title: 电脑重装系统后，hexo的快速配置
date: 2017-04-19 20:14:50
tags:
---
上周电脑再次中毒，网上找了N中方法都没解决，最后只能重装系统。装系统是简单，但是软件安装与配置却是个大麻烦，这不重装系统后，我的hexo就不能写博客了~~现在问题解决了，所以把解决方案记录下来，以备后面再用......

<!--more-->

1、安装Git，Node.js
2、安装hexo。鼠标右键任意位置，选择Git Bash Here打开，$ npm install hexo-cli -g
3、$ npm install hexo-deployer-git --save
4、使用SSH url，如果电脑没有开放SSH端口，会导致部署失败。hexo d时，出现如下错误：
	Please make sure you have the correct access rights
	and the repository exists.
	Error: Permission denied (publickey).
	fatal: Could not read from remote repository.
[如何开放SSH端口](http://opiece.me/2015/04/09/hexo-guide/)

ok~完成，完美！