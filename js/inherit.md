# 类的继承
js中定义一个类只需要使用一个函数就能实现，这个函数也可以叫构造函数。
## 定义类
```js
var Person = function(name,age,sex){
    this.name = name;
    this.age = age;
    this.sex = sex;
}
var p1 = new Pserson("Leo", 18, "male");
console.log(p1);//Person {name: "Leo", age: 18, sex: "male"}
```
这样我们就建立了一个Person类，这个函数也是这个类的构造函数。使用`new Person()`就能通过该构造函数实例化一个Person类型的对象。
## 为类添加共有属性和方法
当类有一些方法或属性是所有实例都可以用的时我们应该添加到类的原型`Person.prototype`。添加到原型里的属性或方法能被能实例继承，实例能够访问这些共有的属性或方法。当然我们也可以直接在每一个实例里初始化这个都要使用到的共有方法。不过这样在每一个实例里都初始化一个相同的方法就显得有些冗余还伴有不必要的内存开销。如果方法是添加在类的原型里，那么无论实例化多少个对象，这些对象都只是对原型里的方法保持引用，并不会在每一个实例里都创建一个方法。

正确添加公共方法的方式如下：
```js
var Person = function(name,age,sex){
    this.name = name;
    this.age = age;
    this.sex = sex;
}

Person.prototype.sayHi = function(){//添加方法
    console.log("Hi my name is " + this.name);
}
//或者
// Person.prototype = {
//     sayHi: function(){
//         console.log("Hi my name is " + this.name);
//     }
// }
var p1 = new Person("Leo", 18, "male");
var p2 = new Person("Tom", 18, "male");
p1.sayHi();//Hi my name is Leo
p1.sayHi();//Hi my name is Tom
```
从上面的例子我们就看到类实例化出的每一个对象都能访问到类原型里的方法。
## 类的继承
js类的继承有很多方法最常用的是原型链继承，还能通过调用父类的构造函数来初始化子类的对象。
### 原型链继承
原型链继成的主要方法就是把父类的实例作为子类的原型。前面的`Person.prototype = {sayHi：func(){...}}`语句就像是让Person继承了一个普通对象。

通过原型继承：
```js
var Person = function(name,age,sex){
    this.name = name;
    this.age = age;
    this.sex = sex;
};

Person.prototype.sayHi = function(){
    console.log("Hi my name is " + this.name);
};

var p1 = new Person("Leo", 18, "male");//实例化一个父类对象

var Student = function(id){
    this.id = id;
};

Student.prototype = p1;
var stu1 = new Student(1001);
console.log(stu1);//Student {id: 1001}
stu1.sayHi();//Hi my name is Leo
```
通过原型链继承父类使用起来有点麻烦，需要先实例化父类的一个对象，再将子类的prototype指向这个实例化的父类对象。父类实例的属性和方法并不存在子类实例化的对象里，但是子类实例继承了父类实例的属性和方法。所以子类实例也能通过属性访问的方式获取到父类实例里的属性和方法。
### 构造继承
构造继承原理就是在子类构造函数内部调用父类构造函数，用父类构造函数来初始化子类实例的部分属性，子类构造函数在调用父类构造函数同时初始化子类的属性。

调用父类构造函数：
```js
var Person = function(name,age,sex){
    this.name = name;
    this.age = age;
    this.sex = sex;
};

Person.prototype.sayHi = function(){
    console.log("Hi my name is " + this.name);
};

var Student = function(name, age, sex, id){
    Person.call(this, name, age, sex);//注意这里使用了函数的Call方法
    this.id = id;
};

var stu1 = new Student("Jack", 22, "male", 1001);
console.log(stu1);//Student {name: "Jack", age: 22, sex: "male", id: 1001}
console.log(stu1.sayHi);//undefined
```
上面例子就是通过在子类构造函数内调用父类的构造函数实现了继承。其实这里并不是真正的继承，只是修改了父类构造函数内部的this指向（修改函数内部this参考[函数的call，apply，bind方法](/js/call&apply&bind.md)）并执行了父类的构造函数。所以这里并没有实例化一个父类对象，只是子类构造函数调用了父类构造函数实例化了一个子类实例，所以子类实例的属性全部在实例里面（通过上面倒数第二行可以看出）。因为这里的实例只是一个子类的实例，所以并不能继承父类的原型链里的属性和方法。所以`stu1.sayHi`是undefined。
### 组和继承
组和继承实际上是对构造继承的的一点优化，子类任然能对父类构造函数传参，同时也能访问父类原型链中的属性和方法。方法就是把子类构造函数的原型指向父类的一个实例。因此子类也能继承父类原型链中的属性和方法了。
```js
var Person = function(name,age,sex){
    this.name = name;
    this.age = age;
    this.sex = sex;
};

Person.prototype.sayHi = function(){
    console.log("Hi my name is " + this.name);
};

var Student = function(name, age, sex, id){
    Person.call(this, name, age, sex);
    this.id = id;
};

Student.prototype = new Person();//增加了这一行

var stu1 = new Student("Jack", 22, "male", 1001);
console.log(stu1);//Student {name: "Jack", age: 22, sex: "male", id: 1001}
console.log(stu1.__proto__);//Person {name: undefined, age: undefined, sex: undefined}，这个是一个父类实例
stu1.sayHi();//Hi my name is Jack
```
通过上面的例子我们看到子类实例也能调用父类原型链中的方法了。因为子类构造函数的原型指向了一个父类构造函数的实例，所以子类实例能通过原型链访问到父类原型中的方法。当我们使用`stu1.name`获取属性时其实是获取的子类实例的属性，子类属性覆盖了原型链中父类实例（父类实例是`stu1.__proto__`）的同名属性属性。然而当我们调用`stu1.sayHi()`方法时子类实例中没有这个方法，就会在原型链中寻找这个方法直到找到父类的原型中的这个方法。注意，虽然`stu1.sayHi()`方法来自父类，但是函数内部this是指向调用方法的对象的，所以这里的`stu1.sayHai()`方法内部this指向了子类实例`stu1`。

下面代码就能清晰展示了方法的调用方式影响了方法内部的this指向：
```js
stu1.sayHi();//Hi my name is Jack
stu1.__proto__.sayHi();//Hi my name is undefined
```
## 寄生组合继承
寄生组和继承实际上是对组和继承的优化，组和继承会分别在父类实例和子类实例中重复创建相同的属性。我们通过子类实例只能访问到子类中的属性，虽然父类实例属性被覆盖，但这些属性确实是存在的，而且是不必要的存在。寄生组和继承就对这一问题优化了。
```js
var Person = function(name,age,sex){
    this.name = name;
    this.age = age;
    this.sex = sex;
};

Person.prototype.sayHi = function(){
    console.log("Hi my name is " + this.name);
};

var Student = function(name, age, sex, id){
    Person.call(this, name, age, sex);
    this.id = id;
};

(function(){
    var Super = function(){};
    Super.prototype = Person.prototype;
    Student.prototype = new Super();
})();

var stu1 = new Student("Jack", 22, "male", 1001);
console.log(stu1);//Student {name: "Jack", age: 22, sex: "male", id: 1001}
console.log(stu1.__proto__);//Person {}
```
通过这种方式我们就可以看到父类实例`stu1.__proto__`就是一个空对象了。
## 相关链接
* [函数的call，apply，bind方法](/js/call&apply&bind.md)
* [返回目录](/README.md)
