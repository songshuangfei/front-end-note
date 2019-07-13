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
