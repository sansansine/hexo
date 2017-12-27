---
title: 使用Vue.js实现登录拦截
date: 2017-08-12 16:59:50
tags:
---
![logo](vue-lanjie.png)

<strong>Vue.js做前台，node.js做后端，应用Vuex、axios实现的简单登录拦截</strong>
[https://github.com/sansansine/vogue.git](https://github.com/sansansine/vogue.git)
<!--more-->
## 前端
### 1、搭建Vue前端项目
![使用Vue-cli脚手架](http://ot0rfbzcd.bkt.clouddn.com/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202017-08-16%20%E4%B8%8A%E5%8D%889.18.05.png)
### 2、 npm安装element-ui、Vuex、axios等依赖和组件并引入
> npm i element-ui -S
> npm install vuex --save
> npm install axios

** main.js中组件和依赖引入：**
```Javascript
import Vue from 'vue'
import ElementUI from 'element-ui'
import VueAwesomeSwiper from 'vue-awesome-swiper'
import '../theme/index.css'
import App from './App'
import router from './router'
import Vuex from 'vuex'
import store from './vuex/store.js'
import axios from 'axios'

Vue.use(ElementUI)
Vue.use(VueAwesomeSwiper)
Vue.use(Vuex)

```
### 3、新建login.vue单页
```html
<template>
  <div class="login">
    <el-form  :model="loginForm" :rules="rules" ref="loginForm" label-width="80px" >
      <h3 class="login-head">登录</h3>
      <el-form-item prop="name">
        <el-input  v-model="loginForm.name" placeholder="用户名"></el-input>
      </el-form-item>
      <el-form-item prop="pwd">
        <el-input v-model="loginForm.pwd" placeholder="密码"></el-input>
      </el-form-item>
      <el-button type="primary" class="login-btn" @click="submitForm('loginForm')">提交</el-button>
    </el-form>
  </div>
</template>
```

```javascript
<script>
import { mapActions } from 'vuex'
import { USER_SIGNIN } from '../vuex/modules/user'
import api from '../fetch/axios.js'
export default {
  data () {
    return {
      loginForm: {
        name: '',
        pwd: ''
      },
      rules: {
        name: [
          {required: true, message: '用户名必填', trigger: 'blur'}
        ],
        pwd: [
          {required: true, message: '请输入密码', trigger: 'blur'},
          {min: 6, max: 16, message: '密码在6到16位之间', trigger: 'blur'}
        ]
      }
    }
  },
  methods: {
    ...mapActions([USER_SIGNIN]),
    submitForm (formName) {
      this.$refs[formName].validate((valid) => {
        if (valid) {
          console.log('val')
          let opt = this.loginForm
          api.Login(opt)
              .then(res => {
                if (res) {
                  this.$message({
                    type: 'success',
                    message: '登录成功'
                  })
                  this.USER_SIGNIN(this.loginForm)
                  this.$router.replace('/one')
                } else {
                  // element-ui消息提示
                  this.$message({
                    type: 'info',
                    message: '密码错误！'
                  })
                }
              })
        } else {
          console.log('not val')
          return false
        }
      })
    }
  }
}
</script>
```
```css
  .login{
    width: 40%;
    margin: 5% 25% 5% 25%;
    border: 1px solid #f9dba4;
    box-shadow: 0 2px 2px 0 #f3daae;
    padding: 0 60px 20px 60px;
  }
  .login-head{
    margin: 30px 0px 20px 0px;
    text-align: center;
    background: orange;
    padding: 8px 15px;
    color: #fff;
  }
  .login-btn{
    width: 120px;
    margin-left: 165px;
    padding: 8px 15px;
    border-radius: 0;
  }
  .el-form-item__content {
    margin-left: 0 !important;
}
```
## 后台
### 1、安装express
> npm install -g express
> npm install -g express-generator

### 2、在Vue项目下新建文件夹service做服务器
1.新建db.js连接数据库
```Javascript
module.exports = {
  mysql: {
      host: 'localhost',
      user: 'root',
      password: '123456',
      database:'vogue'
  }
}
```
2.新建app.js作为express服务器入口
```Javascript
const userApi = require('./api/userApi');
const fs = require('fs');
const path = require('path');
const bodyParser = require('body-parser');
const express = require('express');
const app = express();

app.use(bodyParser.json());
app.use(bodyParser.urlencoded({extended: false}));

// 设置响应头
app.all('*', function(req, res, next) {
    res.header("Access-Control-Allow-Origin", "*");
    res.header("Access-Control-Allow-Methods","PUT,POST,GET,DELETE,OPTIONS");
    res.header('Access-Control-Allow-Headers','x-requested-with,content-type,Access-Control-Allow-Origin');
    res.header("X-Powered-By",' 3.2.1')
    res.header("Content-Type", "application/json;charset=utf-8");
    next();
});

//注册api路由
app.use('/api/user', userApi);

// 监听端口
app.listen(3000);
console.log('success listen at port:3000......');

```
3.新建sqlMap.js作为SQL语句映射文件供api逻辑调用
```Javascript
var sqlMap = {
    // 用户
    user: {
        add: 'select * from user where name = ? AND pwd = ?'
    }
}
module.exports = sqlMap;
```
4.新建userApi.js作为前端调用接口
```Javascript
var models = require('../db/db');
var express = require('express');
var router = express.Router();
var mysql = require('mysql');
var $sql = require('../db/sqlMap');

// 连接数据库
var conn = mysql.createConnection(models.mysql);

conn.connect();
var jsonWrite = function(res, ret) {
    if(typeof ret === 'undefined') {
        res.json({
            code: '1',
            msg: '操作失败'
        });
    } else {
        res.json(ret);
    }
};

// 增加用户接口
router.post('/addUser', (req, res) => {
    var sql = $sql.user.add;
    var params = req.body;
    console.log(params);
    var success = false;
    conn.query(sql, [params.name, params.pwd], function(err, result) {
      console.log(result);
        if (err) {
          console.log('err');
        }else if (result.length<=0) {
          success = false;
          console.log('fail');
        }else {
          success = true;
        }
        jsonWrite(res, success);

    })
});
// 增加用户接口
router.get('/addUser', (req, res) => {
     res.send('retrunJson');
});

module.exports = router;
```
5.进入service目录下安装相关依赖
>npm install  mysql body-parser

## 跨域声明
** 在Vue项目下找到文件configindex.js，添加：**
```Javascript
proxyTable: {
        '/api': {
            target: 'http://127.0.0.1:3000/api/',
            changeOrigin: true,//true允许跨域
            pathRewrite: {
                '^/api': ''
            }
        }
    }
```
## 登录测试
** 在service目录下启动后台：**
> nodemon app.js

** 启动Vue项目： **
> npm run dev

~~访问http://localhost:8080/#/ 输入并登陆~~
