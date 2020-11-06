---
title: vue导航守卫
date: 2020-01-19 10:04:22
categories:
- web前端 
tags: 
- Vue
---
每个守卫方法接收三个参数：
- to: Route: 即将要进入的目标 路由对象
- from: Route: 当前导航正要离开的路由
- next: Function: 一定要调用该方法来 resolve 这个钩子。执行效果依赖 next 方法的调用参数。
<!-- more -->
``` javascript
beforeRouteEnter (to, from, next) {
    // 在渲染该组件的对应路由被 confirm 前调用
    // 不！能！获取组件实例 `this`
    // 因为当守卫执行前，组件实例还没被创建
  },
  beforeRouteUpdate (to, from, next) {
    // 在当前路由改变，但是该组件被复用时调用
    // 举例来说，对于一个带有动态参数的路径 /foo/:id，在 /foo/1 和 /foo/2 之间跳转的时候，
    // 由于会渲染同样的 Foo 组件，因此组件实例会被复用。而这个钩子就会在这个情况下被调用。
    // 可以访问组件实例 `this`
    next(vm => {
      // 通过 `vm` 访问组件实例
    })
  },
  beforeRouteLeave (to, from, next) {
    // 导航离开该组件的对应路由时调用
    // 可以访问组件实例 `this`
  }
``` 
【注1】beforeRouteEnter 守卫 不能 访问 this，因为守卫在导航确认前被调用,因此即将登场的新组件还没被创建。但是可以通过传一个回调给 next来访问组件实例。在导航被确认的时候执行回调，并且把组件实例作为回调方法的参数。
【注2】beforeRouteEnter 是支持给 next 传递回调的唯一守卫。对于 beforeRouteUpdate 和 beforeRouteLeave 来说，this 已经可用了，所以不支持传递回调，因为没有必要了。
 
### 执行顺序
>  beforeRouteLeave < beforeEach < beforeRouteUpdate < beforeEnter <   beforeRouteEnter < beforeResolve < afterEach
### next作用
使导航守卫队列继续向下迭代
### 离开守卫通常用来禁止用户在还未保存修改前突然离开
``` javascript
beforeRouteLeave (to, from , next) {
  const answer = window.confirm('Do you really want to leave? you have unsaved changes!')
  if (answer) {
    next()
  } else {
    next(false)
  }
}
``` 
### 【例】获取上一个页面路由
``` javascript
beforeRouteEnter(to, from, next) {
   next(vm => {    
      // console.log(to)
      // console.log(from) 
      vm.beforeRoute = from.path
      console.log('from', vm.beforeRoute)
    })
  }
``` 
to : 当前页面路由的信息
from : 上一页面路由的信息
vm : 因为当钩子执行前，组件实例还没被创建，不能获取组件实例 ‘this’
vm 就是当前组件的实例相当于上面的 this，所以在 next 方法里你就可以把 vm 当 this 来用了。