# 数组及常用操作
js数组和其他语言数组不一样，并不强制每一个元素类型相同，数组长度也并不是始终固定的。js的数组和对象很像。
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
数组的`pop()`方法能够删除数组尾部的元素，并改变数组长度。`pop()`方法会返回被删除的元素。该方法与数组的`push()`方法相对。
```js
var arr = [1,2,3,4];
arr.pop();
console.log(arr);//[1, 2, 3]
console.log(arr.length);//3
```
### 删除数组首位元素
数组的`shift()`方法能够删除数组首位的元素，并改变数组长度。`shift()`方法会返回被删除的元素。该方法与数组的`unshift()`方法相对。
```js
var arr = [1,2,3,4];
arr.shift()
console.log(arr);//[2, 3, 4]
console.log(arr.length);//3
```
### 在任意位置插入或删除元素
数组的`splice()`方法可以用来删除任意位置的任意个数的元素，同时改变数组长度。传两个参数时第一个参数表示删除元素的起始位置，第二个参数表示删除元素的个数。超过2个的参数会会依次在被删除元素的位置插入。
```js
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