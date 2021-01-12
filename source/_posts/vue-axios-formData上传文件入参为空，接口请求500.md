---
title: vue+axios+formData上传文件入参为空，接口请求500
date: 2021-01-11 21:41:22
categories: vue
tags:
---
最近在做的项目有上传图片的需求，那就来一个吧。谁知，事情进展并不顺利，formdData上传文件入参为空，接口报500错误。一顿搜索，终于找到了问题所在，在此记录一下。

<!--more-->
## 原因
由于业务需要，axios配置了拦截器，在里面做了数据处理。
```
request.interceptors.request.use(config => {
  // 在发送请求前做数据处理
  return config
}, （error） => {
// 错误处理
return  Promise.reject(error);
})
```

## 解决方法
重新创建一个纯净的axios请求。formData是个不一样的对象，需要纯净的axios请求。formData成功的请求头
```
Cont-Type: 
multipart/form-data; boundary=----WebKitFormBoundaryKlju0v1YzjLJ1CnC
```

## 业务代码
```
import axios from 'axios'

handleOk() {
this.$refs.cropper.getCropData(data =>{
let formData = new FormData()
        formData.append('image', data)
        let _this = this;
        axios({
          method: 'POST',
          url: `xxx`,
          headers: { ..._this.headers },
          data: formData
        }).then(...)
})
}
```

原博客：[萌新用vue + axios + formdata 上传文件的爬坑之路](https://blog.csdn.net/qq_41688165/article/details/80834842)
