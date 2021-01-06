---
title: sublime text 3 package control安装
date: 2016-12-16 09:01:53
categories: sublime text 3
tags:
---

这是我第一篇正经的博客~

<!--more-->

使用sublime test 3有段时间了，也就这么将就着用，最近编写代码时出现了烦恼，因为要反复写同样的，比如说：document.createElement\document.getElementsByTagName（以前虽然也这样，但很happy）,每次新建一个文件，总是要自己一个个字母敲上去，不会自动补全，敲快了总是会错啊！（崩溃......）于是下定决心花心思了解sublime test 3,于是有了安装package control
额。。。我的电脑是不是有毒......每次安装各种都会出错，别人只需百度一下，按照网上的方法，瞬间就解决了，但我的电脑总是不行，于是查啊查，锻炼了我解决自己电脑各种安装问题。于是，今天就有了这——sublime text 3 package control安装

<h2>网上大多数人推荐的安装方法有两种：</h2>

<h3>1、代码安装</h3>

从菜单 View - Show Console 或者 ctrl + ~ 快捷键，调出 console。将以下 Python 代码粘贴进去并 enter 执行，不出意外即完成安装。以下提供 ST3 和 ST2 的安装代码：
Sublime Text 3：
``` bash
import urllib.request,os; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); open(os.path.join(ipp, pf), 'wb').write(urllib.request.urlopen( 'http://sublime.wbond.net/' + pf.replace(' ','%20')).read())
```

<h3>2、手动安装：</h3>


可能由于各种原因，无法使用代码安装，那可以通过以下步骤手动安装Package Control：

（1）点击Preferences > Browse Packages菜单

（2）进入打开的目录的上层目录，然后再进入Installed Packages/目录

（3）下载 Package Control.sublime-package 并复制到Installed Packages/目录

（4）重启Sublime Text。

Package Control 主文件下载地址：https://github.com/wbond/sublime_package_control

<img src="/images/st3/download.png">

使用方法：

从菜单 View - Preferences - Package Control ，可以看到：

<img src="/images/st3/install.png">

选择Package Control: Install Package回车，输入想要安装的插件即可


<h2>我在安装Package Control时遇到的一些问题及解决方法：</h2>

<h3>1、用代码安装时报错：</h3>

Warning: mnemonic t not found in menu caption Tools
有的博客说可能是网速的问题~~

<a href="http://www.360doc.com/content/16/0128/14/3242454_531238741.shtml">sublime text无法使用Package Control或插件安装失败的解决方法</a>

<h3>2、手动安装时报错</h3>

参照[Sublime text 3 中Package Control 的安装与使用方法](http://devework.com/sublime-text-3-package-control.html),文件解压后放入对应文件夹，重启ST3，Preferences下出现Package Control,点击，错误提示：需将文件名改为Package Control，更改文件名后再次重启，Preferences下的Package Control消失，额......至今不知为啥？！！
部分网友表示，更改文件名后有用~~

<h3>3、用代码安装时出现错误275309</h3>

解决方法：打开 Preferences->Settings 找到ignored_packages 配置选项，删除其中对 Package Control 的约束即可
如下注释掉"Package Control",即可
```bash
"ignored_packages":
	[
		"ActionScript",
		//"Package Control",
		"Vintage"
	],
```
参照 [如何优雅地使用Sublime Text](http://blog.csdn.net/qq_21577869/article/details/53518878)，亲测有用，问题已解决。终于可以愉快地使用ST3 了
