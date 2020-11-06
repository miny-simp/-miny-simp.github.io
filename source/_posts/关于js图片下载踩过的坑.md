---
title: 关于js图片下载踩过的坑
date: 2019-06-13 15:19:57
categories:
- web前端 
tags: 
- js
---
### HTML <a> download 属性

``` html
<a href="/img/minzz_bg.gif" download="minzz_bg_30">下载图片</a>
```
<!-- more -->
**同源问题：**
  如果url指向同源资源，是正常的。
  如果url指向第三方资源，download会失效，表现和不使用download时一致——浏览器能打开的文件，浏览器会直接打开，不能打开的文件，会直接下载。
**浏览器兼容性问题：**
  只有 Firefox 和 Chrome 支持 download 属性。
  ![](/images/browsersupport.png)

### onclick()方法
``` html
<button class="downloadBtn" type="button" onclick="downloadImg()">下载图片</button>
```
``` javascript
function downloadImg(){
  var img = document.getElementById('zoom-marker-img');   // 获取要下载的图片
  var url = img.src;                              // 获取图片地址
  var a = document.createElement('a');            // 创建一个a节点插入的document
  var event = new MouseEvent('click')             // 模拟鼠标click点击事件
  a.download = 'beautifulGirl'                    // 设置a节点的download属性值
  a.href = url;                                   // 将图片的src赋值给a节点的href
  a.dispatchEvent(event)                          // 触发鼠标点击事件
}
```
### 移动端问题
>微信浏览器：微信内置浏览器不支持外部链接下载，可以进入，但是下载链接全部被封掉了，而且无提示。

因此，需要在外部浏览器打开。

### 正规的简单方法
请后端做一次转发。请求后端接口，返回给前端，前端保存文件。