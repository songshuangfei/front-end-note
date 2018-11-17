# 函数的call，apply，bind方法
call()，apply()，bind()都是`Founction.prototype`上的方法，它们都能改变函数调用时函数内部的this指向。
## call()和apply()
> 在 javascript 中，call 和 apply 都是为了改变某个函数运行时的上下文（context）而存在的，换句话说，就是为了改变函数体内部 this 的指向。JavaScript 的一大特点是，函数存在「定义时上下文」和「运行时上下文」以及「上下文是可以改变的」这样的概念。
```js
let Person = function(name){
    this.name = name;
}

Person.prototype.sayHi = function(){
    console.log(`Hi, my name is ${this.name}`);
}

let p1 = new Person("song");
let p2 = {name: "liu"};
p1.sayHi();             //Hi, my name is song
p1.sayHi.call(p2);      //Hi, my name is liu
p1.sayHi.apply(p2);     //Hi, my name is liu
```
直接普通调用p1的`sayHi()`方法，`p1.sayHi()`中的this是指向p1本身的。当使用`call()`和`apply()`调用任意函数时，被调用函数内部的this将指向call和apply的第一个参数。所以当我们p2中没有`sayHi()`这个方法时我们就能能够用其他对象的`sayHi()`方法，在调用时只需把p2传到call或apply的第一个参数。

call和apply的区别在于它们传入参数的方式不同。call和apply的第一个参数将作为被调用函数的this。call方法可以有任意多个参数，call方法的第二参数开始将完整的作为被调用函数的参数。而apply方法只能有两个参数，第二个参数是一个数组。
```js
let sum = {
    result:0
}
sum.result = 10;

let add1 = function(a,b){
    let tmp = a + b;
    this.result += tmp;
}

let add2 = function(){
    let tmp = 0;
    for(i in arguments){
        tmp += arguments[i]
    }
    this.result += tmp;
}
add1.call(sum,1,2);
console.log(sum.result); //12
add2.apply(sum,[1,2,3,4]);
console.log(sum.result);//23
```
apply方法会把第二个参数（数组）的所有元素传入被调用函数作为参数。apply这样的应用场景就很像es6的展开操作符`...`。
```js
let data = [1,2,3,4];
add2(...data);
```
# bind
> bind()方法会创建一个新函数，称为绑定函数，当调用这个绑定函数时，绑定函数会以创建它时传入 bind()方法的第一个参数作为 this，传入 bind() 方法的第二个以及以后的参数加上绑定函数运行时本身的参数按照顺序作为原函数的参数来调用原函数。
```js
let obj1 = {
    x: 1,
    getX:function () {
        console.log(this.x)
    }
};
let obj2 = {
    x: 2,
};
let getX = obj1.getX.bind(obj2);
obj1.getX();    //1
getX();         //1
```
bind并不会和call，apply一样执行函数，bind将返回一个新的函数，新函数和bind前面的函数一样，不过这个新函数内部的this将绑定到bind的第一个参数。bind传参数方式和call一样。对上面的例子进行修改。
```js
let obj1 = {
    x: 1,
    getX:function () {
        console.log(arguments)//修改1
    }
};
let obj2 = {
    x: 2,
};

let getX = obj1.getX.bind(obj2, 1, 2);//修改2
obj1.getX();//Arguments(0) []
getX();//Arguments(2) [1, 2]
```
bind的第二个参数开始将被作为bind返回的新函数的参数。
## 相关链接
* [返回目录](/README.md)