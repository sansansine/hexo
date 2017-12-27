---
title: Introduce the Semantic UI in Vue project
date: 2017-08-11 11:27:42
tags:
---
![logo](semantic-ui.png)
<strong>在Vue项目中引入前端框架semantic-ui</strong>
[https://github.com/sansansine/semanui.git](https://github.com/sansansine/semanui.git)
<!--more-->

1.jQuery install
>npm install --save jquery

2.webpack.dev.config.js添加如下
```Javascript
// plugins 区块内
new webpack.ProvidePlugin({
  $              : "jquery",
  jquery         : "jquery",
  jQuery         : "jquery",
  "window.jQuery": "jquery"
})
```
3.引入jQuery需要填的坑
* webpack.base.conf.js文件添加：
>var webpack = require('webpack')

* main.js里导入jQuery:
>import 'jquery'

* .eslintrc.js文件中添加：
```Javascript
env: {
  // 原有
  browser: true,
  // 添加
  jquery: true
}
```
4.安装 semantic-ui-css
>npm install semantic-ui-css --save

5.在 webpack.dev.config.js中添加
```Javascript
plugins: [
    new webpack.ProvidePlugin({
      ...
      // Semantic-UI
      semantic     : 'semantic-ui-css',
      Semantic     : 'semantic-ui-css',
      'semantic-ui': 'semantic-ui-css'
    }),
  ]
```
6.在main.js中引入 css 和 js 文件
```ruby
import semantic from '../node_modules/semantic-ui-css/semantic.min.js'
import '../node_modules/semantic-ui-css/semantic.min.css'
```
7.finally test it!(add a new .vue)
```html
<template>
  <div class="semantic-component">
    <div class="ui selection dropdown semanticDropDown">
      <input type="hidden" name="gender" v-model="selected">
      <i class="dropdown icon"></i>
      <div class="default text">Gender</div>
      <div class="menu">
        <div class="item" :data-value="item.Value"
             v-for="item in items"
             @click="changeSelection(item)">
          {{ item.Gender }}
        </div>
      </div>
    </div>
    <pre>{{ JSON.stringify(selectedItem) }}</pre>
  </div>
</template>

<script>
  export default {
    data () {
      return {
        items: [
          {Gender: 'Male', Value: 1},
          {Gender: 'Female', Value: 0}
        ],
        selected: '',
        selecteditem: {}
      }
    },
    methods: {
      changeSelection (item) {
        this.selectedItem = item
        this.selected = item.Value
      }
    },
    mounted () {
      this.selecteditem = {}
      $('.semanticDropDown').dropdown()
    }
  }
</script>
```
~~Semantic-UI引入完成~~
