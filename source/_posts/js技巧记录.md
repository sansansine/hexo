---
title: js技巧记录
date: 2017-12-11 10:10:22
tags:
---
<b>在工作过程中遇见的一些常用的JavaScript技巧记录</b>
<!--more-->
# 1、jQuery给动态添加的元素绑定事件
jquery 1.7版以后使用on动态绑定事件
```javascript
$("#testdiv ul").on("click","li", function() {
     //do something here
 });
```
# 2、JQuery中html、append、after、before、empty、remove
* xx.html(content):给元素添加html代码或者清空html代码(xx.html());
作用原理首先是移除目标元素内部的html代码，然后将新代码添加到目标元素.
* xx.append(content):append的内容添加到了选中内容的内部，并且对其原有的内容不影响;
作用原理是在被选元素的结尾（仍然在内部）插入指定内容.
* $(content).appendTo(xx):appendTo()和append效果相同，只是写法不同.
* xx.after(content):将html代码插入到指定元素的后面，如果后面有元素，则将元素后移，再插入html代码.
<b>*insertAfter()和after()使用的方法和效果相同,只是写法不同</b>
* before()方法将html代码插入到指定元素前面，如果前面有元素，则将元素前移，再插入html代码。
<b>insertBefore()和before()使用的方法和效果也相同.</b>
* xx.empty():被选中的元素中的html内容都被清除，但元素本身还存在.
* xx.remove():被选中的整个元素包括其内部的子元素都被移除了(remove方法将目标元素整个的从DOM中移除).

# 3、获取html5的data-*属性
* 通过dataset属性访问
```javascript
var appId = $("#myDiv").dataset.appid;//获取data-appid的值
```
* 传统获取方式:getAttribute
```javascript
var appId = div.getAttribute("data-appid");
```
* jQuery获取方法
```javascript
var appid = $("#myDiv").data("appid");
```
# jQuery-toggleClass()
该方法检查每个元素中指定的类。如果不存在则添加类，如果已设置则删除之。这就是所谓的切换效果
```javascript
$('.policy-item').toggleClass('active');
```
# 简洁的JS解决tab跳转
```javascript
/* 共用tab切换 */
    $('.tabtitle .tabitem').click(function () {
        var index = $(this).index();
        $(this).addClass("active").siblings('.tabitem').removeClass("active");
        $(this).parent().parent().find('.tab-item').eq(index).show().siblings('.tab-item').hide();
    });
```
# 判断dom是否有class的值
```javascript
$("html").hasClass("no-js");
```
# 数组去重
```javascript
//Array.from() 方法从一个类似数组或可迭代对象中创建一个新的数组实例
var set1 = Array.from(new Set([1,1,2,2,33,'33',44,'44']));
//等价于
var set1 = [...new Set(arr)];
//(Spread)在函数调用中，...将数组装换成逗号分隔的序列，[]使其成为数组
```
# 深浅拷贝
1. 深拷贝和浅拷贝只针对像Object, Array这样的引用类型数据；
2. 浅拷贝是对对象引用地址进行拷贝，并没有开辟新的栈，也就是拷贝后的结果是两个对象指向同一个引用地址，修改其中一个对象的属性，则另一个对象的属性也会改变；
3. 深拷贝则是开启一个新的栈，两个对象对应两个不同的引用地址，修改一个对象的属性，不会改变另一个对象的属性；
```javascript
//深拷贝,对象转成字符串，再把字符串转成对象
var newArr2=JSON.parse(JSON.stringify(arr));
```
# push.apply合并数组
```javascript
var arr1=[1,2,3,4,5],arr2=[6,7,8,9,10];
arr1.push.apply(arr1,arr2);/* 数组长度有限制,一般不超过10万 */
console.log(arr1)//[1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
```
# 缓存变量
* for循环缓存length
```javascript
var arr=[1,2,3,4,5,6];
for(var i=0;i<arr.length;i++){
}/* 每一次循环的时候，都要查询一次arr.length */
//------------------------分割线
var arr=[1,2,3,4,5,6];
for(var i=0,len=arr.length;i<len;i++){
}/* 缓存了arr.length */
```
* 元素事件
```javascript
$('.div1').click(function(){

});/* 点击一次就要查询一次.div1 */
//--------------------------分割线   
var $div1=$('.div1');/* 缓存了$('.div1') */
$div1.click(function(){

})
```
# 将参数转成数组
函数里的arguments，虽然拥有length属性，但是arguments不是一个数组，是一个类数组，没有push,slice等方法。有些时候，需要把arguments转成数组:
```javascript
var _argument=Array.prototype.slice.apply(arguments);
```
# 如何避免“回调地狱”
一堆以})结尾的金字塔，我们称它为“回调地狱”。
* 减少代码嵌套(给函数命名)
```javascript
document.querySelector('form').onsubmit = formSubmit

function formSubmit (submitEvent) {
  var name = document.querySelector('input').value
  request({
    uri: "http://example.com/upload",
    body: name,
    method: "POST"
  }, postResponse)
}
/* 给函数命名 */
function postResponse (err, response, body) {
  var statusMessage = document.querySelector('.status')
  if (err) return statusMessage.value = err
  statusMessage.value = body
}/* ！函数声明在底部，却仍然能调用，这得益于函数提升。 */
```
* 模块化(module.exports)
module.exports来自node.js的模块系统，可以使用在node、Electron，浏览器上（在部署时需要使用Browserify工具打包）。
```javascript
//formuploader.js
module.exports.submit = formSubmit;

function formSubmit (submitEvent) {
  var name = document.querySelector('input').value;
  request({
    uri: "http://example.com/upload",
    body: name,
    method: "POST"
  }, postResponse)
}

function postResponse (err, response, body) {
  var statusMessage = document.querySelector('.status');
  if (err) return statusMessage.value = err;
  statusMessage.value = body
}
//formuploader.js
var formUploader = require('formuploader');
document.querySelector('form').onsubmit = formUploader.submit
```
* 处理每一个错误
常见错误有几种:语法错误（运行失败）、运行时错误（可以运行但是有bug）、平台错误（文件权限问题、磁盘问题、网络问题）
```javascript
//最常用的回调错误处理是Node.js风格，也就是回调函数的第一个参数总是错误参数
 function handleFile (error, file) {
   if (error) return console.error('Uhoh, there was an error', error)
   // otherwise, continue on and use `file` in your code
 }
```

<b>总结:</b>
1、不要嵌套函数，命名后调用更好；
2、使用函数提升；
3、处理回调函数的每一个错误；
4、创建可重用函数，写成模块，让你更容易读懂代码。

# Browserify简单使用
安装:
> npm install -g browserify

使用:
```javascript
//module.js：
module.exports = 'It works from module.js.';
//module2.js：
module.exports = 'It works from module2.js.';
//entry.js：
var m = require('./module.js');
var m2 = require('./module2.js');
console.log(m, m2);
```
使用browserify编译:
> browserify entry.js > bundle.js

编译好的 js可以直接拿到浏览器使用
```html
<script src="bundle.js"></script>
```




