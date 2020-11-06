---
title: CSS中文本溢出文字显示
date: 2019-06-18 17:07:17
categories:
- web前端 
tags: 
- CSS
---
## CSS文本溢出和文字位置设置
  平时用的比较多的，又总是记不住单词的完整拼写，记录一下以后直接来这里看看 ^_^*
### CSS文本溢出
#### 直接用css属性设置(只有-webkit内核才有作用)
``` css
  overflow:hidden;
  text-overflow:ellipsis;
  white-space:nowrap
```
<!-- more -->
* -webkit-line-clamp 用来限制在一个块元素显示的文本的行数,这是一个不规范的属性（unsupported WebKit property），它没有出现在 CSS 规范草案中。
* display: -webkit-box 将对象作为弹性伸缩盒子模型显示 。
* -webkit-box-orient 设置或检索伸缩盒对象的子元素的排列方式 。
* text-overflow: ellipsis 以用来多行文本的情况下，用省略号“…”隐藏超出范围的文本。

限制文字显示两行
``` css 
  display: -webkit-box;
  -webkit-box-orient: vertical;
  -webkit-line-clamp: 2;
  overflow: hidden;
```

#### 利用伪类
``` html 
  <div id="con">
    <span id="txt">文本溢出显示省略号,文本溢出显示省略号</span>
    <span class="t"></span>
  </div>
```
``` css
  <style>
    #txt{
    display: inline-block;
    height: 40px;
    width: 250px;
    line-height: 20px;
    overflow: hidden;
    font-size: 16px;
    }
    .t:after{
    display: inline;
    content: "...";
    font-size: 16px;
      
    }
  </style>
```
#### 针对IE，利用绝对定位遮挡文字
``` html 
  <p id="con2">文本溢出显示省略号,文本溢出显示省略号,文本溢出显示省略号,文本溢出显示省略<span class="t2">...</span>
  </p>
```
``` css
  <style>
    #con2{
      position: relative;
      height: 40px;
      width: 250px;
      line-height: 20px;
      overflow: hidden;
      padding-right: 12px;
    }  
    .t2{
      position: absolute;
      right: 0;
      bottom: 0;
    }
  </style>
```
### CSS文字对齐方式
#### 文字两端对齐
``` html 
  <div>用户名</div>
  <div>密码</div>
  <div>验证码</div>
```
``` css
  <style>
    div {
        margin: 10px 0; 
        width: 100px;
        border: 1px solid red;
        text-align: justify;
        text-align-last:justify
    }
    div:after{
        content: '';
        display: inline-block;
        width: 100%;
    }
  </style>
```