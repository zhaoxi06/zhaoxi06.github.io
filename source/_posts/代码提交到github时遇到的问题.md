---
title: 代码提交到github时遇到的问题
date: 2018-11-10 11:10:31
categories: js
tags:
---
将平时提交github时遇到的问题记录在这里

<!--more-->

<h4>
 第一个问题：git push时提示 error:faild to push some refs to "xxx"
 如下图：
</h4>
<img src="/images/js/11.png">
**问题原因**
在githu上对版本库进行了在线修改；或者之间在某个库中添加readme文件或者其他文件，但是没有同步到本地库。这时从本地commit到远程github库中时就会出现push失败的问题.
**解决方法**
先把远程库同步到本地库
<code>git pull --rebase origin master</code>
这条指令的意思是把远程库中的更新合并到本地库中，–rebase的作用是取消掉本地库中刚刚的commit，并把他们接到更新后的版本库之中。
[来源](https://blog.csdn.net/mbuger/article/details/70197532)
*********************************************************************************
<h4>
第二个问题：fatal: unable to access 'https://github.com/zhaoxi06/DOM.git/': The requested URL returned error: 403
</h4>
403错误，表示资源不可用。服务器理解客户的请求，但拒绝处理它，通常由于服务器上文件或目录的权限设置导致的WEB访问错误。
**问题原因**
邮箱没有验证