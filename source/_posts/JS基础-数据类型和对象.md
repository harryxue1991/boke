---
title: JS基础-数据类型和对象
date: 2017-04-29 13:19:59
tags:
---

## 数据类型
> 基本数据类型可以直接以值的形式存在内存栈中 [1-6]
> 引用数据类型是保存在堆内存中的对象 [7]

1. undefined
2. null
3. number
4. boolean
5. string
6. symbol (es6)
7. object

<!-- more -->

## Typeof

type | result
---- | ---- 
undefined | "undefined"
null | "object"
string | "string"
number | "number"
boolean | "boolean"
symbol | "symbol"
宿主对象 | Implementation-dependent
函数对象 | "function"
任何其他对象 | "object"


## 对象
在计算机学科中，对象是指内存中的可以被标识符引用的一块区域。

属性类型，用来描述对象的某一个属性：

> 数据属性(Map)

特性 | 数据类型 | 描述 | 默认值
----|----|----|----
[[value]] | Any Javascript Types | 包含这个属性的数据值 | undefined
[[Writable]] | Boolean | 如果该值为false，则该属性的[[Value]]特性不能被改变 | true
[[Enumerable]] | Boolean | 如果该值为true， 则该属性可以用for in循环来枚举 | true
[[Configurable]] | Boolean | 如果该值为false，则该属性不能被删除。 除了[[Value]]和[[Writable]]以外的特性都不能被改变 | true

> 访问器属性

特性 | 数据类型 | 描述 | 默认值
----|----|----|----
[[Get]] | 函数对象或者 undefined | 该函数使用一个空的参数列表 能够在有权访问的情况下读取属性值 | undefined
[[Set]] | 函数对象或者 undefined | 该函数有一个参数，用来写入属性值 | true
[[Enumerable]] | Boolean | 如果该值为true， 则该属性可以用for in循环来枚举 | true
[[Configurable]] | Boolean | 如果该值为false，则该属性不能被删除。 除了[[Value]]和[[Writable]]以外的特性都不能被改变 | true

> 对象的创建过程

1. 创建一个build-in object对象obj并初始化
2. 如果Fn.prototype是Object类型，则将obj的内部[[Prototype]]设置为Fn.prototype，否则obj的[[Prototype]]将为其初始化值(即Object.prototype)
3. 将obj作为this，使用args参数调用Fn的内部[[Call]]方法
    1. 内部[[Call]]方法创建当前执行上下文
    2. 调用Fn的函数体
    3. 销毁当前的执行上下文
    4. 返回F函数体的返回值，如果F的函数体没有返回值则返回undefined
4. 如果[[Call]]的返回值是Object类型，则返回这个值，否则返回obj

注意步骤2中， prototype指对象显示的prototype属性，而[[Prototype]]则代表对象内部Prototype属性(隐式的)。
构成对象Prototype链的是内部隐式的[[Prototype]]，而并非对象显示的prototype属性。显示的prototype只有在函数对象上才有意义，从上面的创建过程可以看到，函数的prototype被赋给派生对象隐式[[Prototype]]属性，这样根据Prototype规则，派生对象和函数的prototype对象之间才存在属性、方法的继承/共享关系。

![2017-03-26](http://odl96infd.bkt.clouddn.com/2017-03-26-屏幕快照 2017-03-26 上午11.20.45.png)

## Tips
1. JS引擎会自动将变量声明和变量赋值拆分，变量声明会被提升
2. 函数声明的优先级高于变量声明，但是如果出现赋值语句，则会优先取赋值的值。
3. 活动对象是在进入函数上下文时刻被创建的，它通过函数的arguments属性初始化。arguments属性的值是Arguments对象。

    Arguments对象是活动对象的一个属性，它包括如下属性：
    1. callee ： 指向当前函数的引用
    2. length ： 真正传递的参数个数
    3. properties-indexes (字符串类型的整数) 属性的值就是函数的参数值(按参数列表从左到右排列)。properties-indexes内部元素的个数等于arguments.length. properties-indexes 的值和实际传递进来的参数之间是共享的。

## Function Invocation

一个函数的call方法被调用，大概发生了这么几件事：（简化版）
1. 用第二个至最后一个参数，放到一个数组argsList
2. 把第一个参数当做thisValue
3. 调用函数，将this的值设为thisValue，把argsList当做arguments