---
title: vue中.sync的使用
date: 2021-01-13 20:27:23
categories: vue
tags:
---

vue新手看官网介绍.sync修饰符，尝试在项目中使用。结果仍然报错mutating a prop directly since the value will be overwritten whenever the parent component re-renders. Instead, use a data or computed property based on the prop's value. Prop being mutated: "visible"。一定是我使用姿势不对，在此记录正确的使用姿势，给跟我一样错误解读官网文档的同学指条明路。😎

<!--more-->

## 使用场景
点击父组件的按钮，打开子组件（子组件是一个modal），modal关闭时，通过$emit方法告诉父组件关闭子组件。

## 示例代码
父组件
```
<template>
    <div>
        <child :visible="visible"></child>
        <a-button @click="toogleChild">toogle</a-button>
    </div>
</template>

<script>
data(){
    return {
        visible: false,
    }
    },
    methods: {
        toogleChild(){
        this.visible = !this.visible;
    }
}
</script>
```
子组件
```
<template>
    <a-modal :visible="visible" @cancel="handleCancel">
        // content
    </a-modal>
</template>

<script>
props: ['visible'],
method: {
    handleCancel(){
        // 子组件需要通过$emit来更新visible
        this.$emit('update:visible', false);
    }
}

</script>
```