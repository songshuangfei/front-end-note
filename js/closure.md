# 闭包
在es6之前js没有块作用域的概念，只有全局作用域和函数作用域。函数内定义的变量不能被直接访问，要访问函数内部定义的变量有两种方法。一种就是我们熟悉的函数返回值，还有一种就是返回定义在函数内部的函数，被返回的函数能访问与其同一作用域的变量，这样就形成了闭包。
## 闭包实现
```javascript
function A() {
    var num = 1;
    return function () {
        return num;
    }
}

var a = A();
console.log(a());//1
console.log(num);//ReferenceError: num is not defined
```
定义在函数`A()`内部的变量num只有在该函数内部才能访问，全局访问这个num变量就会有一个未定义的错误。但`A()`返回的是一个匿名函数，这个匿名函数是定义在A函数内的并能够访问到num变量，这个匿名函数被赋值到全局变量a上。所以能通过a访问到num变量。
## 自执行函数
上面的例子为了实现一个闭包定义了一个全局函数A，其实我们应该避免过多的全局变量。而且上面例子中A函数只会调用一次，目的只是实现一个闭包，仅仅这样我们就多了一个全局变量。正确的做法应该是使用一个匿名的自执行函数完成函数的定义和执行。自执行函数只需要在定义函数花括号后加一对括号。这个函数就不需其他地方调用而立刻执行。
```javascript
var a = function () {
    var num = 1;
    return function () {
        return num;
    }
}()                 //注意这里括号，这个函数会立即执行
console.log(a());   //1
```
上面使用了一个立即执行的函数，所以变量a接收到的值并不是这个外层函数而是这个外层函数的返回值即`function () {return num;}`。这样我们就不必再定义全局变量A了。注意，如果这个外层函数没有返回值，那么a将是undefined。
```javascript
var a = function () {
    var num = 1;
    //没有返回值
}()
console.log(a);//undefined
```
## 私有变量
js其实没有私有变量的概念，对象所有属性都能够访问到。但是我们通常人为的区分私有变量和公共变量方法就是在变量名前面加个下划线 `_`，但其实还是能访问到这个属性。有了闭包后我们就能实现拥有“私有变量”的对象。
```javascript
function NewCounter(i) {//计数器构造函数
    var num = i || 0;//计数起始值
    return {
        Add:function(){
            num++;
        },
        GetNum:function(){
            return num;
        }
    }
}
var c1 = NewCounter(3);     //实例化构造器（不需new关键字）
var c2 = NewCounter(2);
c1.Add();
c2.Add();
console.log(c1.GetNum());   //4
console.log(c2.GetNum());   //3
console.log(c1.num);        //undefined
```
上面匿名自执行函数返回的是一个对象，除了这个对象里包含的函数就没有其他函数能访问到num变量。这个num变量就像实例化对象的“私有变量”一样。
## 相关链接
* [返回目录](/README.md)

