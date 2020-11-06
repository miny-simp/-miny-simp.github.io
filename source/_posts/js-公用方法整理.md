---
title: js-公用方法整理
date: 2020-06-04 22:28:22
categories:
- web前端 
tags: 
- js
---
### 将json对象的null转为''
``` javascript
function parseNullForObj(t){
  for(var p in t){
    if(typeof t[p] == 'undefined' || t[p] == null || t[p] == 'null'){
      t[p] = '';
    }
  }
  return t;
}
```
<!-- more -->
### 判断变量是否为空
``` javascript
function isNull(inVariable){
  if(typeof inVariable=='undefined'||inVariable == "" || inVariable == "null" || inVariable == ""||inVariable==null){
    return true;
  }else{
    return false;
  }
}
```

### 检测参数得数据类型
``` javascript
type = para => {
  return Object.prototype.toString.call(para).slice(8,-1)  
}
// --- 扩展 ---
// 是否字符串
export const isString = (o) => {
    return Object.prototype.toString.call(o).slice(8, -1) === 'String'
}
// 是否数字
export const isNumber = (o) => {
    return Object.prototype.toString.call(o).slice(8, -1) === 'Number'
}
// 是否boolean
export const isBoolean = (o) => {
    return Object.prototype.toString.call(o).slice(8, -1) === 'Boolean'
}
// 是否函数
export const isFunction = (o) => {
    return Object.prototype.toString.call(o).slice(8, -1) === 'Function'
}
// 是否为null
export const isNull = (o) => {
    return Object.prototype.toString.call(o).slice(8, -1) === 'Null'
}
// 是否undefined
export const isUndefined = (o) => {
    return Object.prototype.toString.call(o).slice(8, -1) === 'Undefined'
}
// 是否对象
export const isObj = (o) => {
    return Object.prototype.toString.call(o).slice(8, -1) === 'Object'
}
// 是否数组
export const isArray = (o) => {
    return Object.prototype.toString.call(o).slice(8, -1) === 'Array'
}
```
### 将特殊字符通过正则直接删除
``` javascript
function htmlEncode(str) {
  //匹配< / > " & `
  return str.replace(/[<>"&\/`']/g, '');
}
```
### 用js限制字数，超出部分以省略号...显示，过滤空格'&nbsp'
``` javascript
function showHiddenText(content,num) {
  var newstr = content.replace(/&nbsp;/ig, "");
  newstr = newstr.replace(/<br \/>/g, "");
  if(content != null && content != "" && content != "nul" && content != undefined) {
    if(content.length > num) {
      newstr = content.substring(0, num) + "...";
    }
  }
  return newstr;
}
```
### 时间格式化（年月日时分秒）
``` javascript
let date = Date.parse(new Date()) //  获取当前时间戳(毫秒)
dateTime = time => {
  let newDate = new Date(time);
  let { y, M, d, h, m, s } = { 
    y: newDate.getFullYear(), 
    M: newDate.getMonth() + 1, 
    d: newDate.getDate(), 
    h: newDate.getHours(), 
    m: newDate.getMinutes(), 
    s: newDate.getSeconds() };
  return `${y}-${M}-${d}  ${h}:${m}:${s}`;
}
console.log(dateTime(date));

// 格式化日期时间
function formatDate(val){
  if(val) {
    var date = new Date(val);
    var y = date.getFullYear();
    var MM = date.getMonth() + 1;
    MM = MM < 10 ? ('0' + MM) : MM;
    var d = date.getDate();
    d = d < 10 ? ('0' + d) : d;
    var h = date.getHours();
    h = h < 10 ? ('0' + h) : h;
    var m = date.getMinutes();
    m = m < 10 ? ('0' + m) : m;
    var s = date.getSeconds();
    s = s < 10 ? ('0' + s) : s;
    return y + '-' + MM + '-' + d + ' ' + h + ':' + m + ':' + s;
  } else {
    return '-'
  }
}
// 秒转时分
exports.formatSecond = (second) => {
  if(second){
    var HOUR_SECOND = 3600;
    var MINUTE_SECOND = 60;
    var hour = Math.floor(second / HOUR_SECOND);
    var minute = Math.round(((second % HOUR_SECOND) / MINUTE_SECOND));
    if(hour > 0){
        if(minute > 0){
            return hour+"时"+minute+"分";
        }else{
            return hour+"时";
        }
    }else{
        return minute+"分";
    }
  } else {
    return 0;
  }
}
```
### 数字超过9显示省略号
``` javascript
num_filter = val =>{
  val = val?val-0:0;
  if (val > 9 ) {
    return "…"
  }else{
    return val;
  }
}
```
### 删除数组中的元素
``` javascript
// 在数组中找指定的元素,返回下标
arrFinNum = function (arr,num) {
  let index = -1;
  for (let i = 0; i < arr.length; i++) {
    if (num == arr[i]) {
      index = i;
      break;
    }
  }
  return index;
}
let arr = [1,2,3,4,5,6]
console.log(arrFinNum(arr,4));  // 3

// 删除指定元素
delArrNum = (arr,val) => {
  let index = arrFinNum(arr, val) //调用了前面自行添加的arrFinNum方法
  if (index != -1) {
    return arr.splice(index, 1);
  }
}
```
### 判断一个元素是否在数组中
``` javascript
export const contains = (arr, val) => {
    return arr.indexOf(val) != -1 ? true : false;
}
// 或者
export const contains = (arr, val) => {
    return $.inArray(val, arr) != -1 ? true : false;
}
```
### 数组去重
``` javascript
arrDemp = arr => {
  let newArr = [];
  let m = {};
  for (let i = 0; i < arr.length; i++) {
    let n = arr[i];
    if (m[n]) {

    } else {
      newArr.push(arr[i]);
      m[n] = true;
    }
  }
  return newArr;
}

let arr = [1,2,3,5,4,5,4,3,6]
console.log(arrDemp(arr));  //  [ 1, 2, 3, 5, 4, 6 ]

// 也可以使用ES6中的new Set   
let arr = [1,2,3,5,4,5,4,3,6]
let arrDemp = new Set(arr)  //arrDemp是一个对象
let newArr = [...arrDemp]   //把arrDemp转化成数组
console.log(newArr);
```
### ztree树转id pid转化为children格式
``` javascript
var jsonDataTree = transData(res.data, 'id', 'pid', 'children');  
console.log(jsonDataTree);
/** 
 * json格式转树状结构 
 * @param   {json}      json数据 
 * @param   {String}    id的字符串 
 * @param   {String}    父id的字符串 
 * @param   {String}    children的字符串 
 * @return  {Array}     数组 
 */  
function transData(a, idStr, pidStr, childrenStr){
  var r = [], hash = {}, id = idStr, pid = pidStr, children = childrenStr, i = 0, j = 0, len = a.length;  
  for(; i < len; i++){  
    hash[a[i][id]] = a[i];  
  }  
  
  for(; j < len; j++){  
    var aVal = a[j], hashVP = hash[aVal[pid]]; 
    if(hashVP){  
      !hashVP[children] && (hashVP[children] = []);  
      hashVP[children].push(aVal);  
    }else{  
      r.push(aVal);  
    }  
  }  
  return r;  
}  
```
### html 页面传参数解析方法 
``` javascript
function getPageParams() {
  var pageurl = window.location.href;
  var param = {};
  if (pageurl.indexOf("?") != -1) {
    var paramstr = pageurl.split("?")[1];
    var pArr = paramstr.split("&");
    // var pArr = paramstr.split("#"); // minzz ok-admin的tab切换会生成#divid，将&替换成#
    var tArr = null;
    for (var i = 0; i < pArr.length; i++) {
      tArr = pArr[i].split("=");
      if (tArr.length == 2) {
        param[tArr[0]] = tArr[1];
      } else {
        param[tArr[0]] = "";
      }
    }
  }
  return param;
}
```
### 复制简单对象/数组
``` javascript
//复制简单对象
function cloneObj(obj) {
  var newObj = {}
  for (var prop in obj) {
    newObj[prop] = obj[prop];
  }
  return newObj
}
//复制简单数组
function cloneArray(array) {
  var newArray = [];
  for (var i = 0; i < array.length; i++) {
    newArray[i] = array[i];
  }
  return newArray;
}
```
### 获取某月份的最后一天
``` javascript
// 获取某月份的最后一天
function getLastDay(data) {
  var dateList = data.split('-');
  var new_year = dateList[0];    //取当前的年份         
  var new_month = dateList[0]++;//取下一个月的第一天，方便计算（最后一天不固定）         
  if(month>12) {        
   new_month -=12; //月份减         
   new_year++; //年份增         
  }        
  var new_date = new Date(new_year,new_month,1); //取当年当月中的第一天
  var lastDay = (new Date(new_date.getTime()-1000*60*60*24)).getDate();
  return data + '-' + lastDay;//获取当月最后一天日期         
} 
```
### 字符串截取
``` javascript
// js截取两个字符串之间的内容
var str = "aaabbbcccdddeeefff";
str = str.match(/aaa(\S*)fff/)[1];
console.log(str);//结果bbbcccdddeee

// js截取某个字符串前面的内容
var str = "aaabbbcccdddeeefff";
str = str.match(/(\S*)fff/)[1];
console.log(str);//结果aaabbbcccddd

// js截取某个字符串后面的内容
var str = "aaabbbcccdddeeefff";
str = str.match(/aaa(\S*)/)[1];
console.log(str);//结果bbbcccdddeeefff

// 截取字符串的后几位数
var str="abcdefghhhh";//截取后4位
str.substring(str.length-4)
```
### 未完待续
``` javascript

```