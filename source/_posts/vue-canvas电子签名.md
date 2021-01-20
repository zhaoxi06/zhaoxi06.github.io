---
title: vue + canvas电子签名
date: 2021-01-14 20:30:37
categories: vue
tags:
---
最近在做的项目要做移动设备电子签名的功能，实现时参考了以下代码。移动端、pc端均可使用。

<!--more-->

## 原理
组件被挂载后，创建一个在画布上绘图的环境。移动端监听`touchstart`、`touchmove`、`touchend`，pc端监听`mousedown`、`mousemove`、`mouseup`事件，调用canvas的api进行绘制。调用`toDataURL`获得图片地址，用于保存或者回传后台。

## 代码

```
<template>
  <section class="signature">
    <div class="signatureBox">
      <div class="canvasBox"
           ref="canvasHW">
        <canvas ref="canvasF"
                @touchstart='touchStart'
                @touchmove='touchMove'
                @touchend='touchEnd'
                @mousedown="mouseDown"
                @mousemove="mouseMove"
                @mouseup="mouseUp"></canvas>
        <div class="btnBox">
          <a-space>
            <a-button type="danger"
                      @click="overwrite">重写</a-button>
            <a-button type="primary"
                      @click="commit">提交签名</a-button>
            <a-button @click="close">关闭</a-button>
          </a-space>

        </div>
      </div>
    </div>
    <img class="imgCanvas"
         :src="imgUrl">
  </section>
</template>

<script>
export default {
  name: 'Sign',
  data () {
    return {
      stageInfo: '',
      imgUrl: '',
      client: {},
      points: [],
      canvasTxt: null,
      startX: 0,
      startY: 0,
      moveY: 0,
      moveX: 0,
      endY: 0,
      endX: 0,
      w: null,
      h: null,
      isDown: false,
      isViewAutograph: this.$route.query.isViews > 0,
      contractSuccess: this.$route.query.contractSuccess
    }
  },
  mounted () {
    let canvas = this.$refs.canvasF
    canvas.height = this.$refs.canvasHW.offsetHeight - 500
    canvas.width = this.$refs.canvasHW.offsetWidth
    this.canvasTxt = canvas.getContext('2d')
    this.stageInfo = canvas.getBoundingClientRect()
  },
  methods: {
    //mobile
    touchStart (ev) {
      ev = ev || event
      ev.preventDefault()
      if (ev.touches.length == 1) {
        let obj = {
          x: ev.targetTouches[0].clienX,
          y: ev.targetTouches[0].clientY,
        }
        this.startX = obj.x
        this.startY = obj.y
        this.canvasTxt.beginPath()
        this.canvasTxt.moveTo(this.startX, this.startY)
        this.canvasTxt.lineTo(obj.x, obj.y)
        this.canvasTxt.stroke()
        this.canvasTxt.closePath()
        this.points.push(obj)
      }
    },
    touchMove (ev) {
      ev = ev || event
      ev.preventDefault()
      if (ev.touches.length == 1) {
        let obj = {
          x: ev.targetTouches[0].clientX - this.stageInfo.left,
          y: ev.targetTouches[0].clientY - this.stageInfo.top
        }
        this.moveY = obj.y
        this.moveX = obj.x
        this.canvasTxt.beginPath()
        this.canvasTxt.moveTo(this.startX, this.startY)
        this.canvasTxt.lineTo(obj.x, obj.y)
        this.canvasTxt.stroke()
        this.canvasTxt.closePath()
        this.startY = obj.y
        this.startX = obj.x
        this.points.push(obj)
      }
    },
    touchEnd (ev) {
      ev = ev || event
      ev.preventDefault()
      if (ev.touches.length == 1) {
        let obj = {
          x: ev.targetTouches[0].clientX - this.stageInfo.left,
          y: ev.targetTouches[0].clientY - this.stageInfo.top
        }
        this.canvasTxt.beginPath()
        this.canvasTxt.moveTo(this.startX, this.startY)
        this.canvasTxt.lineTo(obj.x, obj.y)
        this.canvasTxt.stroke()
        this.canvasTxt.closePath()
        this.points.push(obj)
      }
    },
    //pc
    mouseDown (ev) {
      ev = ev || event
      ev.preventDefault()
      if (1) {
        let obj = {
          x: ev.offsetX,
          y: ev.offsetY
        }
        this.startX = obj.x
        this.startY = obj.y
        this.canvasTxt.beginPath()
        this.canvasTxt.moveTo(this.startX, this.startY)
        this.canvasTxt.lineTo(obj.x, obj.y)
        this.canvasTxt.stroke()
        this.canvasTxt.closePath()
        this.points.push(obj)
        this.isDown = true
      }
    },
    mouseMove (ev) {
      ev = ev || event
      ev.preventDefault()
      if (this.isDown) {
        let obj = {
          x: ev.offsetX,
          y: ev.offsetY
        }
        this.moveY = obj.y
        this.moveX = obj.x
        this.canvasTxt.beginPath()
        this.canvasTxt.moveTo(this.startX, this.startY)
        this.canvasTxt.lineTo(obj.x, obj.y)
        this.canvasTxt.stroke()
        this.canvasTxt.closePath()
        this.startY = obj.y
        this.startX = obj.x
        this.points.push(obj)
      }
    },
    mouseUp (ev) {
      ev = ev || event
      ev.preventDefault()
      if (1) {
        let obj = {
          x: ev.offsetX,
          y: ev.offsetY
        }
        this.canvasTxt.beginPath()
        this.canvasTxt.moveTo(this.startX, this.startY)
        this.canvasTxt.lineTo(obj.x, obj.y)
        this.canvasTxt.stroke()
        this.canvasTxt.closePath()
        this.points.push(obj)
        this.points.push({ x: -1, y: -1 })
        this.isDown = false
      }
    },
    //重写
    overwrite () {
      this.canvasTxt.clearRect(0, 0, this.$refs.canvasF.width, this.$refs.canvasF.height)
      this.points = []
    },
    //提交签名
    commit () {
      this.imgUrl = this.$refs.canvasF.toDataURL();
      console.log(this.$refs.canvasF.toDataURL()) //签名img回传后台
    },
    // 关闭
    close () {
      this.$message.info('关闭签名')
    }
  }
}
</script>

<style lang="less" scoped>
.signatureBox {
  width: 100%;
  height: calc(100% - 50px);
  box-sizing: border-box;
  overflow: hidden;
  background: #fff;
  z-index: 100;
  display: flex;
  flex-direction: column;
}
.canvasBox {
  box-sizing: border-box;
  flex: 1;
}
.btnBox {
  padding: 10px;
  text-align: center;
}
</style>

```

## 效果预览
<img src="/images/vue/5.png" />


原博客：[Vue Canvas 实现电子签名 手写板](https://blog.csdn.net/weixin_43953710/article/details/99643244)