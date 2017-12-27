---
title: flex布局详解
date: 2017-12-07 17:30:40
tags:
---
<b>Flex是Flexible Box的缩写，意为”弹性盒子布局”。flex 的核心的概念就是 容器 和 轴。容器包括外层的 父容器 和内层的 子容器，轴包括 主轴 和 交叉轴</b>
<!--more-->
# 1、基本概念
采用Flex布局的元素，称为Flex容器（flex container）；它的所有子元素自动成为容器成员，称为Flex项目（flex item）。
![flex](flex-detail.png)
容器默认存在两根轴：水平的主轴（main axis）和垂直的交叉轴（cross axis）。主轴的开始位置（与边框的交叉点）叫做main start，结束位置叫做main end；交叉轴的开始位置叫做cross start，结束位置叫做cross end。
项目默认沿主轴排列。单个项目占据的主轴空间叫做main size，占据的交叉轴空间叫做cross size。
# 2、容器的属性
* flex-direction（决定主轴的方向，即项目的排列方向）
![1](1.png)
![2](2.png)
* flex-wrap(定义，如果一条轴线排不下，如何换行)
（1）nowrap（默认）：不换行；
（2）wrap：换行，第一行在上方；
（3）wrap-reverse：换行，第一行在下方。
* flex-flow
flex-flow属性是flex-direction属性和flex-wrap属性的简写形式，默认值为row nowrap。
*  justify-content(定义项目在主轴上的对齐方式)
![3](3.png)
* align-items(定义项目在交叉轴上如何对齐)
![4](4.png)
* align-content(定义多根轴线时的对齐方式)
![5](5.png)

# 3、项目的属性
* order(定义项目的排列顺序,数值越小，排列越靠前，默认为0)
* flex-grow(定义项目的放大比例，默认为0，即如果存在剩余空间，也不放大)
如果所有项目的flex-grow属性都为1，则它们将等分剩余空间（如果有的话）。如果一个项目的flex-grow属性为2，其他项目都为1，则前者占据的剩余空间将比其他项多一倍。
* flex-shrink(定义项目的缩小比例，默认为1，即如果空间不足，该项目将缩小)
如果所有项目的flex-shrink属性都为1，当空间不足时，都将等比例缩小。如果一个项目的flex-shrink属性为0，其他项目都为1，则空间不足时，前者不缩小。
负值对该属性无效。
* flex-basis(定义在分配多余空间之前，项目占据的主轴空间)
* flex(是flex-grow, flex-shrink 和 flex-basis的简写，默认值为0 1 auto,后两个属性可选)
* align-self(允许单个项目有与其他项目不一样的对齐方式，可覆盖align-items属性。默认值为auto，表示继承父元素的align-items属性，如果没有父元素，则等同于stretch。)
# 4、总结
![flex](flex.png)