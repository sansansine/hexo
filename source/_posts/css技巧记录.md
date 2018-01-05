---
title: css技巧记录
date: 2017-12-25 16:35:56
tags:
---
---
title: css技巧记录
date: 2017-12-25 16:00:07
tags:
---
1、 使用CSS复位
在不同的浏览器上保持一致的样式风格
```css
* {
  box-sizing: border-box;//box-sizing管理CSS盒模型布局
  margin: 0;
  padding: 0;
}
```
2、 使用 :not() 选择器来决定表单是否显示边框
```css
/* 添加边框 */
.nav li:not(:last-child) {
  border-right: 1px solid #666;
}/* .nav li + li或者 .nav li:first-child ~ li  */
```
3、 为 body 元素添加行高(在body里面同一行高)
```css
body {
  line-height: 1.5;
}/* 不必为每一个 <p>，<h*> 元素逐一添加 line-height，直接添加到 body 元素 ,文本元素可以很容易地继承 body 的样式*/
```
4、 垂直居中任何元素
```css
html, body {
  height: 100%;
  margin: 0;
}

body {
  -webkit-align-items: center;  
  -ms-flex-align: center;  
  align-items: center;
  display: -webkit-flex;
  display: flex;
}
```
5、 逗号分隔列表
```css
ul > li:not(:last-child)::after {
  content: ",";
}/* 使列表的每项都由逗号分隔 */
```
6、 使用负的 nth-child 来选择元素

```css
/* 使用负的 nth-child 可以选择 1 至 n 个元素 */
/* 方法一 */
li {
  display: none;
}
/* 选择第 1 至第 3 个元素并显示出来 */
li:nth-child(-n+3) {
  display: block;
}
/* 方法二 */
/* 选择第 1 至第 3 个元素并显示出来 */
li:not(:nth-child(-n+3)) {
  display: none;
}
```
7、 使用 SVG 图标
```css
.logo {
  background: url("logo.svg");
}/* SVG 在所有分辨率下都可以良好缩放，并且支持所有 IE9 以后的浏览器 */
/* 针对仅有图标的按钮，如果 SVG 没有加载成功的话，以下样式对无障碍有所帮助 */
.no-svg .icon-only:after {
  content: attr(aria-label);
}
```
8、 通用选择器 (*) 和 相邻兄弟选择器 (+) 一起使用
```css
* + * {
  margin-top: 1.5em;
}/* 文档流中的所有的相邻兄弟元素将都将设置 margin-top: 1.5em 的样式 */
```
9、 使用 max-height 来建立纯 CSS 的滑块
```css
.slider {
  max-height: 200px;
  overflow-y: hidden;
  width: 300px;
}

.slider:hover {
  max-height: 600px;
  overflow-y: scroll;
}
```
10、 创造格子等宽的表格
```css
.calendar {
  table-layout: fixed;
}/* table-layout: fixed 可以让每个格子保持等宽 */
```
11、 利用 Flexbox 去除多余的外边距
```css
.list {
  display: flex;
  justify-content: space-between;
}

.list .person {
  flex-basis: 23%;
}
```
12、 利用 Flexbox 去除多余的外边距
与其使用 nth-， first-， 和 last-child 去除列之间多余的间隙，不如使用 flexbox 的 space-between 属性：
```css
.list {
  display: flex;
  justify-content: space-between;/* 定义项目在主轴上的对齐方式:两端对齐，项目之间的间隔相等 */
}

.list .person {
  flex-basis: 23%;/* 定义在分配多余空间之前，项目占据的主轴空间 */
}
```
13、 利用属性选择器来选择空链接
```css
a[href^="http"]:empty::before {
  content: attr(href);
}/* 当 <a> 元素没有文本内容，但有 href 属性的时候，显示它的 href 属性 */
```
14、 给 “默认” 链接定义样式
```css
a[href]:not([class]) {
  color: #008000;
  text-decoration: underline;
}
```
15、 一致垂直节奏
```css
.intro > * {
  margin-bottom: 1.25rem;
}/* 通用选择器 (*) 跟元素一起使用，可以保持一致的垂直节奏 */
```
16、 固定比例盒子
```css
.container {
  height: 0;
  padding-bottom: 20%;
  position: relative;
}

.container div {
  border: 2px dashed #ddd;  
  height: 100%;
  left: 0;
  position: absolute;
  top: 0;
  width: 100%;
}/* 使用20％的padding-bottom使得框等于其宽度的20％的高度。与视口宽度无关，子元素的div将保持其宽高比（100％/ 20％= 5:1） */
```
17、 为破碎图象定义样式
```css
img {  
  display: block;
  font-family: Helvetica, Arial, sans-serif;
  font-weight: 300;
  height: auto;
  line-height: 2;
  position: relative;
  text-align: center;
  width: 100%;
}/* 以添加伪元素的法则来显示用户信息和URL的引用： */
img:before {  
  content: "We're sorry, the image below is broken :(";
  display: block;
  margin-bottom: 10px;
}

img:after {  
  content: "(url: " attr(src) ")";
  display: block;
  font-size: 12px;
}
```
18、 用 rem 来调整全局大小；用 em 来调整局部大小
```css
h2 { /* 在根元素设置基本字体大小后 (html { font-size: 100%; }), 使用 em 设置文本元素的字体大小: */
  font-size: 2em;
}

p {
  font-size: 1em;
}/* 然后设置模块的字体大小为 rem： */
article {
  font-size: 1.25rem;
}

aside .module {
  font-size: .9rem;
}/* 现在，每个模块变得独立，更容易、灵活的样式便于维护。 */
```
19、 隐藏没有静音、自动播放的影片
```css
video[autoplay]:not([muted]) {/* 避免在加载页面时自动播放。如果没有静音，则不显示视频： */
  display: none;
}
```
20、 使用选择器:root来控制字体弹性
```css
:root {/* 在响应式布局中，字体大小应需要根据不同的视口进行调整。你可以计算字体大小根据视口高度的字体大小和宽度，这时需要用到:root: */
  font-size: calc(1vw + 1vh + .5vmin);
}/* 现在，您可以使用 root em： */
body {
  font: 1rem/1.6 sans-serif;
}
```
21、 为更好的移动体验，为表单元素设置字体大小
```css
input[type="text"],
input[type="number"],
select,
textarea {
  font-size: 16px;
}/* 当触发<select>的下拉列表时，为了避免表单元素在移动浏览器（ios Safari 等等）上的缩放，加上font-size： */
```
22、 使用text-indent来隐藏文本
```css
h1 {/* 使用“text-indent”我们可以达到图片替换文本的效果，而且方便搜索引擎的优化，还能支持阅读器阅读网页内容 */
       text-indent:-9999px;
    margin:0 auto;
    width:400px;
    height:100px;
    background:transparent url("images/logo.jpg") no-repeat scroll;
}
```
23、 根据文件格式设置链接图标
```css
a[href^="http:"] {
        display:inline-block;
        padding-right:14px;
        background:transparent url(/Images/ExternalLink.gif) center right no-repeat;
}
a[href^="mailto:"] {
        display:inline-block;
        padding-left:20px;
        line-height:18px;
        background:transparent url(/Images/MailTo.gif) center left no-repeat;
}
a[href$='.pdf'] {
        display:inline-block;
        padding-left:20px;
        line-height:18px;
        background:transparent url(/Images/PDFIcon.gif) center left no-repeat;
}
a[href$='.swf'], a[href$='.fla'], a[href$='.swd'] {
        display:inline-block;
        padding-left:20px;
        line-height:18px;
        background:transparent url(/Images/FlashIcon.gif) center left no-repeat;
}
a[href$='.xls'], a[href$='.csv'], a[href$='.xlt'], a[href$='.xlw'] {
        display:inline-block;
        padding-left:20px;
        line-height:18px;
        background:transparent url(/Images/ExcelIcon.gif) center left no-repeat;
}
a[href$='.ppt'], a[href$='.pps'] {
        display:inline-block;
        padding-left:20px;
        line-height:18px;
        background:transparent url(/Images/PowerPointIcon.gif) center left no-repeat;
}
a[href$='.doc'], a[href$='.rtf'], a[href$='.txt'], a[href$='.wps'] {
        display:inline-block;
        padding-left:20px;
        line-height:18px;
        background:transparent url(/Images/WordDocIcon.gif) center left no-repeat;
}
a[href$='.zip'], a[href$='.gzip'], a[href$='.rar'] {
        display:inline-block;
        padding-left:20px;
        line-height:18px;
        background:transparent url(/Images/ZIPIcon.gif) center left no-repeat;
}
```
24、 在IE浏览器中删除textarea的滚动条
```css
textarea{
    overflow:auto;
    }
```
25、 段落首字下沉
```css
p:first-letter{
    display:block;
    margin:5px 0 0 5px;
    float:left;
    color:#FF3366;
    font-size:60px;
    font-family:Georgia;
}
```
26、 所有浏览器下的CSS透明度
```css
.transparent {
    /* Fallback for web browsers that doesn't support RGBa */
    background: rgb(0, 0, 0);
    /* RGBa with 0.6 opacity */
    background: rgba(0, 0, 0, 0.6);
    /* For IE 5.5 - 7*/
    filter:progid:DXImageTransform.Microsoft.gradient(startColorstr=#99000000, endColorstr=#99000000);
    /* For IE 8*/
    -ms-filter: "progid:DXImageTransform.Microsoft.gradient(startColorstr=#99000000, endColorstr=#99000000)";
}
```
27、 图片预加载
```css
#preloader {/* 这样当某个元素需要时，他就已经被加载了，此时就不会有任何延误或闪烁的现像：*/
    background-image: url(image1.jpg);
    background-image: url(image2.jpg);
    background-image: url(image3.jpg);
    width: 0px;
    height: 0px;
    display: inline;
}
```
28、 固定页脚
```css
#footer {
    position:fixed;
    left:0px;
    bottom:0px;
    height:30px;
    width:100%;
    background:#999;
}
/* IE 6 */
* html #footer {
    position:absolute;
    top:expression((0-(footer.offsetHeight)+(document.documentElement.clientHeight ? document.documentElement.clientHeight : document.body.clientHeight)+(ignoreMe = document.documentElement.scrollTop ? document.documentElement.scrollTop : document.body.scrollTop))+'px');
}	
```
29、 翻转图片
```css
img.flip {
    -moz-transform: scaleX(-1);
    -o-transform: scaleX(-1);
    -webkit-transform: scaleX(-1);
    transform: scaleX(-1);
    filter: FlipH;
    -ms-filter: "FlipH";
}
```
30、 clearfix
```css
.clearfix:after{
    content:"";
    height:0;
    line-height:0;
    display:block;
    visibility:hidden;
    clear:both ;
}
.clearfix{
    zoom:1;
}
```
31、 简单的文字模糊效果
```css
*{ 
    color: transparent;
    text-shadow: #111 0 0 5px;
}
```
32、 鼠标移进网页里消失
```css
*{
    cursor: none!important;
}
```
33、 多重边框
```css
.div {
    box-shadow: 0 0 0 6px rgba(0, 0, 0, 0.2), 0 0 0 12px rgba(0, 0, 0, 0.2), 0 0 0 18px rgba(0, 0, 0, 0.2), 0 0 0 24px rgba(0, 0, 0, 0.2);
    height: 200px;
    margin: 50px auto;
    width: 400px
}
```