---
title: ant design vue pro的使用
date: 2020-11-28 14:40:55
categories: vue
tags:
---

最近在用ant-design-vue-pro，记录下遇到的问题及解决方案

<!--more-->
### 1、如何修改默认的语言？
除了修改[官网](https://github.com/vueComponent/ant-design-vue-pro/blob/master/src/locales/index.js)介绍到的`src/locales/index.js`，
<img src="/images/vue/ant-design-vue-pro/changeLocaleLang.png" />

还需要修改`src/core/bootstrap.js`的
```
store.dispatch('setLang', storage.get(APP_LANGUAGE, 'zh-CN'))
```
<img src="/images/vue/ant-design-vue-pro/changeBootstrapLang.png" />

### 2、如何支持多页签模式？
[ant design vue pro 支持多页签模式](https://blog.csdn.net/sunshu123456/article/details/107773010)
修改 src/layouts/BasicLayout.vue 文件，添加multitab
<img src="/images/vue/ant-design-vue-pro/addMultiTab.png" />
添加属性
<img src="/images/vue/ant-design-vue-pro/addVariable.png" />

### 3、如何实现返回原来的页签不刷新？
在`src/config/router.confit.js`中引入`src/layouts/RouteView.vue`，并且注释掉const定义的RouteView。
路由通过meta中的keepAlive属性来定义是否刷新。
修改`src/layouts/RouteView.vue`中的返回值
<img src="/images/vue/ant-design-vue-pro/changeRouteView.png" />
修改返回值
<img src="/images/vue/ant-design-vue-pro/setKeepAlive.png" />

### 4、如何隐藏面包屑导航？
路由对应的页面不要使用`<page-header-wrapper>`包裹内容

### 5、后端返回的数据结构与默认的不一致如何修改？
修改src/utils/request.js文件
<img src="/images/vue/ant-design-vue-pro/changeResponse.png" />

### 6、如何动态渲染菜单？
修改src/store/index.js中的permission的来源
<img src="/images/vue/ant-design-vue-pro/changePermissionSource.png" />

### 7、Antd is not defined 解决办法
作者配置了按需导入，所以vue组件完整导入配置的时候一直出现这个报错。[Antd is not defined 解决办法](https://blog.csdn.net/qq_39990827/article/details/90700077)。评论区的网友给出了一个简单的解决方法：
在main.js中这样引入
```
import Vue from 'vue'
 
import Antd from 'ant-design-vue/es'
 
Vue.use(Antd)
```

### 8、ant design vue pro指令权限是如何生效的？
[官方文档](https://pro.antdv.com/docs/authority-management)用户登录后，调用获取用户信息的接口，用户信息的role属性中有个permissions属性，它的值是数组。数组中的每一个元素是一个权限对象，如下图表示，在dashboard这个权限下，有新增(add)、查询(query)、详情(get)、修改(update)、删除(delete)这五个权限按钮，当前用户拥有delete删除权限。
<img src="/images/vue/ant-design-vue-pro/rolePermissions.png" />