---
title: 简单理解Javascript闭包
date: 2017-09-13 14:21:04
tags:
---
![logo](closure.png)
<strong>闭包（Closure）是Javascript的重点，也是难点，在这里记录下来是希望自己能够每看一次便多一次更深刻的理解</strong>
<!--more-->
## 1、闭包的概念
官方对闭包的解释是：一个拥有许多变量和绑定了这些变量的环境的表达式（通常是一个函数），因而这些变量也是该表达式的一部分。
<strong>函数套函数就是闭包吗？不是！，当一个内部函数被其外部函数之外的变量引用时，才会形成了一个闭包。</strong>
要理解闭包，首先必须理解Javascript特殊的变量作用域。
## 2、变量的作用域
Javascript变量的作用于有两种：全局变量和局部变量；
1.函数内部可以直接读取全局变量；
2.在函数外部无法读取函数内的局部变量；
<strong>~~函数内部声明变量的时候，一定要使用var命令，否则会变成全局变量~~</strong>
为了读取函数内部的变量，可以在函数内部定义一个函数，那么子函数就可以读取父函数内的局部变量，再将子函数作为父函数的返回值，最终我们就得到了函数内部的局部变量。
## 3、闭包
```Javascript
function f1(){
　　　　var n=999;
　　　　nAdd=function(){
          n+=1
        }
　　　　function f2(){
　　　　　　alert(n);
　　　　}
　　　　return f2;
　　}
　　var result=f1();
　　result(); // 999
　　nAdd();// nAdd前面没有使用var关键字，因此nAdd是一个全局变量，全局变量的调用者时window。其次，nAdd的值是一个匿名函数，这个匿名函数本身也是一个闭包,可以在函数外部对函数内部的局部变量进行操作
　　result(); // 1000
```
## 4、闭包的用途
1.可以读取函数内部的变量；
2.让变量的值始终保持在内存中；
## 5、使用注意
1.闭包会使得函数中的变量都被保存在内存中，内存消耗很大，所以不能滥用闭包，否则会造成网页的性能问题，在IE中可能导致内存泄露。所以在退出函数之前，将不使用的局部变量全部删除；
2.闭包会在父函数外部，改变父函数内部变量的值。所以，如果你把父函数当作对象（object）使用，把闭包当作它的公用方法，把内部变量当作它的私有属性，这时一定要小心，不要随便改变父函数内部变量的值。
## 6、闭包经典案例
```Javascript
var name = "The Window";
　　var object = {
　　　　name : "My Object",
　　　　getNameFunc : function(){
　　　　　　return function(){
　　　　　　　　return this.name;
　　　　　　};
　　　　}
　　};//object.getNameFunc()是一个返回函数，没有var声明也是一个匿名函数，是全局变量，window调用
　　alert(object.getNameFunc()());//The Window
```
```Javascript
var name = "The Window";
　　var object = {
　　　　name : "My Object",
　　　　getNameFunc : function(){
　　　　　　var that = this;//this为object
　　　　　　return function(){
　　　　　　　　return that.name;
　　　　　　};
　　　　}
　　};//that=this,this=object,so that=object;object调用
　　alert(object.getNameFunc()());//My Object
```
