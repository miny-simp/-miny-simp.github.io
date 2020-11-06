---
title: 全屏-控制嵌套中的iframe全屏
date: 2019-06-18 11:20:57
categories:
- web前端 
tags: 
- js
---
### 普通的全屏方法
<!-- more -->
``` javascript
// 触发f11全屏
$(document).on('keydown', function (e) {
  var e = event || window.event || arguments.callee.caller.arguments[0];
  if(e && e.keyCode == 122){//捕捉F11键盘动作
    e.preventDefault();  //阻止F11默认动作
    var el = document.documentElement;
    var rfs = el.requestFullScreen || el.webkitRequestFullScreen || el.mozRequestFullScreen || el.msRequestFullScreen;//定义不同浏览器的全屏API
　　//执行全屏
    if (typeof rfs != "undefined" && rfs) {
      rfs.call(el);
    } else if(typeof window.ActiveXObject != "undefined"){
      var wscript = new ActiveXObject("WScript.Shell");
      if (wscript!=null) {
          wscript.SendKeys("{F11}");
      }
    }
　　//监听不同浏览器的全屏事件，并件执行相应的代码
    document.addEventListener("webkitfullscreenchange", function() {
      if (document.webkitIsFullScreen) {
        //全屏后要执行的代码
        setTimeout('changeMinOpTop()', 400);
      }else{
        //退出全屏后执行的代码
        changeMinOpTop();
  　　}
    }, false);

    document.addEventListener("fullscreenchange", function() {
      if (document.fullscreen) {
        //全屏后执行的代码
        setTimeout('changeMinOpTop()', 400);
      }else{
        //退出全屏后要执行的代码
        changeMinOpTop();
      }
    }, false);

    document.addEventListener("mozfullscreenchange", function() {
      if (document.mozFullScreen) {
        //全屏后执行的代码
        setTimeout('changeMinOpTop()', 400);
      }else{
        //退出全屏后要执行的代码
        changeMinOpTop();
      }
    }, false);

    document.addEventListener("msfullscreenchange", function() {
      if (document.msFullscreenElement) {
        //全屏后执行的代码
        setTimeout('changeMinOpTop()', 400);
      }else{
        //退出全屏后要执行的代码
        changeMinOpTop();
      }
    }, false)
 }
});
```

### 页面中嵌套的iframe全屏方法
  通过获取iframe的id，设置此iframe置顶。
``` javascript
  // 全屏
  function fullScreen(){
    $('.fullscreen').hide();
    $('.fullscreen_exit').show();
    var iframeId = self.frameElement.getAttribute('id');
    var idDom = top.document.getElementById(iframeId); 
    idDom.style.position = 'fixed';
    idDom.style.top = '0';
    idDom.style.left = '0';
    idDom.style.height = '100%';
    $('#zoom-marker-img').css({width: 'auto', height: '100%'});
    return;
  }
  //退出全屏
  function exitScreen(){
    $('.fullscreen').show();
    $('.fullscreen_exit').hide();
    var iframeId = self.frameElement.getAttribute('id');
    var idDom = top.document.getElementById(iframeId); 
    idDom.style.position = '';
    idDom.style.top = '';
    idDom.style.left = '';
    idDom.style.height = '220px';
    $('#zoom-marker-img').css({width: '100%', height: 'auto', top: '0', left: '0'});
  }
```

