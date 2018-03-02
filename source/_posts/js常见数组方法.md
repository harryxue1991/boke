---
title: js常见数组方法
date: 2017-05-14 14:04:41
tags: 
---
javaScript的数组方法非常常用，在开发过程中我们经常会用到。为了增加印象，下面我把它列出来：

## 1.join() 
将数组的元素组起一个字符串
```js
var arr = [1,2,3];
arr.join("-");
// 运行结果：
console.log(arr)  //"1-2-3"
```

## 2.push()和pop()
数组末尾，增加或者删除
```js
//----------push
var arr = ["1","2","3"];
var count = arr.push("4","5");
// 运行结果：
console.log(count)  //5
console.log(arr); // ["1", "2", "3", "4", "5"]
//----------pop
var item = arr.pop();
console.log(item); // 5
console.log(arr); // ["1", "2", "3", "4"]
```

## 3.shift()和unshift()
和上面的push() 和 pop() 正好相反，在数组的开头增加或删除
```js
//----------shift()
var arr = ["1","2","3"];
var count = arr.unshift("4","5");
console.log(count); // 5
console.log(arr); //["4", "5", "1", "2", "3"]
//----------unshift()
var item = arr.shift();
console.log(item); // 4
console.log(arr); // ["5", "1", "2", "3"]
```

## 4.sort()
数组排序，若排序二位以上数字，需家参数比较
```js
var arr = [23,12,1,34,116,8,18,37,56,50];
function sequence(a,b){
    return a - b;
}
console.log(arr.sort(sequence));
// 运行结果：        
[1, 8, 12, 18, 23, 34, 37, 50, 56, 116]
```

## 5.reverse()
数组翻转顺序
```js
var arr = [13, 24, 51, 3];
console.log(arr.reverse()); //[3, 51, 24, 13]
console.log(arr); //[3, 51, 24, 13](原数组改变)
```

## 6.concat()