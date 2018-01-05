---
title: JS随笔
date: 2017-11-22 13:40:48
tags:
---
<strong>在阅读博客过程中的JavaScript疑惑记录</strong>
<!--more-->
# js立即执行函数的写法
立即执行函数其实就是直接调用匿名函数
```javascript
(
    function(){
        alert(1);
    }()
); 

(function(){
    alert(1);
})(); 

!function(){
    alert(1);
}(); 

void function(){
    alert(2);
}(); 
```
# javascript执行机制
<strong>javascript是一门单线程语言，在最新的HTML5中提出了Web-Worker，但javascript是单线程这一核心仍未改变。所以一切javascript版的"多线程"都是用单线程模拟出来的</strong>
1. js任务：
* 同步任务（当我们打开网站时，网页的渲染过程就是一大堆同步任务，比如页面骨架和页面元素的渲染）、
* 异步任务（加载图片音乐之类占用资源大耗时久的任务，就是异步任务）

![执行机制](js.png)
```javascript
let data = [];
$.ajax({
    url:www.javascript.com,
    data:data,
    success:() => {
        console.log('发送成功!');
    }
});
console.log('代码执行结束');
//ajax进入Event Table，注册回调函数success
//执行console.log('代码执行结束')
//ajax事件完成，回调函数success进入Event Queue
//主线程从Event Queue读取回调函数success并执行
```
2. setTimeout(在指定的毫秒数后调用函数或计算表达式)(异步)
```javascript
setTimeout(() => {
    console.log('延时3秒');
},3000)
```
setTimeout(fn,0)的含义是，指定某个任务在主线程最早可得的空闲时间执行，意思就是不用再等多少秒了，只要主线程执行栈内的同步任务全部执行完成，栈为空就马上执行。
3.setInterval
<strong>可按照指定的周期（以毫秒计）来调用函数或计算表达式,setInterval() 方法会不停地调用函数，直到 clearInterval() 被调用或窗口被关闭.</strong>
对于执行顺序来说，setInterval会每隔指定的时间将注册的函数置入Event Queue，如果前面的任务耗时太久，那么同样需要等待
4.Promise与process.nextTick(callback)
<strong>Promise就是一个对象，用来传递异步操作的消息。它代表了某个未来才会知道结果的事件（通常是一个异步操作），并且这个事件提供统一的 API，可供进一步处理</strong>
<strong>process.nextTick(callback)在事件循环的下一次循环中调用 callback 回调函数</strong>
macro-task(宏任务)：包括整体代码script，setTimeout，setInterval
micro-task(微任务)：Promise，process.nextTick
![mask](mask.png)

# javascript预解析
<strong>预解析是把带有var和function关键字的事先声明，但不会赋值</strong>
* var：只要是通过var定义的，不管是变量，还是函数，都是先赋值undefined，如果是变量，也不管变量有没有赋值，在预解析阶段，都是会被赋值为undefined；
* function：function进行预解析的时候，不仅是声明而且还定义（define）了，但是它存储的数据的那个空间里面存储的是代码是字符串，没有任何意义；
  
1.变量声明提前；
2.function优先于var
```javascript
alert(a);
a();
var a=3;
function a(){
    alert(10)
}   
alert(a);
a=6;
a(); 
//刚开始，a就是function a(){alert(10)} ，就会看到这个函数
//alert(10)
//alert(3)
//报错，a已经不是函数

```
```javascript
alert(a);
a();
var a=3;
var a=function(){
    alert(10)
} ;  
alert(a);
a=6;
a();
//undefined
//报错
```
3.声明式函数与赋值式函数，区别在于：在JS的预编译期，声明式函数将会先被提取出来，然后才按顺序执行js代码。
```javascript
f();
function f(){           //声明式函数
 alert(2);
}
f1();
var f1 = function () {  //赋值式函数
 alert(2);
} 
```
作用域：
```javascript
var a=0;
function aa(){
    alert(a);
    var a=3
}
aa();
//underfind  在aa函数里面，有var a=3，那么在aa作用域里面，就是把a这个变量声明提前，但是不会赋值，所以是underfind
```
```javascript
var a=0;
function aa(a){
    alert(a);
    var a=3
}
aa(5);
alert(a)
//5,0   在函数体内，参数a的优先级高于变量a
```
数据排列：
```javascript
function num(n,m){
    console.log(n);
    if(n<m){
        num(n+1,m);
        console.log(n);
    }
}
num(2,5)  //2345432
```
其它：
```javascript
for (var i = 0; i < 5; i++) {
 setTimeout(function() {
  console.log(i);
 }, 1000);
}
console.log(i);
//这个大家就要小心一点了，答案是5    55555
//在setTimeout执行之前，for循环早就执行完了，i的值早已经是5了，所以一开始是执行，最后面的console.log(i);
//在for循环的时候一下子自定义5个setTimeout，大概一秒后，就是输出55555
```
```javascript
for (var i = 0; i < 5; i++) {
 (function(j) { // j = i
  setTimeout(function() {
   console.log(j);
  }, 1000);
 })(i);
}
console.log(i); 
//这里的解析和上面基本一样，只是用闭包来记录每一次循环的i,
//所以答案是5     01234
```
let的局部作用域：
```javascript
for (let i = 0; i < 5; i++) {
 setTimeout(function() {
  console.log(i);
 }, 1000);
}
console.log(i);
//结果是  报错   01234 
```
# JS异常
Javascript执行过程：
1. 当前代码块将作为一个任务压入任务队列中，JS线程会不断地从任务队列中提取任务执行。
2. 当任务执行过程中出现异常，且异常没有捕获处理，则会一直沿着调用栈一层层向外抛出，最终终止当前任务的执行。
3. JS线程会<b>继续</b>从任务队列中提取下一个任务继续执行。

对于 Javascript 而言，我们面对的仅仅只是异常，异常的出现不会直接导致 JS 引擎崩溃，最多只会使当前执行的任务终止。

<b>脚本错误一般分为两种：语法错误(SyntaxError)，运行时错误。几种异常监控的处理方式：</b>
* <b>try-catch 异常处理</b>
通过给代码块进行 try-catch 进行包装后，当代码块发生出错时 catch 将能捕捉到错误的信息，页面也将可以继续执行。但try-catch只能捕获捉到<b>运行时非异步错误</b>，对于语法错误和异步错误就捕捉不到。
* <b>window.onerror 异常处理</b>
window.onerror 捕获异常能力比 try-catch 稍微强点，无论是异步还是非异步错误，onerror 都能捕获到运行时错误,对于语法错误还是无能为力.
```javascript
window.onerror = function (msg, url, row, col, error) {
  console.log('我知道错误了');
  console.log({
    msg,  url,  row, col, error
  });
  return true;
};
error;/* 运行时同步错误 */
window.onerror = function (msg, url, row, col, error) {
  console.log('我知道异步错误了');
  console.log({
    msg,  url,  row, col, error
  });
  return true;
};
setTimeout(() => {
  error;
});/* 异步错误 */
```
1、 对于 onerror 这种全局捕获，最好写在所有 JS 脚本的前面，因为你无法保证你写的代码是否出错，如果写在后面，一旦发生错误的话是不会被 onerror 捕获到的
2、 onerror 是无法捕获到网络异常的错误

<b>在实际的使用过程中，onerror 主要是来捕获预料之外的错误，而 try-catch 则是用来在可预见情况下监控特定的错误，两者结合使用更加高效。</b>
* Promise 错误
通过 Promise 可以帮助我们解决异步回调异常的问题，但是一旦 Promise 实例抛出异常而你没有用 catch 去捕获的话，onerror 或 try-catch 也无能为力，无法捕捉到错误
在写 Promise 实例的时候养成最后写上 catch 函数是个好习惯，如果你的应用用到很多的 Promise 实例的话，最好添加一个 Promise 全局异常捕获事件 unhandledrejection。

# $(window).load()、window.onload=function(){}、$(document).ready()和windo、和document的区别
1、$(window).load() 和window.onload=function(){}是页面中的所有元素（包括图片、flash）等都加载完毕后，才能执行；$(document).ready() 是页面中的DOM元素加载完成后便可执行。
2、$(window).load()和window.onload=function(){}不同的是，前者可以和$(document).ready()一样，可以同时加载多个函数。
3、window代表的是浏览器窗口，即可视的浏览器窗口；document代表的是整个页面的dom元素；即document只是window的一个属性；
4、两者的区别在页面有滚动条时可以直观的显示出来，当出现滚动条时，$(window).height和$(document).height是不相等的，$(document).height比$(window).height大，因为window的高度始终都是可见的浏览器窗口的高度，而document的高度则是整个页面的dom元素的高度，即超出一屏幕了。
5、当某一触发事件，需要页面的所有元素都加载完毕后才执行，并且元素不是通过ajax回调填充的情况下，使用$(window).load()即可。
6、当某一触发事件，需要页面的所有元素都加载完毕后才执行，并且元素是通过ajax回调填充的情况下，使用$(window).load()会出现有时有效，有时无效的情况.