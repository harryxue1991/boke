---
title: js常见数组方法
date: 2017-05-14 14:04:41
tags: 
---
javaScript的数组方法非常常用，在开发过程中我们经常会用到。为了增加印象，下面我把它列出来：

## ES5常用数组方法：

> 1.join()

将数组的元素组起一个字符串

```js
var arr = [1,2,3];
arr.join("-");
// 运行结果：
console.log(arr)  //"1-2-3"
```

<!-- more -->

> 2.push()和pop()

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

> 3.shift()和unshift()

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

> 4.sort()

数组排序，若排序二位以上数字，需加参数比较

```js
var arr = [23,12,1,34,116,8,18,37,56,50];
function sequence(a,b){
    return a - b;
}
console.log(arr.sort(sequence));
// 运行结果：
[1, 8, 12, 18, 23, 34, 37, 50, 56, 116]
```

> 5.reverse()

数组翻转顺序

```js
var arr = [13, 24, 51, 3];
console.log(arr.reverse()); //[3, 51, 24, 13]
console.log(arr); //[3, 51, 24, 13](原数组改变)
```

> 6.concat()

数组添加到原数组中，返回值是合并后的，但原数组不变
注：传入的数组会将它的每一项添加到原数组中，但二维数组的话，传入的会是数组

```js
var arr = [1,3,5,7];
var arrCopy = arr.concat(9,[11,13]);
console.log(arrCopy); //[1, 3, 5, 7, 9, 11, 13]
console.log(arr); // [1, 3, 5, 7](原数组未被修改)
```

> 7.slice()

截取数组的某一段，原数组未变，返回值是截取的那一段
若接受一个参数，则返回这个参数到末尾的值

```js
var arr = [1,2,3,4,5,6];
var arrCopy = arr.slice(1);
console.log(arrCopy) //[2, 3, 4, 5, 6]
```

若两个参数，则返回指定的开始到结束所有项   但不包括结束项哦

```js
var arr = [1,2,3,4,5,6];
var arrCopy2 = arr.slice(1,4);
console.log(arrCopy2) //[2, 3, 4]
//若值为负数，则从末尾开始计算
var arrCopy3 = arr.slice(-4,-1)
console.log(arrCopy3)  //[3,4,5]
```

> 8.splice()

它很强大，它有很多种用法，可以实现删除、插入和替换，会改变原数组

```js
//删除：指定两个参数：第一项位置和要删除的项数，返回删除的项
var arr = [1,2,3,4,5]
var arrRemoved = arr.splice(0,2);
console.log(arr) //[3,4,5]
console.log(arrRemoved) //[1,2]

//插入：在指定位置插入任意数量的项，需要3格参数：起始位置、0（要删除的项数）和要插入的项
var arrRemoved2 = arr.splice(2,0,8,9);
console.log(arr); // [3, 4, 8, 9, 5]
console.log(arrRemoved2); // []

//替换：和插入一样，只需要把第二个参数（需要删除的项数）改一下，改成需要删除的数量，
//然后后面参数再往后添加，相当于替换了
var arrRemoved3 = arr.splice(1,1,6,7);
console.log(arr); // [3, 6, 7, 8, 9, 5]
console.log(arrRemoved3); //[4]
```

> 9.indexOf()和lastIndexOf()

查找索引，若加一个参数，表示从数组开头，第二个参数表示查找起点位置的索引
indexOf是从左往右差，lastIndexOf是右往左差，若无法查到，则返回-1

```js
//----------indexOf()
var arr = [1,3,5,7,7,5,3,1];
console.log(arr.indexOf(5)); //2
console.log(arr.indexOf(5,3)); //5
//----------lastIndexOf()
console.log(arr.lastIndexOf(5)); //5
console.log(arr.lastIndexOf(5,4)); //2
console.log(arr.indexOf("5")); //-1
```

## ES6常用数组方法：

> 1.forEach()

对数组遍历循环，参数为function，function接收3个参数
第一个参数为value值，第二个参数为索引值，第三个参数为数组本身

```js
var arr = [1, 2, 3, 4, 5];
    arr.forEach(function(x, index, a){
    console.log(x + '|' + index + '|' + (a === arr));
});
// 输出为：
// 1|0|true
// 2|1|true
// 3|2|true
// 4|3|true
// 5|4|true
```

> 2.map()

意思是给数组里每一项，运行给定的函数，再输出函数处理后的数组

```js
var arr = [1, 2, 3, 4, 5];
var arr2 = arr.map(function(item){
    return item+1;
});
console.log(arr2); //[2, 3, 4, 5, 6]
```

> 3.filter()

过滤功能，返回过滤后的值

```js
var arr = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
var arr2 = arr.filter(function(x, index) {
    return index % 3 === 0 || x >= 8;
});
console.log(arr2); //[1, 4, 7, 8, 9, 10]
```

> 4.every()

判断数组中每一项是否满足条件，只有所有项都满足，参会返回true

```js
var arr = [1, 2, 3, 4, 5];
var arr2 = arr.every(function(x) {
    return x < 10;
});
console.log(arr2); //true
var arr3 = arr.every(function(x) {
    return x < 3;
});
console.log(arr3); // false
```

> 5.some()

判断数组中是否有满足条件的项，只要有一项满足，就返回true

```js
var arr = [1, 2, 3, 4, 5];
var arr2 = arr.some(function(x) {
    return x < 3;
});
console.log(arr2); //true
var arr3 = arr.some(function(x) {
    return x < 1;
});
console.log(arr3); // false
```

> 6.reduce()和 reduceRight()

接收一个函数作为累加器，数组中的每个值（reduce从左到右，reduceRight从右到左）开始缩减
一共两个参数，第一个函数，第二个基础初始值
传给函数的4个参数：前一个值、当前值、项的索引和数组对象。

```js
var values = [1,2,3,4,5];
var sum = values.reduceRight(function(prev, cur, index, array){
    return prev + cur;
},10);
console.log(sum); //25
```

> 7.find()

参数是回调函数，回调函数可以接受3个参数，默认返回value值
参数：value值，索引index，数组arr

```js
let arr=[1,2,234,'sdf',-2];
arr.find(function(x){
    return x<=2;
})
//结果：1， 使用return将返回第一个符合条件的项
arr.find(function(x,i,arr){
    if(x<2)
        {console.log(x,i,arr)}
})
//结果：1 0 [1, 2, 234, "sdf", -2]，-2 4 [1, 2, 234, "sdf", -2]
```

> 8.findIndex()

和上面的find()函数一样，只是默认返回的是索引值

```js
let arr=[1,2,234,'sdf',-2];
arr.findIndex(function(x){
    return x<=2;
})
//结果：0，返回第一个符合条件的x值的索引
arr.findIndex(function(x,i,arr){
    if(x<2){console.log(x,i,arr)}
})
//结果：1 0 [1, 2, 234, "sdf", -2]，-2 4 [1, 2, 234, "sdf", -2]
```

> 9.includes()

查询数组中是否有这个值，返回布尔值，牛逼了我的哥
第二个参数是查询的起始位置

```js
let arr=[1,2,234,'sdf',-2];
arr.includes(2);// 结果true，返回布尔值
arr.includes(20);// 结果：false，返回布尔值
arr.includes(2,3)//结果：false，返回布尔值
```

> 9.keys()

keys，对数组索引值遍历

```js
let arr=[1,2,234,'sdf',-2];
for(let a of arr.keys()){
    console.log(a)
}
//结果：0,1,2,3,4  遍历了数组arr的索引
```

这里出现了一个es6的for...of的语法，它可以遍历数组，字符串，arguments,集合等，of之前还可以使用解构语法。

> 10.values()

和上面的keys对应，是对数组的每一项遍历

```js
let arr=[1,2,234,'sdf',-2];
for(let a of arr.values()){
    console.log(a)
}
//结果：1,2,234,sdf,-2 遍历了数组arr的值
```

> 11.entries()

对数组键值对的遍历，之前有说过，of前面可以使用解构语法。

```js
let arr=['w','b'];
for(let a of arr.entries()){
    console.log(a)
}
//结果：[0,w],[1,b]
for(let [i,v] of arr.entries()){
    console.log(i,v)
}
//结果：0 w,1 b
```

> 12.fill()

用于将一个固定值替换数组的元素，当第三个参数大于数组长度时候，以最后一位为结束位置。
三个参数： 1.必填，2.开始位置（索引），3.结束位置（结束位置不填充）

```js
let arr = ['w','b'];
arr.fill('i')
//结果：['i','i'],改变原数组
arr.fill('o',1)
//结果：['i','o']改变原数组,第二个参数表示填充起始位置
new Array(3).fill('k').fill('r',1,2)
//结果：['k','r','k']，第三个数组表示填充的结束位置
```

> 13.Array.of()

返回永远是一个数组，参数不分类型，只分数量，数量为0返回空数组

```js
Array.of('w','i','r')
//["w", "i", "r"]返回数组
Array.of(['w','o'])
//[['w','o']]返回嵌套数组
Array.of(undefined)
//[undefined]依然返回数组
Array.of()
//[]返回一个空数组
```

> 14.copyWithin()

copyWithin方法接收三个参数，被替换数据的开始处、替换块的开始处、替换块的结束处(不包括);
copyWithin(s,m,n)

```js
["w", "i", "r"].copyWithin(0)
//此时数组不变
["w", "i", "r"].copyWithin(1)
//["w", "w", "i"],数组从位置1开始被原数组覆盖，只有1之前的项0保持不变
["w", "i", "r","b"].copyWithin(1,2)
//["w", "r", "b", "b"],索引2到最后的r,b两项分别替换到原数组1开始的各项，当数量不够，变终止
["w", "i", "r",'b'].copyWithin(1,2,3)
//["w", "r", "r", "b"]，强第1项的i替换为第2项的r
```

> 15.Array.from()

Array.from可以把带有lenght属性类似数组的对象转换为数组，也可以把字符串等可以遍历的对象转换为数组，它接收2个参数，转换对象与回调函数

```js
Array.from({'0':'w','1':'b',length:2})
//["w", "b"],返回数组的长度取决于对象中的length，故此项必须有！
Array.from({'0':'w','1':'b',length:4})
//["w", "b", undefined, undefined],数组后2项没有属性去赋值，故undefined
Array.from({'0':'w','1':'b',length:1})
//["w"],length小于key的数目，按序添加数组

//////////////////////////////
let divs=document.getElementsByTagName('div');
Array.from(divs)
//返回div元素数组
Array.from('wbiokr')
//["w", "b", "i", "o", "k", "r"]
Array.from([1,2,3],function(x){
        return x+1
    }
)
//[2, 3, 4],第二个参数为回调函数
```