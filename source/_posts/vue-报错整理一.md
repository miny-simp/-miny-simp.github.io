---
title: vue报错整理一
date: 2019-06-19 14:10:45
categories:
- web前端 
tags: 
- Vue
---
  Vue运行中总会遇到各种的报错问题，整理一下

### error awesome@1.0.0 dev: `node build/dev-server.js`
问题：初始化项目时，npm run dev 以后出现报错：@1.0.0 dev: `node build/dev-server.js`，是端口号被占用的原因
解决办法：在config文件夹中的index.js修改端口号为8088，或者其他
<!-- more -->
``` javascript
Error: listen EADDRINUSE :::8080
    at Object._errnoException (util.js:992:11)
    at _exceptionWithHostPort (util.js:1014:20)
    at Server.setupListenHandle [as _listen2] (net.js:1355:14)
    at listenInCluster (net.js:1396:12)
    at Server.listen (net.js:1480:7)
    at Function.listen (F:\workpace\projects\vue-demo-kugou\node_modules\express\lib\application.js:618:24)
    at Object.<anonymous> (F:\workpace\projects\vue-demo-kugou\build\dev-server.js:82:18)
    at Module._compile (module.js:652:30)
    at Object.Module._extensions..js (module.js:663:10)
    at Module.load (module.js:565:32)
    at tryModuleLoad (module.js:505:12)
    at Function.Module._load (module.js:497:3)
    at Function.Module.runMain (module.js:693:10)
    at startup (bootstrap_node.js:191:16)
    at bootstrap_node.js:612:3
npm ERR! code ELIFECYCLE
npm ERR! errno 1
npm ERR! awesome@1.0.0 dev: `node build/dev-server.js`
npm ERR! Exit status 1
npm ERR!
npm ERR! Failed at the awesome@1.0.0 dev script.
npm ERR! This is probably not a problem with npm. There is likely additional logging output above.
```
有时候修改完以后依然会报错，就需要删除掉node_modules文件夹，再 npm install 一遍。

### [Vue warn]: Do not use built-in or reserved HTML elements as component id: Head
问题：vue.esm.js:571 [Vue warn]: Do not use built-in or reserved HTML elements as component id: Head
解决办法：翻译为“不要使用内置或保留的HTML元素作为组件ID：Head”，说明有命名跟html5标签冲突了，打开项目其中一个App.vue组件，将引入的组件<Head \> 的名称改为<HeadYY \> 或者其他名称就可以了，所有Head组件名称改为一致。

### App.vue{ parser: "babylon" } is deprecated; we now treat it as { parser: "babel" }
问题：npm run dev以后提示以上错误
解决办法：比如我的项目名称是vuexDemo,找到工程文件夹里的 vuexDemo\node_modules\vue-loader\lib\template-compiler\index.js,
//将以下代码
``` javascript
if (!isProduction) {
  code = prettier.format(code, { semi: false, parser: 'babylon' })
}
```
修改为：
``` javascript
if (!isProduction) {
  code = prettier.format(code, { semi: false, parser: 'babel' })
}
```
如果没有找到template-compiler文件夹，就需要更新vue-loader的版本
``` bash
  npm install -D vue-loader@14.2.4
```
更新以后就没有以上错误了，但是高版本也可能引起其他问题

### npm install时出现以下错误 npm ERR! chromedriver@2.46.0 install: `node install.js`
问题：
``` javascript
npm ERR! code ELIFECYCLE
npm ERR! errno 1
npm ERR! chromedriver@2.46.0 install: `node install.js`
npm ERR! Exit status 1
npm ERR!
npm ERR! Failed at the chromedriver@2.46.0 install script.
npm ERR! This is probably not a problem with npm. There is likely additional logging output above.
```
解决办法：
``` bash
  npm install chromedriver --chromedriver_cdnurl=http://cdn.npm.taobao.org/dist/chromedriver
```

### [Vue warn]: Computed property "count" was assigned to but it has no setter.
问题：修改count的值时报错
html:
``` html
<template>
  <div>
    <p>{{count}}</p>
  </div>
</template>
```
js:
``` javascript
computed: {
  count() {
    return this.$store.state.count
  }
}
```
原因：vuex中是单项流，v-model是vue中的双向绑定，但是在computed中只通过get获取参数值，没有set无法改变参数值
修改后的 js:
``` javascript
computed: {
  count: {
    get: function() {
      return this.$store.state.count
    },
    set: function() {

    }
  }
}
```
