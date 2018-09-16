# const定义对象的可修改性
const是常量的声明方法，但是const把并不是保证了声明变量的值不变，而是不允许变量指向的地址改动。如果声明的变量是数组或对象，变量内部属性则能通过属性访问方式被修改。
## const声明的引用类型变量可被修改
```javascript
const a = {
    x: 1,
    b: 2
};
a.b = 3;         //修改常量内部属性可实现
console.log(a.b) //3
```
```javascript
const a = {
    x: 1,
    b: 2
};
a = {    //TypeError: Assignment to constant variable
    x: 1,
    b: 3
}
```
这里给a变量赋值就是修改了a指向的内存地址，就会报错"TypeError: Assignment to constant variable"。

数组和对象是一样的。
```javascript
const arr = [1,2];
arr[0] = 2;
console.log(arr);//[2,2]
```
## 对象冻结
如果想要对象不被修改的话可以使用Object.freeze()。该方法可以用来冻结对象的属性。属性如果是值类型则不能修改其值。属性如果是数组或对象则不能修改属性的指向的数组或对象，但是能通过属性访问的方式修改对象属性的属性。
```javascript
const obj = Object.freeze({
    x: 0,
    y: {
        p: 0
    }
});

obj.x = 1;          //修改不生效
console.log(obj.x)  //0

obj.y = {p: 1}      //修改对象属性的指向，不生效
console.log(obj.y)  //{p: 0}

obj.y.p = 1;        //修改生效，obj.y下的属性并未冻结
console.log(obj.y); //{p: 1}
```
Object.freeze()只能冻结对象的直接属性不能修改。并不能保证属性中的属性不被修改。但是可以通过遍历对象的方式实现所有属性的冻结。在[对象的拷贝与冻结](/es6/objectCopyFreeze.md)有讲。

## 相关链接
* [对象的拷贝与冻结](/js/objectCopyFreeze.md)
* [返回目录](/README.md)
