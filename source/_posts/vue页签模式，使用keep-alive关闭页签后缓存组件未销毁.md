---
title: vue页签模式，使用keep-alive关闭页签后缓存组件未销毁
date: 2021-01-20 20:16:53
categories: vue
tags:
---

keep-alive关闭页签后缓存组件未销毁

<!--more-->

## 问题背景
vue项目中使用页签模式，同时使用keep-alive缓存页签，关闭页签后重新打开，缓存内容仍然存在。

## 解决方法
使用keep-alive的include属性，属性值为当前打开的页签。即只缓存当前打开的页签。
页签store
```
import storage from 'store'
import { ALL_TABS } from '@/store/mutation-types'

const multiTab = {
  state: {
    allTabs: []
  },

  mutations: {
    [ALL_TABS]: (state, tabs) => {
      state.allTabs = tabs
      storage.set(ALL_TABS, tabs)
    }
  }
};

export default multiTab
```
页签组件
```
this.$watch('pagesName', () => {
      this.$store.commit(ALL_TABS, this.pagesName)
})
```
路由组件
```
const { $store: { state } } = this
const { multiTab: { allTabs } } = state
<keep-alive :include="allTabs">
   <router-view key="fullPath" />
</keep-alive>
```

## 关联知识点
### `keep-alive`
* `include`：- 字符串或正则表达式。只有名称匹配的组件会被缓存。
* `exclude`： - 字符串或正则表达式。任何名称匹配的组件都不会被缓存。
* `max`： - 数字。最多可以缓存多少组件实例。

`inculde`、`exclude`匹配组件的写法有三种：
```
<!-- 逗号分隔字符串 -->
<keep-alive include="a,b">
  <component :is="view"></component>
</keep-alive>

<!-- 正则表达式 (使用 `v-bind`) -->
<keep-alive :include="/a|b/">
  <component :is="view"></component>
</keep-alive>

<!-- 数组 (使用 `v-bind`) -->
<keep-alive :include="['a', 'b']">
  <component :is="view"></component>
</keep-alive>
```
`inculde`、`exclude`值“a、b”指的是当前这个路由对应组件的name属性值

### vuex store
* state： 数据源，相当于组件中的data
* getters：store的计算属性，相当于组件中的computed
* mutations：更改 Vuex 的 store 中的状态的唯一方法是提交（commit） mutation。处理的都是同步事务
* actions：处理异步操作。通过dispatch方法来触发
* modules：vuex允许将 store 分割成模块（module）。每个模块拥有自己的 state、mutation、action、getter、甚至是嵌套子模块