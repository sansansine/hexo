---
title: Vue实例和生命周期的理解
date: 2017-11-23 10:17:12
tags:
---
<strong>每个 Vue 实例在被创建之前都要经过一系列的初始化过程。例如需要设置数据监听、编译模板、挂载实例到 DOM、在数据变化时更新 DOM 等。同时在这个过程中也会运行一些叫做生命周期钩子的函数，给予用户机会在一些特定的场景下添加他们自己的代码。</strong>
<!--more-->
# 1、Vue实例详解
每个 Vue 应用都是通过 Vue 函数创建一个新的 Vue 实例开始的

## 1.1 data对象
数据绑定离不开data里面的数据,也是Vue的核心属性.data对象是Vue绑定数据到HTML标签的数据源泉,Vue会自动将data里面的数据进行递归抓换成getter和setter，然后就可以自动更新HTML标签了

## 1.2 computed
Vue的计算属性（computed)的属性会自动混入Vue的实例中。所有 getter 和 setter 的 this 上下文自动地绑定为 Vue 实例。这就很强大了，再计算属性中定义的函数里面可以直接使用指向了vue实例的this
Vue检测到<strong>数据发生变动</strong>时就会执行对相应数据有引用的函数
应用：利用computed可以做一些监控之类的效果
computed vs methods vs watch
我们可以使用 methods 来替代 computed，效果上两个都是一样的，但是 computed 是基于它的依赖缓存，只有相关依赖发生改变时才会重新取值。而使用 methods ，在重新渲染的时候，函数总会重新调用执行。
computed是在HTML DOM加载后马上执行的，如赋值；
而methods则必须要有一定的触发条件才能执行，如点击事件；
watch呢？它用于观察Vue实例上的数据变动。对应一个对象，键是观察表达式，值是对应回调。值也可以是方法名，或者是对象，包含选项。
所以他们的执行顺序为：默认加载的时候先computed再watch，不执行methods；等触发某一事件后，则是：先methods再watch。

## 1.3 methods
methods 将被混入到 Vue 实例中。可以直接通过 VM 实例访问这些方法，或者在指令表达式中使用。方法中的 this 自动绑定为 Vue 实例。
<strong>注意，不应该使用箭头函数来定义method函数,理由是箭头函数绑定了父级作用域的上下文，所以 this 将不会按照期望指向Vue实例，this.a 将是 undefined</strong>

## 1.4 watch
一个对象，键是需要观察的表达式，值是对应回调函数。值也可以是方法名，或者包含选项的对象。Vue 实例将会在实例化时调用 $watch()，遍历 watch 对象的每一个属性。
```javascript
watch: {
    // 监控a变量变化的时候，自动执行此函数
    a: function (val, oldVal) {
      console.log('new: %s, old: %s', val, oldVal)
    },
    // 深度 watcher
    c: {
      handler: function (val, oldVal) { /* ... */ },
      deep: true //对象内部的属性监听，也叫深度监听,数组的改变不需要使用深度watch
    }
  }
```
<strong>//注意，不应该使用箭头函数来定义 watcher 函数 (例如 searchQuery: newValue => this.updateAutocomplete(newValue))。理由是箭头函数绑定了父级作用域的上下文，所以 this 将不会按照期望指向 Vue 实例，this.updateAutocomplete 将是 undefined。</strong>

## 1.5 设置el的详解
如果这个选项在实例化时有作用，实例将立即进入编译过程，否则，需要显式调用 vm.$mount()(动态添加el)手动开启编译

# 2、Vue实例的生命周期
Vue实例有一个完整的生命周期，也就是从开始创建、初始化数据、编译模板、挂载Dom、渲染→更新→渲染、卸载等一系列过程，我们称这是Vue的生命周期。通俗说就是Vue实例从创建到销毁的过程，就是生命周期

## 2.1 beforeCreate
应用：可以在这加个loading事件 
在实例初始化之后，数据观测 (data observer) 和 event/watcher 事件配置之前被调用

## 2.2 created
应用：在这结束loading，还做一些初始化，实现函数自执行 
实例已经创建完成之后被调用。在这一步，实例已完成以下的配置：数据观测(data observer)，属性和方法的运算， watch/event 事件回调。然而，挂载阶段还没开始，$el 属性目前不可见

## 2.3 beforeMount
在挂载开始之前被调用：相关的 render 函数首次被调用。

## 2.4 mounted
应用：在这发起后端请求，拿回数据，配合路由钩子做一些事情
el 被新创建的 vm.$el 替换，并挂载到实例上去之后调用该钩子。如果 root 实例挂载了一个文档内元素，当 mounted 被调用时 vm.$el 也在文档内。

## 2.5 beforeUpdate
数据更新时调用，发生在虚拟 DOM 重新渲染和打补丁之前。 你可以在这个钩子中进一步地更改状态，这不会触发附加的重渲染过程。

## 2.6 update
由于数据更改导致的虚拟 DOM 重新渲染和打补丁，在这之后会调用该钩子。

## 2.7 beforeDestroy
实例销毁之前调用。在这一步，实例仍然完全可用

## 2.8 destroy
Vue 实例销毁后调用。调用后，Vue 实例指示的所有东西都会解绑定，所有的事件监听器会被移除，所有的子实例也会被销毁。 该钩子在服务器端渲染期间不被调用。
