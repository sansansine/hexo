---
title: Promise 初解
date: 2017-12-15 11:04:58
tags:
---
<b>Promise 对象用于一个异步操作的最终完成（或失败）及其结果值的表示。(简单点说就是处理异步请求。Promise就是一个容器，里面保存着某个未来才会结束的事件（通常是一个异步操作）的结果，相对传统方案来说避免了层层嵌套的回调函数</b>
<!--more-->
Promise三种状态:pending(进行中)、fulfilled 或 rejected
# 语法
```javascript
const promise = new Promise(function(resolve, reject) {
  // ... some code

  if (/* 异步操作成功 */){
    resolve(value);
  } else {
    reject(error);
  }
});
```
executor 函数在Promise<b>构造函数执行时同步执行<b>，被传递 resolve 和 reject 函数（executor 函数在Promise构造函数返回新建对象前被调用）。resolve 和 reject 函数被调用时，分别将promise的状态改为fulfilled（完成）或rejected（失败）。executor 内部通常会执行一些异步操作，一旦完成，可以调用resolve函数来将promise状态改成fulfilled，或者在发生错误时将它的状态改为rejected。
如果在executor函数中抛出一个错误，那么该promise 状态为rejected。executor函数的返回值被忽略。
# 特点
（1）对象的状态不受外界影响。Promise对象代表一个异步操作，有三种状态：pending（进行中）、fulfilled（已成功）和rejected（已失败）。只有异步操作的结果，可以决定当前是哪一种状态，任何其他操作都无法改变这个状态。这也是Promise这个名字的由来，它的英语意思就是“承诺”，表示其他手段无法改变。
（2）一旦状态改变，就不会再变，任何时候都可以得到这个结果。Promise对象的状态改变，只有两种可能：从pending变为fulfilled和从pending变为rejected。只要这两种情况发生，状态就凝固了，不会再变了，会一直保持这个结果，这时就称为 resolved（已定型）。如果改变已经发生了，你再对Promise对象添加回调函数，也会立即得到这个结果。这与事件（Event）完全不同，事件的特点是，如果你错过了它，再去监听，是得不到结果的。
Promise 新建后就会立即执行:
```javascript
let promise = new Promise(function(resolve, reject) {
  console.log('Promise');
  resolve();
});

promise.then(function() {
  console.log('resolved.');
});

console.log('Hi!');

// Promise
// Hi!
// resolved
```
# 注意
1、Promise构造函数是同步执行的，promise.then中的函数是异步执行的。
2、.then 或者 .catch 的参数期望是函数，传入非函数则会发生值穿透即把值往后传或者抛。