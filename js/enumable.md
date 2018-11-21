# 对象和数组的枚举
## 对象枚举
我们可以使用`for...in`循环来遍历对象中的所有属性名。该枚举会列举出对象中的所有属性（包括原型中的属性）。但是js中基本包装类型的原型属性是不可枚举的，如Object, Array, Number等。
```js
var obj ={
    x:1,
    y:2,
    z:3
}
for(var key in obj){
    console.log(`${key}:${obj[key]}`)
}
// x:1
// y:2
// z:3
```
上面循环里的变量key就是obj的属性名，key的类型是String。
```js
Object.prototype.sum = function(){
    return this.x+this.y+this.z;
}
var obj ={
    x:1,
    y:2,
    z:3
}
for(var key in obj){
    console.log(`${key}:${obj[key]}`)
}
// x:1
// y:2
// z:3
// sum:function(){return this.x+this.y+this.z;;}
```
我们往Object的原型里添加了的属性依然被`for...in`循环枚举出来了。我们可以使用`hasOwnProperty()`方法判断枚举出的属性是否属于当前对象。如果是该方法会返回true。
```js
Object.prototype.sum = function(){
    return this.x+this.y+this.z;
}
var obj ={
    x:1,
    y:2,
    z:3
}
for(var key in obj){
    if(obj.hasOwnProperty(key)){//判断
        console.log(`${key}:${obj[key]}`)
    }
}
// x:1
// y:2
// z:3
```
通过`hasOwnProperty()`方法我们就可以过滤掉原型链中的属性。
## 数组的枚举（迭代）
数组也可以使用`for..in`循环来枚举。不过并不推荐这样做。js的数组和对象很相似，在js中Object是Array的父类。
```js
var arr = [];
console.log(arr.__proto__.__proto__==Object.prototype);//true
console.log(Array.prototype.__proto__==Object.prototype)//true
```
也就是说js的数组并不是是一个纯粹的数组。它也可以像对象一样拥有自己的属性，也能从原型链中继承属性和方法（js的其他基本类型和Array一样都是Objec的子类）。前面说到在`for..in`循环中js中基本包装类型的原型属性是不可枚举的”。我们并不用担心`for..in`遍历数组时得到`Array`里的属性或方法。但是当我们自己在`Array.prototype`里添加属性或在数组实例里添加属性时，`for...in`循环就会枚举出我们添加的这些属性。
```js
Array.prototype.x = 100;
var arr = [1,2,3];
arr.y = 200;
for(var i in arr){
    console.log(`${i}:${arr[i]}`);
}
//0:1
//1:2
//2:3
//y:200
//x:100
```
上面的例子看到`for...in`循环将数组的索引作为对象属性枚举了出来，同时也枚举出了在`Array.Prototype`和数组实例中添加的属性。然而数组的使用主要是使用数组索引对应的值，枚举出其他属性就违背了数组的使用理念。所以并不推荐使用`for...in`循环遍历数组。正确的应该使用循环自增量来做索引。
```js
Array.prototype.x = 100;
var arr = [1,2,3];
arr.y = 200;
for(var i = 0;i < arr.length; i++){
    console.log(`${i}:${arr[i]}`);
}
//0:1
//1:2
//2:3
```
es6后有了更加方便遍历数组的`for..of`循环，`for..of`不能用来枚举对象。
> for...of语句在可迭代对象（包括 Array，Map，Set，String，TypedArray，arguments 对象等等）上创建一个迭代循环，调用自定义迭代钩子，并为每个不同属性的值执行语句。
```js
Array.prototype.x = 100;
var arr = [1,2,3];
arr.y = 200;
for(var v of arr){
    console.log(v)
}
//1
//2
//3
```
`for...of`循环就能很方便的遍历数组，而且不会枚举出数组里的其他属性。
## 总结
* 在枚举对象属性时我们可以使用`for...in`循环，并可用`hasOwnProperty()`方法判断枚举出的属性是否直接属于当前对象。
* 数组遍历并不推荐使用`for...in`循环（可能会枚举出非数组元素的其他属性）。
* 数组遍历推荐使用普通for循环（自增变量作数组索引）或`for...of`循环。
## 数组的一些迭代方法
|  方法名 | 描述 | 
| :- | :- |
| map | 对数组中的每一项运行给定函数,返回每一项return的内容所组成的数组 |
| forEach | 对数组中的每一项运行给定函数，无返回值 |
| every | 对数组中的每一项运行给定函数，如果每一项返回true，则最终返回true |
| filter | 对数组中的每一项运行给定函数，返回该函数返回ture的数组素元素组成的新数组 |
## 相关链接
* [原型链和构造函数](/js/prototype.md)
* [返回目录](/README.md)