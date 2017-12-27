---
title: ES6常用语法
date: 2017-11-15 15:52:05
tags:
---
<strong>总结的es6常用的一些常用的语法和比es5优越的方面，可能只是es6新特性的10%-20%，但是开发上占了80%左右的。</strong>
<!--more-->
# 1、"let"  vs  "var"、"const"
let有块级作用域的的区分概念(只在let命令所在的代码块内有效);
```javascript
//相当于声明了一个全局的i变量。
for(var i=0;i<10;i++){
}
```
```javascript
//j只在这个for循环有效，如果在循环外调用就会报错
for(let j=0;j<10;j++){
    console.log(j)
}
```
const初始化赋值之后就不能再改变赋值,用在引用插件，库，或者模块化开发上,在开发上可以由于重名而带来的异常
```javascript
const url = required('httpUrl');
```
# 2、arrow function(箭头函数)
  当使用箭头函数时，函数体内的this对象，就是定义时所在的对象，而不是使用时所在的对象。原因是箭头函数没有this，它的this是继承外面的，因此内部的this就是外层代码块的this；而ES5的函数里的this是使用时所在的对象。
```javascript
//es6写法-隐式返回
sumArr (arr) 
{
    return arr.reduce((pre, cur) =>pre + cur)
}
//es6写法-显式返回
sumArr(arr) 
{
    return arr.reduce((pre, cur) =>{return pre + cur})
}
//es5写法
sumArr: function(arr) 
{
    return arr.reduce(function (pre, cur) {
        return pre + cur
    })
}
```
# 3、template string（模板字符串:&{}）
```javascript
//ES6
return `剩余时间${d}天${h}小时${m}分钟${s}秒"`;
$("#test").append(
  `<p>
      这是<i>${obj.author}</i>
      写的一个实例。这个实例是为了
      <i> ${obj.thing}</i>
      <span>写作日期是：${obj.time}</span>
   </p>`
);
//ES5
return "剩余时间" + d + "天 " + h + "小时 " + m + " 分钟" + s + " 秒";

```
# 4、destructuring（结构赋值）
```javascript
//es5
var name='守候';
var sex='男';
var info= {name:name, sex: sex};
console.log(info) //Object {name: "守候", sex: "男"}

//es6
let name='守候';
let sex='男';
let info= {name, sex};
console.log(info)  //Object {name: "守候", sex: "男"} 

//es6也可以反过来 
let info={name: "守候", sex: "男"};
let {name,sex}=info;
console.log(name,sex)// "守候" "男"
```
# 5、default（函数参数的默认值）, rest
1. default
参数变量是默认声明的，所以不能用let或const再次声明。
```javascript
function log(x, y = 'world') {
    console.log(x, y);
}

log('Hello');   // Hello world
log('Hello', 'China');  // Hello China
log('Hello', '');   // Hello 
```
2. rest
Rest操作符和Spread操作都是用三个点（...）表示，但作用整好相反。
Rest操作符一般用在函数参数的声明中，而Spread用在函数的调用中；
使用 Rest 参数，ES6 为我们提供一种新的方式来创建可变参数的函数
```javascript
function input(...params){  
    console.log(params)  
}  
  
input(1,2,3,4)  //[1,2,3,4]  
  
function input2(a,b,...params){  
    console.log(params)  
}  
  
input2(1,2,3,4)  //[3,4]  
```
rest参数作用： 将多余的逗号分隔的参数序列转换为数组参数
注意： rest参数必须是最后一个参数，否则报错
<b>将一个数组转为用逗号分隔的参数序列</b>
```javascript
console.log(...[1, 2, 3])  
// 1 2 3  
console.log(1, ...[2, 3, 4], 5)  
// 1 2 3 4 5  
```
# 6、export & import（对应的特性就是，模块化开发）
封装模块的时候，用export把模块暴露出去，需要使用的时候，用import引进行来
# 7、常用API
## 7.1 字符串

1.1 repeat
```javascript
//将原字符串重复n次
'守候'.repeat(3)
```
1.2 includes & startsWith & endsWith
>includes：是否找到了参数字符串,返回布尔值。(ES7)
>startsWith：参数字符串是否在原字符串的头部,返回布尔值。
>endsWith：参数字符串是否在原字符串的尾部,返回布尔值。

三个方法都接受两个参数，第一个参数是参数字符串，第二个是开始检索的位置
```javascript
var str='我就是守候';
str.startsWith('我就是')//true
str.startsWith('我',2)//false
str.endsWith('守候')//true
str.includes('守候')//true
str.includes('我',3)//false
```
1.3 padStart & padEnd(ES8)
>padStart:如果字符串不够指定长度，在头部补全指定字符
>padEnd：如果字符串不够指定长度，在尾部补全指定字符

两个方法都接收两个参数，第一个是指定字符串的最小长度，第二个用来补全的字符串。
如果指定字符串的长度，等于或大于指定的最小长度（第一个参数）。就直接返回原字符串，如果忽略第二个参数，就使用空格补全原字符串！
```javascript
var str='守候';
str.padEnd(10,'123')//"守候12312312"
str.padStart(10,'123')//"12312312守候"
str.padEnd(10)//"守候        "
str.padStart(10)//"        守候"
str.padStart(1)//"守候"
str.padEnd(1)//"守候"
```
1.4 isNaN(检查一个值是否为NaN)
```javascript
Number.isNaN(NaN)//true
Number.isNaN(15)//false
```
1.5 isInteger(判断一个值是否为整数,需要注意的是，比如1和1.0都是整数)
```javascript
Number.isInteger(1)//true
Number.isInteger(1.0)//true
Number.isInteger(1.1)//false
```
1.6 sign(判断一个数到底是正数、负数、还是零)
```javascript
Math.sign(-10)// -1
Math.sign(10)// +1
Math.sign(0)// +0
Math.sign(-0)// -0
Math.sign(NaN)// NaN
Math.sign('10')// +1
Math.sign('守候')// NaN
Math.sign('')// 0
Math.sign(true)// +1
Math.sign(false)// 0
Math.sign(null)// 0
```
1.7 trunc(去除一个数的小数部分，返回整数部分)
```javascript
Math.trunc(1.1)//1
Math.trunc(-1.1)//-1
Math.trunc(-0.1)//-0
Math.trunc('123.456')//123
Math.trunc('守候')//NaN
```
## 7.2 对象
2.1 assign(将源对象（ source ）的所有可枚举属性，复制到目标对象（ target ）)
第一个参数是目标对象，后面的参数都是源对象,如果目标对象与源对象有同名属性，或多个源对象有同名属性，则后面的属性会覆盖前面的属性
```javascript
var info1={name:'守',sex:'男'},info2={name:'候',city:'广州'};
Object.assign(info1,info2)//{name: "候", sex: "男", city: "广州"}
```
2.2 keys、values、entries(ES8)
```javascript
//根据对象自身可遍历的键名进行遍历，返回一个数组
var info={name: "守候", sex: "男", city: "广州"};
Object.keys(info)//["name", "sex", "city"]
//根据对象自身可遍历的键值进行遍历，返回一个数组
Object.values(info)//["守候", "男", "广州"]
//根据对象自身可遍历的键值对进行遍历，返回一个数组
Object.entries(info)//[["name", "守候"],["sex", "男"],["city", "广州"]]
```
## 7.3 数组
3.1 from(将两类对象转为真正的数组)
```javascript
Array.from('守候')//["守", "候"]
//常见的使用方式还有-将Dom集合和arguments转成真正的数组
let oLi = document.querySelectorAll('li');
Array.from(oLi ).forEach(function (item) {
  console.log(item);
});
// arguments对象
function fn() {
  let args = Array.from(arguments);
}
//顺便说下Set
let newSet = new Set(['a', 'b','a','c']);
Array.from(newSet) // ['a', 'b','c'] 
//ES6 新增的数据结构--Set。它类似于数组，但是成员的值都是唯一的，不重复的。
//相信大家很容易想到怎么用了，比如数组去重，用Set实现就简单多了。   
removeRepeatArray(arr) 
{
    //return [Array.from(arr)]
    return [...new Set(arr)]
}
```
3.2 find(找出第一个符合条件的数组成员,如果没找到符合条件的成员就返回underfind)
```javascript
[1, 2, 3, 4].find((n) => n > 2)//3
```
3.3 findIndex(找出第一个符合条件的数组成员的索引)
```javascript
//第一个大于2的成员的索引
[1, 2, 3, 4].findIndex((n) => n > 2)//2
```
3.4 includes(某个数组是否包含给定的值，返回一个布尔值,如果没找到符合条件的成员就返回underfind)
```javascript
//!!ES7
[1, 2, 3].includes(2)//true
[1, 2, 3].includes(5)//false
[1, 2, NaN].includes(NaN)//true
```
# ES7仅有的2个特性
* includes()
```javascript
//使用includes()验证数组中是否存在某个元素
let arr = ['react', 'angular', 'vue'];
if (arr.includes('react'))
{
    console.log('React存在');
}
```
* 指数操作符
```javascript
//使用指数运算符**，就像+、-等操作符一样,替代calculateExponent或者Math.pow()
console.log(7**3);
```

