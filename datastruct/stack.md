# 栈
```js
var Stack =(function(){
    let items = [];

    function stack(){};

    stack.prototype = {
        get size(){
            return items.length;
        },
        get all() {
            return items;
        },
        get isEmpty(){
            return items.length === 0;
        },
        push(i){
            items.push(i)
        },
        pop(){
            return items.pop()
        },
        
    }
    return stack;
})();

var s1 = new Stack();
s1.push(1);
s1.push(2);
s1.push(3);
s1.push(4);
s1.push(5);
console.log(s1.size);
while(1){
    if(s1.isEmpty){
        break;
    }
    
    console.log(s1.pop());
}

console.log("end");

```