# 面试总结
* 京东面试，自己实现一个bind方法。
```js
Function.prototype.myBind=function(ctx){
    let args = Array.prototype.slice.call(arguments,1);
    let that = this;
    return function(){
        let a = arguments.length===0?args:arguments
        that.apply(ctx,a)
    }
}

function aa(a,b){
    console.log(this.x+a+b);
}

let obj={
    x:1
}

let bindedaa = aa.myBind(obj,2,5);

bindedaa();//8
bindedaa(1,1);//3
```
* 京东面试:

问：多久会输出1
```js
setTimeout(() => {
    console.log("1")
}, 5000);

while(){
    //阻塞6秒
}
console.log("2");
```
答：
```js
//emm 
```

* 知道创宇笔试：

问：
```js
function a(){
    console.log("a");
}

function b(){
    // 无论b执行多少次，a只会60s后才能再执行
    //这里写上逻辑
    a();
}
```
答：
```js
function a(){
    console.log("a");
}

function b(){
    console.log("b");
    
    if(!global.time){
        global.toggle = false;
        global.time = setInterval(() => {
            global.toggle = false;
        }, 6000);
    }
    if(global.toggle){
        return;
    }
    global.toggle = true;
    a();
}

b();
b();
b();
b();
b();
b();

setTimeout(() => {
    b();
}, 7000);
```
