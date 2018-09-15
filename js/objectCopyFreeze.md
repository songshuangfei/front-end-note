# 对象的拷贝与冻结
js对象是通过引用传递，它们永远不会被复制。在赋值或传参时number、string和boolean都会产生一个副本而对象和数组都是传递的引用。
## 对象的引用传递
```javascript
var obj = {
    x: 1
};

var obj2 = obj;   //obj和obj2都指向了同一个对象
obj2.x = 0;
console.log(obj); //{x: 0}

obj = {           //修改obj指向(指向新对象)
    y: 1
}
console.log(obj)  //{y: 1}
console.log(obj2) //{x: 0},并未因obj指向改变而改变。
```
`var obj2 = obj`这一句让obj2指向了obj所指向的对象。注意，obj2是直接指向了内存中的`{x: 1}`这个对象，而不是`obj2->obj->{x: 1}`这样的指向方式。所以后面`obj = {y: 1}`让obj指向了新的对象但obj2任然指向原来的对象`{x: 1}`。在`obj = {y: 1}`这一句之前obj和obj2都是指向同一个对象的，所以这两个变量任何一个修改指向对象`{x: 1}`中的属性后另外一变量访问对象属性时获得的都是修改后的值。

数组和对象是一样的也时引用传递。
```javascript
const arr = [1,2];
var arr1 = arr;
arr1[0] = 2;
console.log(arr);           //[2,2]，arr改变
console.log(typeof(arr1));  //object
```
上面可以看到数组类型和对象一样时引用传递，我们查看数组变量的类型其实也是object。

## 对象拷贝
很多时候我们需要对某个对象属性修改后再使用整个对象但又不想修改原来的对象。如果通给对象过赋值是达不到这样的效果的。因为原来的对象也会被修改。Object.assign()能够的实现对简单对象的拷贝。当对象复杂后使用Object.assign()就不能很好的拷贝对象了。

当对象属性值都是number、string或boolean时能够使用Object.assign()对对象的完整拷贝。
```javascript
const obj1 = {
    a: 0,
    b: 0
};
  
const obj2 = Object.assign({}, obj1);
obj2.a = 1;
console.log(obj2);  //{a: 1,b: 0}
console.log(obj1);  //{a: 0,b: 0},obj1并未改变
```
Object.assign()第一个参数是目标对象，第二个参数对象属性会覆盖第一个对象的同名属性，这里第一个对象时空的，所以相当于拷贝了一个obj1。注意，这里的obj1中的所有属性都是简单的值类型，这些值类型的属性拷贝过去都是生成了一个副本的。所以修改obj2中的属性并不会影响obj1。当obj1中有数组和对象这样的传递引用的值时这样拷贝就不行了。
```javascript
const obj1 = {
    a: 0,
    obj:{
        b: 0,
    }
};
const obj2 = Object.assign({}, obj1);

obj2.a = 1;
console.log(obj1.a);    //0
console.log(obj2.a);    //1
  
obj2.obj.b = 1;         //修改obj2
console.log(obj2.obj);  //{b: 1}，改变
console.log(obj1.obj);  //{b: 1}，也跟着改变
```
这里虽然和上面例子一样也是用Object.assign()对对象拷贝，但是本例中的obj1是一个复杂的对象，其属性值中有对象类型。前面我们知道了对象是传递引用的。所以在将obj1拷贝到新对象时由于obj属性是一个对象，所以新对象里拷贝的只是obj属性值（对象）的引用。因此obj1和obj2的obj属性都是引用的同一个对象。但是a属性是number类型，拷贝时是产生的副本，所以是不同的值。

## 对象的浅拷贝
上面我们使用Object.assign()拷贝对象其实就是浅拷贝。我们用函数实现浅拷贝。
```javascript
function objectCopy(obj){
    var newobj = {};
    for ( var attr in obj) {
        newobj[attr] = obj[attr];
    }
    return newobj;
}

var obj1 = {
    a: 0,
    obj:{b: 0}
};

var obj2 = objectCopy(obj1);

obj2.a = 1;
console.log(obj1.a);     //0
console.log(obj2.a);     //1

obj2.obj.b = 1;
console.log(obj1.obj);  //{b: 1}
console.log(obj2.obj);  //{b: 1}
```
我们遍历了被拷贝的对象直接属性，将属性值赋值给新对像的同名属性。这样拷贝对对象类型的属性只是拷贝的引用。新对象和旧对象的obj属相是指向同一个引用的。浅拷贝在es6中有更简单的写法。
```javascript
var obj1 = {x: 1,y: 1};
var obj2 = {...obj1};//obj2是obj1的浅拷贝
```
## 对象深拷贝
如果想拷贝对象里的所有属性实现真正意义上的完全拷贝，我们就需要遍对象历每一个属性，判断该属性的值是不是对象，如果是对象则继续遍历该对象属性直到属性都不再是对象，并将所有值给新对象。
```javascript
function objectCopy(obj){
    if(typeof(obj) != "object"){
        return obj;
    }
    var newobj = {};
    for(var attr in obj){
        newobj[attr] = objectCopy(obj[attr])
    }
    return newobj;
}

var obj1 = {
    a: 0,
    obj:{b: 0}
};
var obj2 = objectCopy(obj1);
obj2.obj.b = 1;
console.log(obj1.obj);  //{b: 0}
console.log(obj2.obj);  //{b: 1}
```
这样实现的对象拷贝无论被拷贝的对象中嵌套有多少层的对象都能实现完全的拷贝。

## 对象冻结
在es6中我们可以用const申明不可修改的变量。与赋值同理const申明变量能让number、string和boolean类型的变量不能被修改。但却只能让array和object的引用不被修改。还是能通过属性访问方式修改const声明的对象或数组内的值。
```javascript
const x = 0;
x = 1;          //TypeError: Assignment to constant variable.
```
```javascript
const obj = {
    x: 0
}
obj.x = 1
console.log(obj.x); //1

obj = {             //TypeError: Assignment to constant variable.
    y: 0
}
```
上面例子看出const申明的对象只能防止变量的引用不能被修改，但对象内部的属性还是能修改的。并不能真正意义上防止对象被修改。我们可以使用Object.freeze()冻结对象，但是Object.freeze()只能冻结对象的直接属性，属性如果是对象那么该属性内部的属性也能修改。
```javascript
const obj = Object.freeze({
    x: 0,
    y: {p: 0}
});
obj.x = 1;          //修改不生效
console.log(obj.x); //0

obj.p = {p: 1};     //修改对象属性的指向，不生效
console.log(obj.y)  //{p: 0}

obj.y.p = 1;        //修改生效，obj.y下的属性并未冻结
console.log(obj.y); //{p: 1}
```
我们可以像对象深拷贝一样使用该方法完全冻结对象。
```javascript
function objectFreeze(obj){
    Object.freeze(obj);
    for(var attr in obj){
        if(typeof(obj[attr]) == "object"){
            objectFreeze(obj[attr]);
        }
    }
}

const obj2 = {
    x:1,
    obj:{
        y:1,
        z:{
            a:1
        }
    }
}
objectFreeze(obj2);         //冻结对象
obj2.obj.z.a = 0;           //修改深层属性任然无效
console.log(obj2.obj.z.a)   //1
```
通过上面的方法我们就实现了一个真正不能修改的常量对象。

## 相关链接
* [const定义对象的可修改性](/es6/const.md)
* [返回目录](/README.md)
