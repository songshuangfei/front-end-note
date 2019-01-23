# 数组及常用操作
书签:
* [创建和初始化数组](#t1)
* [数组遍历](#t2)
* [删除、添加和插入元素](#t3)
* [二维数组](#t4)
* [数组合并与复制](#t5)
* [数组排序](#t6)
* [数组检索](#t7)


js数组和其他语言数组不一样，并不强制每一个元素类型相同，数组长度也并不是始终固定的。js的数组和对象很像。
<span name="t1"></span>
## 创建和初始化数组
创建数组方式有两种：
* `var arr = new Array()`
* `var arr = []`

第一种方法创建时构造函数可以传一个Number类型的参数（必须是整数）为数组长度，同时数组元素并不会有默认值，都是undefined。
```js
var arr = new Array(3);
console.log(arr.length);//3
console.log(arr[0]);//undefined
arr[0] = 10;
console.log(arr[0]);//10
```
es6中数组有了一个新的方法`fill()`可以为数组设置初始化值。
```js
var arr = new Array(3)
arr.fill(0);
console.log(arr);//[0,0,0]
```
构造函数传多个参数时这些参数就会依次转换为数组元素。
```js
var arr = new Array(0,1,2);
console.log(arr.length);//3
console.log(arr[0]);//0
```
使用构造函数创建，传一个Number类型时就不能确定意思是数组长度还是数组的第一个元素。所以用第二种方法创建数组较好。第二种方法创建数组时数组元素和数组长度就都很明确了。
```js
var arr = [];
console.log(arr.length);//0

var arr2 = [1];
console.log(arr2.length,arr2[0])//1  1
```
<span name="t2"></span>
## 数组遍历
数组遍历有两种方式：
* for类循环
* 数组自带方法
### for循环
普通for循环是遍历数组常用的方法。
```js
var arr = [0,1,2];
for (let i = 0; i < arr.length; i++) {
    console.log(arr[i]);   
}
```
### for...in循环
for...in循环其实是用来遍历对象的,for...in循环会遍历出对象里的键值。只要是可枚举属性都会被枚举出来，包括原型链中的对象。下面是for...in枚举对象的例子。
```js
function A() {
    this.x =1;
}
A.prototype.s = 2;
function B() {
    this.y = 3;
}
B.prototype = new A();
var b = new B();
for(let k in b){
    console.log(k)
}
//结果
// y
// x
// s
```
js的数组和对象很像。`typrof []`会返回`"object"`。for...in循环遍历数组时会把数组下标转换成string类型作为键值。同时遍历出数组对象其他属性键值。
```js
Array.prototype.x = 100;
var arr = [0,1,2];
arr.y = 100;
for(let k in arr){
    console.log(arr[k]);
}
//结果
// 0
// 1
// 2
// 100
// 100
```
### for...of循环
es6中有了一种新的for循环for...of循环。for...of循环遍历数据时要求被遍历的数据有迭代器接口。es6中数组是原生具有这个接口的。
```js
var arr = ["a","b","c"];
for (const i of arr) {
    console.log(i);
}
//a b c

for (const i of arr.values()) {
    console.log(i)
}
//a b c

for (const i of arr.keys()) {
    console.log(i)
}
// 0 1 2

for (const i of arr.entries()) {
    console.log(i)
}
// Array(2) [0, "a"]
// Array(2) [1, "b"]
// Array(2) [2, "c"]
```
### 数组常用的一些迭代方法
|  方法名 | 描述 | 
| :- | :- |
| map | 对数组中的每一项运行给定函数,返回每一项return的内容所组成的数组 |
| forEach | 对数组中的每一项运行给定函数，无返回值 |
| every | 对数组中的每一项运行给定函数，如果每一项返回true，则最终返回true |
| filter | 对数组中的每一项运行给定函数，返回该函数返回ture的数组素元素组成的新数组 |
<span name="t3"></span>
## 删除、添加和插入元素
### 数组越界
在超过数组长度的位置设置值时，数组的长度也会增加。添加元素前数组的最后一个元素到新添加元素之间的位置值会是undefined。
```js
var arr = ["a","b","c"];
arr[4] = "d";
console.log(arr.length);//5
console.log(arr[3]);//undefined
```
### 在尾部追加元素
通过设置值的方式在尾部添加元素，数组的length也会曾加。
```js
var arr = [0,1,2];
arr[arr.length] = 3;
console.log(arr);//[0, 1, 2, 3]
console.log(arr.length);//4
```
使用数组的push方法在数组尾部添加元素。Array.prototype.push()接受任意个数的参数。这些参数会按顺序添加在数组尾部，同时数组长度也会增加。
```js
var arr = [0,1,2];
arr.push(3,4,5);
console.log(arr);//[0, 1, 2, 3, 4, 5]
console.log(arr.length);//6
```
### 在首位插入元素
数组有自带的方法`Array.prototype.unshift()`能够方便的在数组首位置插入一个或多个元素，其他元素都先后移动，并且改变数组长度。
```js
var arr = [1,2,3];
arr.unshift(-1,0);//[-1, 0, 1, 2, 3]
console.log(arr,arr.length);//5
```
### 删除数组尾部元素
数组的`Array.pototype.pop()`方法能够删除数组尾部的元素，并改变数组长度。`pop()`方法会返回被删除的元素。该方法与数组的`Array.pototype.push()`方法相对。
```js
var arr = [1,2,3,4];
arr.pop();
console.log(arr);//[1, 2, 3]
console.log(arr.length);//3
```
### 删除数组首位元素
数组的`Array.pototype.shift()`方法能够删除数组首位的元素，并改变数组长度。`shift()`方法会返回被删除的元素。该方法与数组的`Array.pototype.unshift()`方法相对。
```js
var arr = [1,2,3,4];
arr.shift()
console.log(arr);//[2, 3, 4]
console.log(arr.length);//3
```
### 在任意位置插入或删除元素
数组的`Array.pototype.splice()`方法可以用来删除任意位置的任意个数的元素，同时改变数组长度。传两个参数时第一个参数表示删除元素的起始位置，第二个参数表示删除元素的个数。超过2个的参数会会依次在被删除元素的位置插入。
```jsn
var arr = [1,2,3,4];
arr.splice(1,2,0);
console.log(arr);//[1,0 4]
console.log(arr.length);//3
```
`splice()`也可以用来添加元素。当删除的元素个数为0时就是单纯的往数组里插入元素了。
```js
var arr = [1,2,3,4];
arr.splice(1,0,0);
console.log(arr);//[1, 0, 2, 3, 4]
console.log(arr.length);//5
```
<span name="t4"></span>
## 二维数组
js二维数组实际上就是一个普通数组的每一个元素任然是一个数组。这样就构成了一个二维数组。多维数组也是一样继续嵌套数组。
```js
var arr = [new Array(3), new Array(3), new Array(3)];
arr.forEach(v=>{v.fill(0)});//初始化

//将二维数组全部push到一个普通数组
var arr2 = [];
for(let i = 0; i < arr.length; i++) {
    for (let j = 0; j < arr[i].length; j++) {
        arr2.push(arr[i][j]);
    }
}
```
<span name="t5"></span>
## 数组合并与复制
### 数组合并
数组的`Array.pototype.contact()`方法可以合并数组，`contact()`方法参数如果传入的是数组，则会合并这个数组到旧数组里。如果传入的不是数组就会把这个参数追加到数组。追加顺序按参数顺序。最终返这个新数组。
```js
var arr = [1,2,3]
var arr2 = arr.concat({a:1},[0,1,2],"a");
console.log(arr2);//[1, 2, 3, Object, 0, 1, 2, "a"]
```
### 复制数组
前面说到js数组和对象很像，js的数组和对象也一样是引用传递的。赋值或传参时其实都只是对数组的引用进行了拷贝。
```js
var arr = [1,2,3]
var arr2 = arr;
arr2[0] = 100;
console.log(arr);//[100, 2, 3]
```
想要真正复制数组可以遍历数组，然后将每一个元素push到新的数组。es6中有了更简单的方法。
* 使用扩展运算符（...）
* `Array.from()`方法

使用扩展运算符：
```js
var arr = [1,2,3]
var arr2 = [...arr];
arr2[0] = 100;
console.log(arr);//[1, 2, 3]
```
使用`Array.from()`方法
```js
var arr = [1,2,3]
var arr2 = Array.from(arr);
arr2[0] = 100;
console.log(arr);//[1, 2, 3]
```
这里两种方法都是浅拷贝，如果数组元素是个复合对象（数组，对象）。那么拷贝的只是它们的引用。
```js
var arr = [1,2,3]
var arr2 = [...arr];
arr2[0] = 100;
console.log(arr);//[1, 2, 3]
```
```js
var arr = [1,2,3,{x:1}]
var arr2 = Array.from(arr);
arr2[3].x = 100;
console.log(arr[3]);//{x:100}
```
<span name="t6"></span>
## 数组排序
### 数组默认排序
数组有一个自带的排序方法`Array.prototype.sort()`。该方法接受一个函数作为参数，这个函数叫做compare函数（比较函数）。如果`sort()`没有传入参数时该方法默认会把数组元素当作字符串排序。
```js
var a = [1,14,13,8,3,"ab","aa"];
a.sort();
console.log(a);//[1, 13, 14, 3, 8, "aa", "ab"]
```
当给`sort()`方法传入一个“比较函数”后，就相当于给了`sort()`方法一个排序规则。 “比较函数”规则如下：

* “比较函数”接受两个参数，这两个参数就代表数组里的元素。
* “比较函数”必须返回三类值：大于0，小于0或0。
* 返回小于零的数时表示第一个参数小，返回大于0时表示第二个参数小，返回0时表示两个数一样大。
* 最终`sort()`函数会用这个“比较函数”定义好的规则遍历数组元素，两两比较，自动排序。

如果数组元素都是数字那么我们可以直接在“比较函数”里返回两个参数的差值。
```js
var a = [1,14,13,8,3];
function compareFn(a,b) {
    return a-b;
}
a.sort(compareFn);
console.log(a);//[1, 3, 8, 13, 14]
```
“比较函数”返回第二个参数减去第一个参数的值将会是倒序。
```js
var a = [1,14,13,8,3];
function compareFn(a,b) {
    return b-a;
}
a.sort(compareFn);
console.log(a);//[14, 13, 8, 3, 1]
```
和下面是一样的效果。
```js
var a = [1,14,13,8,3];
function compareFn(a,b) {
    if(a<b) return -1;      //第一个参数较小
    else if(a>b) return 1;  //第二个参数较小
    return 0;               //两个参数一样大
}
a.sort(compareFn);
console.log(a);//[1, 3, 8, 13, 14]
```
### 自定义排序
假设这里有一个数组，数组里装的是一些文件的信息，每个文件用一个对象表示。对象里包含文件的名称和大小。我们让这个数组能够按文件大小排序。
```js
var files = [
    {name:"1.txt",size:20},
    {name:"p.txt",size:1024},
    {name:"a.txt",size:500}
];

function sortBySize(a, b) {
    if(a.size < b.size)
        return -1;
    if(a.size > b.size)
        return 1;
    return 0;
};

files.sort(sortBySize);

files.forEach(v=>{
    console.log(v.name);
});
//1.txt
//a.txt
//p.txt
```
### 数组倒序
`Array.prototype.reverse()`能够将数组的元素首尾颠倒。
```js
var a = [0,1,2,3];
a.reverse();
console.log(a);//[3, 2, 1, 0]
```
<span name="t7"></span>
## 数组搜索
数组有两个搜索方法`Array.prototype.indexof()`和`Array.prototype.lastIndexof()`，这两个方法都接受一个参数，即被搜索的元素。`indexof()`返回匹配参数的第一个元素的索引，`lastIndexof()`返回匹配参数的最后一个元素的索引。
```js
var a = [1,2,3,1];
console.log(a.indexOf(1));//0
console.log(a.lastIndexOf(1));//3
```
`Array.prototype.indexof()`和`Array.prototype.lastIndexof()`只能简单检索与参数相等的元素，不能按复杂的条件检索。es6有两个新方法`find()`和`findIndex()`。这两个方法都接受一个回调函数作为参数。只要回调函数返回true则表示匹配到元素，回调函数有3个参数，分别是数组当前遍历元素的值、索引和被遍历的数组，可以使用这两个方法完成一些复杂的数组元素匹配检索。`find()`返回第一个匹配到到元素的值，`findIndex()`返回第一个匹配到的元素的索引。如果没有匹配到`find()`返回undefined，`lastIndexof()`返回-1。
```js
var a = [1,2,3,1];
var r = a.find((v,i,arr)=>{
    if(v%3===0) 
        return true;
    else
        return false;
});
console.log(r);//3 值
var r2 = a.findIndex((v,i,arr)=>{
    if(v%3===0) 
        return true;
    else
        return false;
});
console.log(r2);//2 索引
```
还有一个`Array.prototype.includes()`方法，该方法接受1个或2个参数。当传一个参数时表示在整个数组检索这个参数，当传2个参数时第一个参数时，第一个参数表示检索的起始位置，第二个参数表示被检索的值。当检索到时该方法会返回true，否则返回false。
```js
var a = [1,2,3,1];
console.log(a.includes(2));//true
console.log(a.includes(2,2));//false
```
## 数组去重
数组去重有很多方法，这里使用es6的set结构实现数组去重。
```js
var a = [1,2,2,1,4,6,5,5,5,7];
var s = new Set();
a.forEach((v) => {
    s.add(v)
});
a = Array.of(...s);
console.log(a);//[ 1, 2, 4, 6, 5, 7 ]
```
## 相关链接
* [返回目录](/README.md)

