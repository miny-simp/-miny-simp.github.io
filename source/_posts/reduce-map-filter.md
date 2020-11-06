---
title: 'reduce,map,filter'
date: 2019-07-24 16:08:25
categories:
- web前端 
tags: 
- js
---

## reduce(),map(),filter(),sort(),find(),findIndex(),forEach()理解
高阶函数： reduce()  map()  filter() sort() find() findIndex() forEach()
### reduce()
计算数组元素相加后的总和。
reduce() 方法接收一个函数作为累加器，数组中的每个值（从左到右）开始缩减，最终计算为一个值。
语法：
``` javascript
array.reduce(function(total, currentValue, currentIndex, arr), initialValue)
```
<!-- more -->
total：必需。初始值, 或者计算结束后的返回值。
currentValue：必需。当前元素
currentIndex：可选。当前元素的索引
arr：可选。当前元素所属的数组对象。
initialValue	可选。传递给函数的初始值

比如一个 Array 求和：
``` javascript
var arr = [1, 3, 5, 7, 9];
arr.reduce(function(x, y) {
    return x + y;
});    // 25
```

数组去重：
``` javascript
// 数组去重
let arr = [1,2,3,4,4,1];
let newArr = arr.reduce((pre,cur) => {
  if(!pre.includes(cur)){
    return pre.concat(cur);
  } else {
    return pre;
  }
},[]);
console.log(newArr); // [1, 2, 3, 4]
```

### map()
map() 方法返回一个新数组，数组中的元素为原始数组元素调用函数处理后的值。
语法：
``` javascript
array.map(function(currentValue,index,arr), thisValue)
```
currentValue	必须。当前元素的值
index	可选。当前元素的索引值
arr	可选。当前元素属于的数组对象
thisValue	可选。对象作为该执行回调时使用，传递给函数，用作 "this" 的值。
如果省略了 thisValue，或者传入 null、undefined，那么回调函数的 this 为全局对象。

比如把 Array 的所有数字转换为字符串：
``` javascript
var arr = [1, 2, 3, 4, 5, 6, 7];
arr.map(String);   // ['1', '2', '3', '4', '5', '6', '7']
```
>>本质上是用元素作为函数参数去调用函数，所以无需加上参数。比如字符串转为数字：arr.map(Number); 

### filter()
filter() 方法创建一个新的数组，新数组中的元素是通过检查指定数组中符合条件的所有元素。
语法：
``` javascript
array.filter(function(currentValue,index,arr), thisValue)
```
currentValue	必须。当前元素的值
index	可选。当前元素的索引值
arr	可选。当前元素属于的数组对象
thisValue	可选。对象作为该执行回调时使用，传递给函数，用作 "this" 的值。
如果省略了 thisValue ，"this" 的值为 "undefined"

和map()不同的是，filter()把传入的函数依次作用于每个元素，然后根据返回值是true还是false决定保留还是丢弃该元素。
``` javascript
// 100以内的素数
function getPrimes(arr) {
  return arr.filter(function(element) {
    if(element === 1) return false;
    for (let i = 2; i < element; i++) {
        if (element % i == 0) return false;
    }
    return true;
    });
}
var i, result, array = [];
for (i = 1; i < 100; i++) {
  array.push(i);
}
result = getPrimes(array);
console.log('result:', result); // [2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47, 53, 59, 61, 67, 71, 73, 79, 83, 89, 97]
```
### sort()
排序算法。
【注】sort()方法的默认排序规则
  1. 字符串根据ASCII码进行排序，因此小写字母a的ASCII码在大写字母之后。
  2. Array的sort()方法默认把所有元素先转换为String再排序，因此'10'会排在'2'的前面，因为字符'1'比字符'2'的ASCII码小。
>>通常规定，对于两个元素x和y，如果认为x < y，则返回-1，如果认为x == y，则返回0，如果认为x > y，则返回1，这样，排序算法就不用关心具体的比较过程，而是根据比较结果直接排序。
在给定其属性之一的值的情况下，可以对对象进行排序：
``` javascript
var items = [
  { name: 'Edward', value: 21 },
  { name: 'Sharpe', value: 37 },
  { name: 'And', value: 45 },
  { name: 'The', value: -12 },
  { name: 'Magnetic', value: 13 },
  { name: 'Zeros', value: 37 }
];

// sort by value
items.sort(function (a, b) {
  return a.value - b.value;
});

// sort by name
items.sort(function(a, b) {
  var nameA = a.name.toUpperCase(); // ignore upper and lowercase
  var nameB = b.name.toUpperCase(); // ignore upper and lowercase
  if (nameA < nameB) {
    return -1;
  }
  if (nameA > nameB) {
    return 1;
  }

  // names must be equal
  return 0;
});
```

**Array 数组类型的高阶函数：find() findIndex() forEach()**
### find()
find()方法用于查找符合条件的第一个元素，如果找到了，返回这个元素，否则，返回undefined。
``` javascript
var arr = ['Apple', 'pear', 'orange'];
console.log(arr.find(function (s) {
    return s.toLowerCase() === s;
})); // 'pear', 因为pear全部是小写

console.log(arr.find(function (s) {
    return s.toUpperCase() === s;
})); // undefined, 因为没有全部是大写的元素
```
>【如果需要查找元素的位置或元素是否存在于数组中，请使用 Array.prototype.indexOf() 或 Array.prototype.includes() 。】

通过其中一个属性在数组中查找对象：
``` javascript
const inventory = [
    {name: 'apples', quantity: 2},
    {name: 'bananas', quantity: 0},
    {name: 'cherries', quantity: 5}
];

function isCherries(fruit) { 
    return fruit.name === 'cherries';
}

console.log(inventory.find(isCherries)); 
// { name: 'cherries', quantity: 5 }
```

### findIndex()
findIndex()和find()类似，也是查找符合条件的第一个元素，不同之处在于findIndex()会返回这个元素的索引，如果没有找到，返回-1：

### forEach()
forEach()和map()类似，它也把每个元素依次作用于传入的函数，但不会返回新的数组。forEach()常用于遍历数组，因此，传入的函数不需要返回值：
``` javascript
var array1 = ['a', 'b', 'c'];

array1.forEach(function(element) {
  console.log(element);
});

// expected output: "a"
// expected output: "b"
// expected output: "c"
```

【例】如果在迭代期间修改了数组，则可能会跳过其他元素。
以下示例记录“one”，“two”，“four”。当到达包含值“two”的条目时，整个数组的第一个条目被移开，这导致所有剩余条目向上移动一个位置。因为元素“四”现在位于数组中的较早位置，所以将跳过“三”。forEach()在迭代之前不会复制数组。
``` javascript
var words = ['one', 'two', 'three', 'four'];
words.forEach(function(word) {
  console.log(word);
  if (word === 'two') {
    words.shift();
  }
  console.log('words:', words);
});
 // one
// words: (4) ["one", "two", "three", "four"]
// two
// words: (3) ["two", "three", "four"]
// four
// words: (3) ["two", "three", "four"]
```

[文档学习网站一](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach)
[文档学习网站二](https://www.liaoxuefeng.com/wiki/1022910821149312/1278025786193984)