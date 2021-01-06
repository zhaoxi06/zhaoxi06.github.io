---
title: hexo博客迁移到另一台电脑
date: 2020-11-26 21:40:35
tags:
---
之前的电脑太卡了，换了新电脑，需要把博客迁移到新电脑，记录下操作步骤。

<!--more-->
## 操作步骤
### 1、安装必要的软件
[git](https://git-scm.com/)
[nodejs](https://nodejs.org/en/)

### 2、拷贝源文件
将原电脑上个人博客文件夹下这几个文件拷贝到新电脑的路径下：
```
_config.yml
package.json
scaffolds/
source/
themes/
```

### 3、安装hexo
```
npm install hexo-cli -g
```

### 4、在新电脑博客路径下，安装第三方依赖库
```
npm install
npm install hexo-deployer-git --save    // 文章部署到git的模块
npm install // 主题所需要的依赖
```

### 5、github添加 SSH KEYS
首先本地创建 SSH KEYS
```
$ ssh-keygen -t rsa -C "xxx@qq.com"     // 自己github的注册邮箱
```
之后一路回车就行
成功的话会在 ~/下生成 .ssh文件夹，进去，打开 id_rsa.pub，复制里面的key即可。然后拷贝到 Github 的 SSH Keys(这里要添加一个新的)
然后在终端中，我们再次测试下公钥有没有添加成功：ssh -T git@github.com
会弹出确认命令，输入yes,会弹出你的名字等等，这就代表成功了

转载自[如何在另一台电脑上继续hexo写博客](https://blog.csdn.net/wqssh21/article/details/105105705)