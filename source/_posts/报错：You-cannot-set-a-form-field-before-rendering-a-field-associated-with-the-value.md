---
title: >-
  报错：You cannot set a form field before rendering a field associated with the
  value
date: 2020-11-28 15:36:16
categories: vue
tags:
---

使用antd的Form组件setFieldsValue时出现这个报错。

<!--more-->

### 原因
1、在form组件渲染之前使用了setFieldsValue
2、注册的表单项数量与setFieldsValue中键值对的个数不一致


### 解决方法
如果是第一个原因，在this.$nextTick()中使用this.form.setFieldsValue。
如果是第二个原因
保证表单项数量与setFieldsValue中键值对的个数一致。
```
this.form.setFieldsValue({
  name: this.data.name,
  age: this.data.age
})
```
或者使用lodash的pick
```
this.form.setFieldsValue(pick(this.data, 'name', 'age'))
```


如果你跟我一样有这样的需求：一个表单中，其中一个表单项的值，会影响另外一个表单项的显示隐藏，导致表单项数量与setFieldsValue中键值对的个数不一致，一直报这个错无法赋值，可以用v-show代替v-if来控制表单的显示隐藏。因为v-show会渲染DOM，v-if不会渲染。