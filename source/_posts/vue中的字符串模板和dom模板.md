---
title: vue中的字符串模板和dom模板
date: 2021-01-11 21:45:31
categories: vue
tags:
---
vue官方文档中多次提及字符串模板和dom模板，对这个概念不了解，在此记录查阅到的内容。

<!--more-->

## 1.字符串模板
字符串模板就是写在vue中的template中定义的模板，如.vue的单文件组件模板和定义组件时template属性值的模板。字符串模板不会在页面初始化参与页面的渲染，会被vue进行解析编译之后再被浏览器渲染，所以不受限于html结构和标签的命名。
```
Vue.component('MyComponentA', {
    template: '<div MyId="123"><MyComponentB>hello, world</MyComponentB></div>'
})

<div id="app">
    <MyComponentA></MyComponentA>
</div>
```

## 2.dom模板(或者称为Html模板)
dom模板就是写在html文件中，一打开就会被浏览器进行解析渲染的，所以要遵循html结构和标签的命名，否则浏览器不解析也就不能获取内容了。
下面的例子不会被正确渲染, 会被解析成mycomponent,但是注册的vue的组件是MyComponent，因此无法渲染。
```
<!DOCTYPE <html>
    <head>
        <meta charset="utf-8">
        <title>Vue Component</title>
    </head>
    <body>
        <div id="app"> 
            Hello Vue
            <MyComponent></MyComponent>
        </div>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
    <script >
        //全局注册
        Vue.component('MyComponent', {
            template: '<div>组件类容</div>'
        });
        new Vue ({
            el: '#app'
        });
    </script>
    </body>
</html>
```
所以，下面的例子就可以正常显示了：
```
<!DOCTYPE <html>
    <head>
        <meta charset="utf-8">
        <title>Vue Component</title>
    </head>
    <body>
        <div id="app"> 
            Hello Vue
            <my-component></my-component>
        </div>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.5.16/dist/vue.js"></script>
    <script >
        //全局注册
        Vue.component('my-component', {
            template: '<div>组件类容</div>'
        });
        new Vue ({
            el: '#app'
        });
    </script>
    </body>
</html>
```
因为html对大小写不敏感，所以在DOM模板中使用组件必须使用kebab-case命名法(短横线命名)。
因此,对于组件名称的命名，可参考如下实现：
```
/*-- 在单文件组件、JSX和字符串模板中 --*/
<MyComponent/>
/*-- 在 DOM 模板中 --*/
<my-component></my-component>
或者
/*-- 在所有地方 --*/
<my-component></my-component>
```
转载自：[vue中涉及的字符串模板与dom模板](https://www.jianshu.com/p/8c63c93a346b)

## 补充
HTML 中的 attribute 名是大小写不敏感的，所以浏览器会把所有大写字符解释为小写字符。这意味着当你使用 DOM 中的模板时，camelCase (驼峰命名法) 的 prop 名需要使用其等价的 kebab-case (短横线分隔命名) 命名。如果你使用字符串模板，那么这个限制就不存在了。
```
Vue.component('blog-post', {
  // 在 JavaScript 中是 camelCase 的
  props: ['postTitle'],
  template: '<h3>{{ postTitle }}</h3>'
})
<!-- 在 HTML 中是 kebab-case 的 -->
<blog-post post-title="hello!"></blog-post>
```

不同于组件和 prop，事件名不会被用作一个 JavaScript 变量名或 property 名，所以就没有理由使用 camelCase 或 PascalCase 了。并且 v-on 事件监听器在 DOM 模板中会被自动转换为全小写 (因为 HTML 是大小写不敏感的)，所以 v-on:myEvent 将会变成 v-on:myevent——导致 myEvent 不可能被监听到。因此，推荐始终使用 kebab-case 的事件名。