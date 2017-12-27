---
title: Sass 入门一
date: 2017-11-24 15:10:44
tags:
---
<b>第一次接触Sass，阅读官方文档时的简单记录</b>
<!--more-->
# 1、 安装(mac)
 1. Ruby安装
 Linux和Mac已自带Ruby，不用再安装
 >ruby -v //验证ruby安装成功
 
 2. Sass安装
 >sudo gem install sass
 >sass -v //验证Sass安装成功
 
 3. Sass编译
 >sass input.scss output.css //单文件转换命令
 >sass --watch input.scss:output.css //单文件监听命令
 >sass --watch app/sass:public/stylesheets //如果你有很多的sass文件的目录，你也可以告诉sass监听整个目录：
 
 4. webstorm下设置sass自动编译
 [webstorm下设置sass](https://www.cnblogs.com/cshi/p/5622650.html)
 <b>注意：在webstorm下文件后缀为.scss</b>
 
# 2、Sass的变量
sass让人们受益的一个重要特性就是它为css引入了变量
 1. 变量声明
 如果变量定义在了规则块外边，那么在这个样式表中都可以像 nav规则块那样引用它；
如果变量定义在了nav的{ }规则块内，那么它只能在nav规则块内使用。
```scss
$nav-color: #F90;
nav {
  $width: 100px;
  width: $width;
  color: $nav-color;
}
nav {//编译后
  width: 100px;
  color: #F90;
}
```
 2. 变量引用
 凡是css属性的标准值可存在的地方，变量就可以使用。css生成时，变量会被它们的值所替代。之后，如果你需要一个不同的值，只需要改变这个变量的值，则所有引用此变量的地方生成的值都会随之改变
 ```scss
$highlight-color: #F90;
$highlight-border: 1px solid $highlight-color;
.selected {
  border: $highlight-border;
}
.selected {//编译后
  border: 1px solid #F90;
}
```
 3. 变量名用中划线还是下划线分隔
 <b>Sass兼容两种方式</b>
 
# 3、Sass嵌套
<b>css中重复写选择器是非常恼人的,sass在输出css时会把这些嵌套规则处理好，避免重复书写</b>
```scss
#content {
  article {
    h1 { color: #333 }
    p { margin-bottom: 1.4em }
  }
  #content aside { background-color: #EEE }
}
 /* 编译后 */
#content article h1 { color: #333 }
#content article p { margin-bottom: 1.4em }
#content aside { background-color: #EEE }
```
 1. 父选择器的标识符&
 ```scss
article a {
  color: blue;
  &:hover { color: red }
}/*编译后*/
article a { color: blue }
article a:hover { color: red }
```
2. 群组选择器的嵌套
```scss
.container {
  h1, h2, h3 {margin-bottom: .8em}
}/*编译后*/
.container h1, .container h2, .container h3 { margin-bottom: .8em }
nav, aside {
  a {color: blue}
}/*编译后*/
nav a, aside a {color: blue}
```
3. 子组合选择器和同层组合选择器：>、+和~
子组合选择器>:选择一个元素的<b>直接</b>子元素
同层相邻组合选择器+:选择<b>紧接<b>在另一个元素后的元素，而且二者有相同的父元素
同层全体组合选择器~:选择<b>所有</b>跟在一个元素后的同层元素,不管它们之间隔了多少其他元素
```scss
article > section { border: 1px solid #ccc } //只会选择article下紧跟着的子元素中命中section选择器的元素
header + p { font-size: 1.1em } //选择header元素后紧跟的p元素
article ~ article { border-top: 1px dashed #ccc } //选择所有跟在article后的同层article元素
```
4. 嵌套属性
```scss
nav {
  border: {
  style: solid;
  width: 1px;
  color: #ccc;
  }
}/*编译后*/
nav {
  border-style: solid;
  border-width: 1px;
  border-color: #ccc;
}
nav {
  border: 1px solid #ccc {
  left: 0px;
  right: 0px;
  }
}
```
# 3、导入SASS文件
sass的@import规则在生成css文件时把相关文件导入进来,当你@import一个局部文件时，还可以不写文件的全名，即省略文件名开头的下划线h和后缀。
 
 1. 使用SASS部分文件
 sass局部文件的文件名以下划线开头。这样，sass就不会在编译时单独编译这个文件输出css，而只把这个文件用作导入
 2. 默认变量值
 一般情况下，你反复声明一个变量，只有最后一处声明有效且它会覆盖前边的值;
 !default:如果这个变量被声明赋值了，那就用它声明的值，否则就用这个默认值
 3. 嵌套导入
 sass允许@import命令写在css规则内
 ```scss
 aside {
 background: blue;
 color: white;
 }
 .blue-theme {@import "blue-theme"}/*得到如下：*/
 .blue-theme {
 aside {
  background: blue;
  color: #fff;
 }
 }
 ```
被导入的局部文件中定义的所有变量和混合器，也会在这个规则范围内生效。这些变量和混合器不会全局有效，这样我们就可以通过嵌套导入只对站点中某一特定区域运用某种颜色主题或其他通过变量配置的样式。
4. 原生的CSS导入
由于sass兼容原生的css，所以它也支持原生的CSS@import。可以把原始的css文件改名为.scss后缀，即可直接导入
# 4、静默注释
```scss
body {
  color: #333; // 这种注释内容不会出现在生成的css文件中
  padding: 0; /* 这种注释内容会出现在生成的css文件中 */
}
body {
  color /* 这块注释内容不会出现在生成的css中 */: #333;
  padding: 1; /* 这块注释内容也不会出现在生成的css中 */ 0;
}
```
# 5、混合器
混合器使用@mixin标识符定义，这个标识符给一大段样式赋予一个名字，然后就可以在你的样式表中通过@include来使用这个混合器。
```scss
@mixin rounded-corners {
  -moz-border-radius: 5px;
  -webkit-border-radius: 5px;
  border-radius: 5px;
}
notice {
  background-color: green;
  border: 2px solid #00aa00;
  @include rounded-corners;
}
```
1. 何时使用混合器
在不停地重复一段样式，那就应该把这段样式构造成优良的混合器
2. 混合器中的CSS规则
混合器中不仅可以包含属性，也可以包含css规则，包含选择器和选择器中的属性
3. 给混合器传参
```scss
@mixin link-colors($normal, $hover, $visited) {
  color: $normal;
  &:hover { color: $hover; }
  &:visited { color: $visited; }
}
a {
  @include link-colors(blue, red, green);
}

//Sass最终生成的是：

a { color: blue; }
a:hover { color: red; }
a:visited { color: green; }
```
4. 默认参数值
为了在@include混合器时不必传入所有的参数，我们可以给参数指定一个默认值。参数默认值使用$name: default-value的声明形式，默认值可以是任何有效的css属性值，甚至是其他参数的引用
```scss
@mixin link-colors(
    $normal,
    $hover: $normal,
    $visited: $normal
  )
{
  color: $normal;
  &:hover { color: $hover; }
  &:visited { color: $visited; }
}//@include link-colors(red) $hover和$visited也会被自动赋值为red
```
# 6、使用选择器继承来精简CSS
选择器继承是说一个选择器可以继承为另一个选择器定义的所有样式。这个通过@extend语法实现
```scss
//通过选择器继承继承样式
.error {
  border: 1px solid red;
  background-color: #fdd;
}
.error a{  //应用到.seriousError a
  color: red;
  font-weight: 100;
}
h1.error { //应用到hl.seriousError
  font-size: 1.2rem;
}
.seriousError {
  @extend .error;
  border-width: 3px;
}//.seriousError不仅会继承.error自身的所有样式，任何跟.error有关的组合选择器样式也会被.seriousError以组合选择器的形式继承
```
1. 跟混合器相比，继承生成的css代码相对更少。因为继承仅仅是<b>重复选择器</b>，而不会重复属性，所以使用继承往往比混合器生成的css体积更小。如果你非常关心你站点的速度，请牢记这一点。
2. 继承遵从css层叠的规则。当两个不同的css规则应用到同一个html元素上时，并且这两个不同的css规则对同一属性的修饰存在不同的值，css层叠规则会决定应用哪个样式。相当直观：通常权重更高的选择器胜出，如果权重相同，定义在后边的规则胜出。
<b>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;因为继承只会在生成css时复制选择器，而不会复制大段的css属性。但是如果你不小心，可能会让生成的css中包含大量的选择器复制
避免这种情况出现的最好方法就是不要在css规则中使用后代选择器（比如.foo .bar）去继承css规则</b>



