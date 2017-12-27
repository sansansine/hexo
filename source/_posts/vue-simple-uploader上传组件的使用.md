---
title: vue-simple-uploader上传组件的使用
date: 2017-08-29 14:27:58
tags:
---
![logo](vue-simple-uploader.png)
<strong>在Vue项目中引入vue-simple-uploader上传组件，后台使用node.js</strong>
[https://github.com/sansansine/semanui.git](https://github.com/sansansine/semanui.git)
<!--more-->
## 1、搭建Vue前端项目
![使用Vue-cli脚手架](http://ot0rfbzcd.bkt.clouddn.com/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202017-08-16%20%E4%B8%8A%E5%8D%889.18.05.png)
## 2、vue-simple-uploader安装
>npm install vue-simple-uploader -save_scroll

## 3、在main.js中引入组件
>import uploader from 'vue-simple-uploader'
>Vue.use(uploader)

## 4、新建up.vue单页
```html
<template>
  <uploader :options="options" class="uploader-example">
    <uploader-unsupport></uploader-unsupport>
    <uploader-drop>
      <p>Drop files here to upload or</p>
      <uploader-btn>select files</uploader-btn>
      <uploader-btn :attrs="attrs">select images</uploader-btn>
      <uploader-btn :directory="true">select folder</uploader-btn>
    </uploader-drop>
    <uploader-list></uploader-list>
  </uploader>
</template>
```
```vue
<script>
  export default {
    data () {
      return {
        options: {
          target: '//localhost:3000/upload',
          testChunks: false
        },
        attrs: {
          accept: 'image/*'
        }
      }
    }
  }
</script>
```
```css
  .uploader-example {
    width: 880px;
    padding: 15px;
    margin: 40px auto 0;
    font-size: 12px;
    box-shadow: 0 0 10px rgba(0, 0, 0, .4);
  }
  .uploader-example .uploader-btn {
    margin-right: 4px;
  }
  .uploader-example .uploader-list {
    max-height: 440px;
    overflow: auto;
    overflow-x: hidden;
    overflow-y: auto;
  }
  .uploader-file-name {
    text-align: left;
}
```
## 5、使用express搭建Node.js后台
[参考地址：https://github.com/simple-uploader/Uploader/tree/develop/samples/Node.js
](https://github.com/simple-uploader/Uploader/tree/develop/samples/Node.js
)
