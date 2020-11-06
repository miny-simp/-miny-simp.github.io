---
title: if else 结构的优化
date: 2019-06-10 15:42:40
categories:
- web前端 
tags: 
- js
---
### if else 结构的优化(数据结构对代码的简化)
例如：
``` javascript
function returnWeekday(){
    let string = "今天是星期";
    let date = new Date().getDay();
    if (date === 0) {
        string += "日";
    } else if (date === 1) {
        string += "一";
    } else if (date === 2) {
        string += "二";
    } else if (date === 3) {
        string += "三";
    } else if (date === 4) {
        string += "四";
    } else if (date === 5) {
        string += "五";
    } else if (date === 6) {
        string += "六";
    }
    return string
}
console.log(returnWeekday())
```
if else 的判断太多
<!-- more -->
### 使用switch来优化一下：
``` javascript
function returnWeekday(){
    let string = "今天是星期";
    let date = new Date().getDay();
    switch (date) {
        case 0 :
            string += "日";
            break;
        case 1 :
            string += "一";
            break;
        case 2 :
            string += "二";
            break;
        case 3 :
            string += "三";
            break;
        case 4 :
            string += "四";
            break;
        case 5 :
            string += "五";
            break;
        case 6 :
            string += "六";
            break;
    }
    return string
}
console.log(returnWeekday())
```

### 使用数组来优化：
``` javascript
function returnWeekday(){
    let string = "今天是星期";
    let date = new Date().getDay();
    // 使用数组
    let dateArr = ['天','一','二','三','四','五','六'];
    return string + dateArr[date]
}
console.log(returnWeekday())
```
### 倘若我们的每个 case 是不规律的字符串呢？那我们使用对象，每个 case 为一个 key
``` javascript
function returnWeekday(){
    let string = "今天是星期";
    let date = new Date().getDay();
    // 使用对象
    dateObj = { 
        0: '天', 
        1: "一", 
        2: "二", 
        3: "三", 
        4: "四", 
        5: "五", 
        6: "六", 
    };
    return string + dateObj[date]
}
console.log(returnWeekday())
```
### 使用 charAt 字符方法
``` javascript
// charAt 定位方法
function returnWeekday(){
    return "今天是星期" + "日一二三四五六".charAt(new Date().getDay());
}
console.log(returnWeekday())
```
### 若要求returnWeekday() 方法不仅返回今天是周几，还需要区分工作日和休息日，调用相关方法。
``` javascript
function returnWeekday(){
    let string = "今天是星期";
    let date = new Date().getDay();
    // 使用对象
    dateObj = { 
        0: ['天','休'], 
        1: ["一",'工'], 
        2: ["二",'工'], 
        3: ["三",'工'], 
        4: ["四",'工'], 
        5: ["五",'工'], 
        6: ["六",'休'], 
    }
    // 类型，这里也可以对应相关方法
    dayType = {
        '休': function(){
            // some code
            console.log('为休息日')
        },
        '工': function(){
            // some code
            console.log('为工作日')
        }
    }
    let returnData = {
        'string' : string + dateObj[date][0],
        'method' : dayType[dateObj[date][1]]
    }
    return returnData
};
console.log(returnWeekday().method.call(this))
```
### 三元运算
``` javascript
//滚动监听，头部固定
handleScroll: function () {
    let offsetTop = this.$refs.pride_tab_fixed.getBoundingClientRect().top;
    if( offsetTop < 0 ){
        this.titleFixed = true
    } else {
        this.titleFixed = false
    }
    
    // 改成三元
    (offsetTop < 0) ? this.titleFixed = true : this.titleFixed = false;
    
    // 我们发现条件块里面的赋值情况是布尔值，所以可以更简单
    this.titleFixed = offsetTop < 0;
}
```
### 逻辑与运算符
``` javascript
if( falg ){
    someMethod()
}
```
可以修改为：
``` javascript
falg && someMethod();
```
### 使用 includes 处理多重条件
``` javascript
if( code === '202' || code === '203' || code === '204' ){
    someMethod()
}
```
可以修改为：
``` javascript
if( ['202','203','204'].includes(code) ){
    someMethod()
}
```
--- 
原文是来自掘金的一篇文章，刚有了自己的网站，记录下来学习一下。
[原文地址](https://juejin.im/post/5cf8be116fb9a07edc0b4710?utm_source=gold_browser_extension)