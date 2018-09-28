# 原型链和构造函数
js每一个对象都会指向一个原型对象，并能从原型对象中继承属性。每个对象都能通过 `__proto__` 属性访问到本身原型对象。创建对象的方式不同，对象的原型也会不同。对象的原型层层指向就能构成原型链。
## 原型和原型链
js中最基本的对象是 `Object.prototype` 。js中所有的对象的原型链最后都会指向 `Object.prototype` 这个对象。对象字面量创建的对象的原型会直接指向 `Object.prototype` 。js中函数也是对象，函数的原型则不是直接指向 `Object.prototype` 而是直接指向 `Function.prototype`，`Function.prototype` 的原型再指向 `Object.prototype`（这就是一个原型链）。Number、Array这些类型也是先指向相应的原型，这个原型再指向js的标准对象（Object.prototype）。
```javascript
var obj ={      //对象字面量创建的对象
    x:1
}
console.log(obj.__proto__ === Object.prototype); //true 
//普通对象原型（obj.__proto__）直接指向Object.prototype

function con(){ //函数也是对象
}
console.log(con.__proto__ === Function.prototype); //true
//任意函数原型（con.__proto__）指向Function.prototype

console.log(Function.prototype.__proto__ === Object.prototype); //true
//函数原型的原型（Function.prototype.__proto__）指向Object.prototype

//所以有下面结论
console.log(con.__proto__.__proto__ === obj.__proto__);//true
//对象实例和函数的原型都指向最终的js标准对象Object.prototype
```
数组、数字、字符串、布尔都有和函数一样的原型链。
```javascript
console.log(Array.prototype.__proto__ === Object.prototype)     //true
console.log(String.prototype.__proto__ === Object.prototype)    //true
console.log(Number.prototype.__proto__ === Object.prototype)    //true
console.log(Boolean.prototype.__proto__ === Object.prototype)   //true
```
Array、String、Number、Boolean和Object这些都是构造函数，`new Object()` 就能实例化一个对象。总结：实例化的对象的原型指向其构造函数的prototype。除了Object()构造函数(Object已经是最基本的对象类型)构造函数的原型都指向js基本对象Object.prototype。
## 构造函数
js创建对象的方法最常用的就是通过对象字面量创建，这样创建的对象原型指向`Object.prototype`。通过`Object.create()`方法来创建对象，可以指定新对象原型为该方法的第一个参数而不是指向`Object.prototype`。构造函数创建对象也是常用的方法。通过构造函数创建对象也能够将创建的对象的原型指向任意对象。

使用`Object.create()`方法指定新对象的prototype。
```javascript
var obj = Object.create({
    x:1
})
console.log(obj)            //Object{}
console.log(obj.x)          //1，这个值是继承下来的
console.log(obj.__proto__)  //Object {x: 1}
```
`Object.create()`方法参数必须是一个对象或null。当参数是对象时能将创建的对象的原型指向这个参数对象。所以就能从中继承属性。

js基本类型的构造函数创建对象的方法。
```javascript
var arr = new Array();
console.log(arr.constructor);//function Array()

var obj = new Object();
console.log(obj.constructor);//function Object()

//字面量创建也一样
var arr2 =[];
console.log(arr2.constructor);//function Array()
```
### 自定义构造函数。
```javascript
function Person(name){          //这里的Person就和Array、Number一样了，不过是我们自定义的类型。
    this.name = name;           //构造函数里的this代表新创建的对象
    this.seyHello = function(){
        console.log("Hi I am "+ this.name);
    }
}
var p = new Person("Jack");     //创建新对象
p.seyHello();                   //Hi I am Jack
console.log(p)                  //Person {name: "jack", seyHello: }
//构造函数里向this上添加的属性就会再实例化的新对象里

console.log(p.constructor === Person);//实例对象的构造函数

console.log(p.__proto__ === Person.prototype);//true
```
上面最后一行我们看到，实例对象的原型就是构造函数的prototype属性。所以实例化的对象会继承构造函数的prototype属性里的所有属性。
### 继承
由上面知到，我们可以在构造函数的prototype里添加属性，让由该构造函数实例化的对象都能继承prototype里的属性。
```javascript
function Person(name){
    this.name = name;
}

Person.prototype = {
    sayhi:function () {
        console.log("Hi I am " + this.name)
    }
}

var p = new Person("Jack");
var p2 = new Person("Tom");
p.sayhi();//Hi I am Jack
p2.sayhi();//Hi Iam Tom
console.log(p);//Person {name: "Jack"}
console.log(p2);//Person {name: "Tom"}
```
我们将每个对象都会有的相同的方法`sayhi()`,放到构造函数的prototype里了。这样实例化出来的每个对象都会有这个方法。在最后两行的执行结果里我们看到实例化的对象里其实看不到sayhai这个属性。这是因为在访问对象属性时会先在对象自己的属性里寻找，如果没有找到就继续在原型里寻找，以此类推，直到最后的Object.prototype。如果在实例化的对象里添加同名属性则会覆盖原型链中的属性，但不会修改原型链中的属性。
```javascript
p2.sayhi = function(){
    console.log("Hello I am "+this.name);
}

console.log(p);//Person {name: "Jack"}
console.log(p2);//Person {name: "Tom",sayhi:}, p2里添加了一个同名方法
p.sayhi();//Hi Iam Tom
p2.sayhi();//Hello Iam Tom
```
在p2里添加了一个同名方法p2将使用自己的方法，不再在原型链中寻找。而p没有自己的方法，只有使用原型链中的方法。上面的p和p2就是构建了原型链。以p为例，`p.__proto__`指向`Person.prototype`(即`{sayhi:function(){}}`)。`{sayhi:function(){}}`指向`Object.prototype`。
## 原型运用
我们可以向`constructor.prototype`添加我们想给这个类型添加的方法。无论这个constructor是类似Array、Number这样的默认类型还是像上面的Person一样自定义的类型。

给数组添加一个求所有元素和的方法。
```javascript
Array.prototype.getSum = function(){
    var sum =0;
    for(var i =0 ;i< this.length;i++){
        sum += this[i]
    }
    return sum;
}

var arr = new Array();
arr = [1,2,3,4];
console.log(arr.getSum());
```
## 相关链接
* [对象的拷贝与冻结](/js/objectCopyFreeze.md)
* [返回目录](/README.md)

