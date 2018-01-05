---
title: css3新特性最佳实践
date: 2017-12-27 15:16:47
tags:
---
# 过渡(transition)
transition: CSS属性,花费时间,效果曲线(默认ease),延迟时间(默认0)
```css
/*宽度从原始值到制定值的一个过渡，运动曲线ease,运动时间5秒，0.2秒后执行过渡*/
.btn{
/*宽度从原始值到制定值的一个过渡，运动曲线ease,运动时间0.5秒，0.2秒后执行过渡*/
transition:width,.5s,ease,.2s;
/*所有属性从原始值到制定值的一个过渡，运动曲线ease,运动时间0.5秒*/
transition:all,.5s
}
```
1. hover效果(button)
```css
.btn:hover{
    transition: all .5s;
    background: #ccc;
}
```
2. 下拉菜单(显示下拉)
设置ul的过渡.ul-transition ul{transform-origin: 0 0;transition: all .5s;}
```css
.ul-transition ul{
    transform-origin: 0 0;
    transition: all .5s;
}
```
# 动画
