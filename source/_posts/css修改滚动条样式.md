---
title: css修改滚动条样式
date: 2020-11-28 15:29:27
categories: css
tags: 转载
---

转载博客

<!--more-->
```
<div class="inner">
    <div class="innerbox">
        <p style="height:200px;">这是内容111</p>
        <p style="height:400px;">这里是内容222</p>
        <p>这里是内容333</p>
    </div>
</div>

.inner{
    width: 265px;
    height: 400px;
    position: absolute;
    top: 33px;
    left: 13px;
    overflow:hidden;
}
.innerbox{
    overflow-x: hidden;
    overflow-y: auto;
    color: #000;
    font-size: .7rem;
    font-family: "\5FAE\8F6F\96C5\9ED1",Helvetica,"黑体",Arial,Tahoma;
    height: 100%;
}
/*滚动条样式*/
.innerbox::-webkit-scrollbar {
    width: 4px;    
    /*height: 4px;*/
}
.innerbox::-webkit-scrollbar-thumb {
    border-radius: 10px;
    -webkit-box-shadow: inset 0 0 5px rgba(0,0,0,0.2);
    background: rgba(0,0,0,0.2);
}
.innerbox::-webkit-scrollbar-track {
    -webkit-box-shadow: inset 0 0 5px rgba(0,0,0,0.2);
    border-radius: 0;
    background: rgba(0,0,0,0.1);

}
```

<img src="/images/css/changeScrollStyle.png" />

滚动条几个属性，主要有下面7个属性

* ::-webkit-scrollbar 滚动条整体部分，可以设置宽度啥的
* ::-webkit-scrollbar-button 滚动条两端的按钮
* ::-webkit-scrollbar-track  外层轨道
* ::-webkit-scrollbar-track-piece  内层滚动槽
* ::-webkit-scrollbar-thumb 滚动的滑块
* ::-webkit-scrollbar-corner 边角
* ::-webkit-resizer 定义右下角拖动块的样式