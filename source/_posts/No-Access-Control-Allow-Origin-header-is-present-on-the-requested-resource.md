---
title: No Access-Control-Allow-Origin header is present on the requested resource.
date: 2021-01-15 20:53:13
categories: vue
tags:
---

控制台报错`No Access-Control-Allow-Origin header is present on the requested resource.`，后端接口500。

<!--more-->

## 详细报错
`Access to XMLHttpRequest at 'http://microdev.ty.yltyxy.com/index/test' from origin 'http://localhost:8000' has been blocked by CORS policy: Response to preflight request doesn't pass access control check: No 'Access-Control-Allow-Origin' header is present on the requested resource.`

## 问题背景
由于业务需要，用axios封装了一个request并配置了拦截器，用于处理401、500等等状态码并进行错误提示。生产环境下，拦截器拦截到的error.response值为undefined。并且console控制台报错
has been blocked by CORS policy: Response to preflight request doesn't pass access control check: No 'Access-Control-Allow-Origin' header is present on the requested resource.
后端表示在ngix配置了跨域。

## 问题原因
这个问题与前端无关，需要后端支持跨域。
我们这个项目后端用的php，他们添加了一个全局中间件处理跨域就好了。ngix和后端都需要处理。