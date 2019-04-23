# async & await
async和await是`Promise`的语法糖。
## async
async的使用很简单。只需将其加在一个函数前面就行。
```js
async function A(){};
```
执行下这个函数。
```js
async function A(){};

let a  = A();

console.log(a); //Promise {<resolved>: undefined}
```
这个函数结果是返回了一个`Promise`对象，而且这个`Promise`对象的状态是`resolved`状态，resolve的值是undefined。

给函数添加一个返回值。
```js
async function A(){
    return "data";
};

let a  = A();

console.log(a);// Promise {<resolved>: "data"}
```
这次返回的`Promise`对象就resolve了函数的return的值。所以async修饰的函数被执行后返回的是一个`Promise`对象，这个Promise对象会立即resolve这个async函数的返回值。

async函数返回的是`Promise`对象，可以在asycn函数后链式调用Promise的`then`方法和`catch`方法。当函数成功return时（相当于Promise对象的resolve）就会执行`then`的回调函数。如果函数在return之前发生错误（相当于Promise对象的reject），就会执行`catch`的回调函数。如下：
```js
async function A(){
    const n = Math.random();
    if(n > 0.5){
        throw("an err");
    } else {
        return "data";
    }
};

A().then(data => {
    console.log(data)
}).catch(err => {
    console.log(err)
});
```
所以上面这个async函数和下面的Promise等价。
```js
let a = new Promise((rs,rj)=>{
    const n = Math.random();
    if(n > 0.5){
        rj("an err");
        // or throw("an err");
    } else {
        rs("data")
    }
});

a.then(data=>{
    console.log(data)
}).catch(err=>{
    console.log(err)
});
```
总结：async函数被执行时会返回一个立即resolve的`Promise`,而且这个promise会resolve这个async函数return的值。
## await
因为async函数的返回值作为resolve，就不能像`Promise`对象一样能异步resolve或reject。这样就有了await。await是用来执行一个`Promie`异步操作的，将返回`Promise`对象resolve的值。但await只能在async函数内使用。
```js
await new Promise((rs,rj)=>{
    rs("data");
});
// 将这段代码粘贴到Chrome的控制台会返回“data”,
// 但实际上await命令只能在async函数中使用
```
await的使用方法
```js
async function A(){
    let d = await new Promise((rs,rj)=>{
        setTimeout(() => {
            rs("Hi");
        }, 200);
    })
    d += "!";
    console.log(d)；
}

A();//Hi
```
在async函数里使用await调用`Promise`，能够让async内像同步执行一样。由前面知道async函数调用后返回一个`Promise`对象，这个`Promise`对象会resolve函数return的值。我们尝试将async内部`Promise`对象的结果return。
```js
async function A(){
    let d = await new Promise((rs,rj)=>{
        setTimeout(() => {
            rs("Hi");
        }, 200);
    })
    d += "!";
    return d;
}

console.log(A()); //Promise { <pending> }

A().then(d=>{
    console.log(d);//Hi!
})
```
所以在async函数最后返回的新的Promise会resolve内部Promise的resolve的结果d。

下面两个例子一个是用`Promise`实现，一个是用`async/await`实现。是相似的。
```js
async function A(){
    let a = await new Promise((rs,rj)=>{ // 第一个promise
        console.log(1)
        rs("Hi!");
    });
    // 第一个promise 结束
    console.log(2)
    return a; // 第二个promise resolve
}

let newP = A();// 第二个promise

newP.then(data => {
    // 第二个promise结束
    console.log(data)
});

console.log(newP);
```
```js

let p = new Promise((rs,rj)=>{ // 第一个 promise
    console.log(1);
    rs("Hi!");
    // 第一个promise 已经结束
});

let newP = p.then(data=>{ // 第二个promise (promise 的 then会返回一个promise)
    console.log(2);
    return data;// 第二个promise resolve（相当于async函数的return）
});

newP.then(data=>{
    // 第二个promise结束
    console.log(data)
});

console.log(newP);
```

两个例子结果都如下：

![result](./img/async&await01.png)

上面第二个例子的`promise`的then的也会返回一个新的`promise`，并resolve其传入回调函数的返回值，作用和async函数一样。比如：
```js
let p = new Promise((rs,rj)=>{
    rs("随便是啥都不会用");
});

let p2 = p.then(d=>{
    return "没用第一个promise的值";
});

p2.then(d=>{
    console.log(d);// 没用第一个promise的值
});

```
```js
function p(){
    return new Promise((rs,rj)=>{
        rs("随便是啥都不会用");
    })
}

async function A(){
    let d = await p();
    return"没用第一个promise的值";
}


A().then(d=>{
    console.log(d)
})
```
两个例子结果都如下：

![result](./img/async&await02.png)

总结: async函数中使用await后，能够像同步一样执行。可以理解成在async函数中，await命令之后的代码相当于在被await修饰的Promise的then的回调中执行。
## 相关链接
* [返回目录](/README.md)