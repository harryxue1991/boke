---
title: Javascript类型判断
date: 2016-11-09 13:13:46
tags:
---

## typeof

```js
typeof 3 // "number"
typeof "abc" // "string"
typeof {} // "object"
typeof true // "boolean"
typeof undefined // "undefined"
typeof function(){} // "function"
```

以上判断执行都正常，但是

```js
typeof [] // 'Object'
typeof null // 'Object'
```

这两个结果显然不对，同样，使用typeof判断Date，RegExp等等实例时，返回结果都是Object。所以，它只能用来做基础类型的判断。

<!-- more -->

## instanceof

```js
[1, 2, 3] instanceof Array // true
/abc/ instanceof RegExp // true
({}) instanceof Object // true
(function(){}) instanceof Function // true
```

以上都正常，但是

```js
3 instanceof Number // false
true instanceof Boolean // false
'abc' instanceof String // false
```

判断基本类型时，instanceof就显得非常无力，除非你这么做：

```js
(3).constructor === Number // true
true.constructor === Boolean // true
'abc'.constructor === String // true
null.constructor === '' //Type error

new Number(3) instanceof Number // true
new Boolean(true) instanceof Boolean // true
new String('abc') instanceof String // true
```
但是问题是，我们根本无法知道基本类型的封装类是什么，所以，instanceof也无法完美地满足我们的需求。

## Duck-Typing

因为我们无法完美得检测出对象类型，于是人们就想出一个办法，只要一个东西满足鸭子的特征，那么它就是一只鸭子。从而我们有了很多的类型检测的函数，这里就不一一列举了。