---
title: "ES6 \_学习笔记"
date: 2017-11-15 17:42:06
tags:
---
<b>ECMAScript 6.0（以下简称 ES6）是 JavaScript 语言的下一代标准，已经在 2015 年 6 月正式发布了。它的目标，是使得 JavaScript 语言可以用来编写复杂的大型应用程序，成为企业级开发语言。</b>
<!--more-->
```javascript
// ES6
input.map(item => item + 1);

// ES5
input.map(function (item) {
  return item + 1;
});
```
# 1、let
 1. 表明函数内部的变量i与循环变量i不在同一个作用域，有各自单独的作用域:
```javascript
for (let i = 0; i < 3; i++) {
  let i = 'abc';
  console.log(i);
}
// abc
// abc
// abc
```
 2. 不存在变量提升
var命令会发生”变量提升“现象，即变量可以在声明之前使用，值为undefined；
let命令改变了语法行为，它所声明的变量一定要在声明后使用，否则报错。
 3. 暂时性死区
```javascript
var tmp = 123;
if (true) {
  tmp = 'abc'; // ReferenceError
  let tmp;
}
```
如果区块中存在let和const命令，这个区块对这些命令声明的变量，从一开始就形成了封闭作用域。凡是在声明之前就使用这些变量，就会报错；
“暂时性死区”也意味着typeof不再是一个百分之百安全的操作.暂时性死区的本质就是，只要一进入当前作用域，所要使用的变量就已经存在了，但是不可获取，只有等到声明变量的那一行代码出现，才可以获取和使用该变量。
 4. 不允许重复声明
let不允许在相同作用域内，重复声明同一个变量,因此，不能在函数内部重新声明参数
 5. 块级作用域
ES5 只有全局作用域和函数作用域，没有块级作用域，这带来很多不合理的场景：  第一种场景，内层变量可能会覆盖外层变量；第二种场景，用来计数的循环变量泄露为全局变量；
ES6 允许块级作用域的任意嵌套，内层作用域可以定义外层作用域的同名变量，外层作用域无法读取内层作用域的变量，块级作用域的出现，实际上使得获得广泛应用的立即执行函数表达式（IIFE）不再必要。
ES6 引入了块级作用域，明确允许在块级作用域之中声明函数。ES6 规定，块级作用域之中，函数声明语句的行为类似于let，在块级作用域之外不可引用。
本质上，块级作用域是一个语句，将多个操作封装在一起，没有返回值，但是，在块级作用域以外，没有办法得到t的值，因为块级作用域不返回值，除非t是全局变量。现在有一个提案，使得块级作用域可以变为表达式，也就是说可以返回值，办法就是在块级作用域之前加上do，使它变为do表达式，然后就会返回内部最后执行的表达式的值。
ES6 的块级作用域允许声明函数的规则，只在使用大括号的情况下成立，如果没有使用大括号，就会报错
```javascript
let x = do {
  let t = f();
  t * t + 1;
};
```
# 2、const(声明一个只读的常量,一旦声明，常量的值就不能改变)
<strong>const实际上保证的，并不是变量的值不得改动，而是变量指向的那个内存地址不得改动</strong>
 1. const一旦声明变量，就必须立即初始化；
 2. const也与let一样只在声明所在的块级作用域内有效，同样存在暂时性死区，只能在声明的位置后面使用；
 3. const也与let一样不可重复声明；
 4. 但对于复合类型的数据（主要是对象和数组），变量指向的内存地址，保存的只是一个指针，const只能保证这个指针是固定的，至于它指向的数据结构是不是可变的，就完全不能控制.
<strong>ES5 只有两种声明变量的方法：var、function；ES6 除了添加let和const命令，还有两种声明变量的方法：import、class，所以，ES6 一共有 6 种声明变量的方法</strong>

# 3、顶层对象
 1. 顶层对象，在浏览器环境指的是window对象，在Node指的是global对象。ES5之中，顶层对象的属性与全局变量是等价的。
 2. ES6 为了改变这一点，一方面规定，为了保持兼容性，var命令和function命令声明的全局变量，依旧是顶层对象的属性；另一方面规定，let命令、const命令、class命令声明的全局变量，不属于顶层对象的属性

# 4、global 对象
在语言标准的层面，引入global作为顶层对象。也就是说，在所有环境下，global都是存在的，都可以从它拿到顶层对象

# 5、数组的结构赋值
<strong>事实上，只要某种数据结构具有 Iterator 接口，都可以采用数组形式的解构赋值/strong>
```javascript
let [a, b, c] = [1, 2, 3];/*等价*/
let a = 1;
let b = 2;
let c = 3;
```
 1. 解构赋值允许指定默认值
 ```javascript
let [x, y = 'b'] = ['a']; // x='a', y='b'
let [x, y = 'b'] = ['a', undefined]; // x='a', y='b
let [x = 1] = [null];/*如果一个数组成员是null，默认值就不会生效，因为null不严格等于undefined。*/
x // null
```
<b>如果默认值是一个表达式，那么这个表达式是惰性求值的，即只有在用到的时候，才会求值。</b>
 2. 对象的解构赋值

                          
                        