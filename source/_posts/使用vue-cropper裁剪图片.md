---
title: 使用vue-cropper裁剪图片
date: 2021-01-12 19:50:17
categories: vue
tags:
---
最近在做的项目有裁剪图片、旋转图片的需求，记录下vue-copper的使用。

<!--more-->
## 文档
https://www.npmjs.com/package/vue-cropper

## 安装
`npm install vue-cropper`

## 引入
```
import VueCropper from 'vue-cropper'
Vue.use(VueCropper)
```

## 使用
vue代码
```
<template>
  <div>
// 从本地选择文件上传
    <a-upload name="image"
              list-type="picture-card"
              class="avatar-uploader"
              :show-upload-list="false"
              :customRequest="uploadImage"
              :before-upload="beforeUpload">
      <img v-if="imgUrl"
           :src="imgUrl"
           class="image"
           :alt="title" />
      <div v-else>
        <a-icon :type="imgLoading ? 'loading' : 'plus'" />
        <div class="ant-upload-text">
          上传
        </div>
      </div>
    </a-upload>
// 编辑图片：裁剪、旋转
    <a-modal v-if="visible"
             title="编辑图片"
             :visible="visible">
      <vue-cropper style="height: 360px"
                   ref="cropper"
                   v-bind="optionCropper"></vue-cropper>
      <template slot="footer">
        <a-button @click="handleRotate">
          旋转
        </a-button>
        <a-button @click="handleCancel">
          关闭
        </a-button>
        <a-button type="primary"
                  @click="handleOk">
          裁剪
        </a-button>
      </template>
    </a-modal>
  </div>
</template>

export default {
  name: 'ImageUpload',
  data () {
    return {
      imgLoading: false,
      imgUrl: this.url,
      visible: false,
      optionCropper: {
        img: '',
        autoCrop: true,
        autoCropWidth: '300px',
        autoCropHeight: '300px',
      },
    }
  },
  methods: {

//从本地选择文件上传
    uploadImage (file) {
      this.optionCropper.img = URL.createObjectURL(file.file);
      this.visible = true;
    },

// 旋转图片
    handleRotate () {
      this.$refs.cropper.rotateRight(90);
    },

// 确认裁剪，调用接口保存图片
    handleOk () {
      this.$refs.cropper.getCropData(data => {
        this.imgLoading = true;
        let formData = new FormData()
        formData.append('image', data)
        let _this = this;
        axios({... })
      });
}
// 上传前的校验
    beforeUpload (file) {
      const isJpgOrPng = file.type === 'image/jpeg' || file.type === 'image/jpg' || file.type === 'image/png'
      if (!isJpgOrPng) {
        this.$message.error('只能上传jpg/png格式的头像!')
      }
      const isLt2M = file.size / 1024 / 1024 < 2
      if (!isLt2M) {
        this.$message.error('图片不得大于2MB!')
      }
      return isJpgOrPng && isLt2M
    },
  }
}
</script>
```

## 效果
<img src="/images/vue/4.png" />

## 知识点记录
1、`URL.createObjectURL()`
URL.createObjectURL() 静态方法会创建一个 DOMString，其中包含一个表示参数中给出的对象的URL。这个 URL 的生命周期和创建它的窗口中的 document 绑定。这个新的URL对象表示指定的 File 对象或 Blob 对象。
语法
```
URL.createObjectURL(object)
```