---
title: Vue 生命周期
date: 2019-11-03 20:07:10
categories:
- web前端 
tags: 
- Vue
---

### 一 生命周期  （更新中）
#### 1、beforeCreate  创建前
【注】这个阶段，不能使用 this 变量，data() 中的数据、menthods() 中的方法、watcher() 中的事件都不能获得。
【例】添加 loading 事件，在加载实例时触发
<!-- more -->

#### 2、created 创建完毕
【注】这个阶段，可以操作vue实例中的数据和方法，不能操作 dom 节点
【例】初始化完成时事件，结束 loading 事件，异步请求

#### 3、beforeMounte 挂在前
【注】在挂载开始之前被调用，相关的 render 函数首次被调用。

#### 4、mounted 挂载结束
【注】这个阶段，dom 节点被渲染到文档内，可以操作 dom节点
【例】挂载元素，获取 dom 节点

#### 5、beforeUpdate 更新前
【注】响应式数据更新时调用，发生在虚拟DOM打补丁之前；2、适合在更新之前访问现有的DOM，比如手动移除已添加的事件监听器

#### 6、updated 更新完成
【注】虚拟 DOM 重新渲染和打补丁之后调用，组件DOM已经更新，可执行依赖于DOM的操作。2、避免在这个钩子函数中操作数据，可能陷入死循环
【例】对数据的统一处理

#### 7、beforeDestroy 销毁前
【注】实例销毁之前调用。这一步，实例仍然完全可用，this仍能获取到实例。2、常用于销毁定时器、解绑全局事件、销毁插件对象等操作
【例】确认停止事件的确认框

#### 8、destroyed 销毁完成
【注】实例销毁后调用，调用后，Vue 实例指示的所有东西都会解绑定，所有的事件监听器会被移除，所有的子实例也会被销毁 

### 二 关于 nextTick
【注】在下次 DOM 更新循环结束之后执行延迟回调。在修改数据之后立即使用这个方法，获取更新后的 DOM。
【例】更新数据后立即操作 dom 节点
``` javascript
  created() {
    setTimeout(() => {
      this.number = 100
      this.$nextTick(() => {
        console.log('nextTick', document.getElementsByTagName('p')[0])
      })
    },100)
  }
```
Vue.nextTick() 的用法：
(1) 如果要在 create() 函数中进行 dom 操作，就要将对 dom 的操作放在 Vue.nextTick() 的回调函数中。 原因：create() 函数执行期间 dom 还未渲染，mounted() 函数执行期间，dom 的挂载和渲染都已完成。
(2) 数据变化时执行某一操作，这个操作需要使用随数据变化而变化的 dom 结构，需要将这一操作访入 Vue.nextTick() 的回调函数总才能实现。