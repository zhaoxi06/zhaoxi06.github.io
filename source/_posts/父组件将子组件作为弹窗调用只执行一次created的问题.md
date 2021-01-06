---
title: 父组件将子组件作为弹窗调用只执行一次created的问题
date: 2020-11-28 15:23:01
categories: vue
tags: 
---

用v-if将子组件包裹起来，因为v-if=false时可以将子组件销毁掉，再次调用时重新渲染