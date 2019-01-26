# 栈
栈是一种常用的数据结构。栈可以想象成一个依次只能放进一个物体的桶，“桶底”叫做栈底“桶口”叫做栈顶。放入元素只能从栈顶放入，拿出元素也只能从栈顶拿出，要拿出栈底的元素只能先拿出其上面的所有元素。即栈的规则“先进后出，后进先出”。

用js的数组实现一个栈的类。
```js
var Stack =(function(){
    let items = [];

    function stack(){};

    stack.prototype = {
        get size(){
            //返回栈内元素个数
            return items.length;
        },
        get isEmpty(){
            //栈是否为空
            return items.length === 0;
        },
        push(i){
            //入栈
            items.push(i)
        },
        pop(){
            //出栈，并返回出栈元素
            if(items.length === 0){
                return null;
            }
            return items.pop()
        },
        peek(){
            //返回栈顶元素
            if(items.length === 0){
                return null;
            }
            return items[items.length - 1]
        },
        clear(){
            //清空栈
            items = [];
        },
        print(){
            //输出栈内容
            console.log(items.toString());
        }
    }
    return stack;
})();

var s1 = new Stack();
s1.push(1);
s1.push(2);
console.log(s1.peek());//2
console.log(s1.size, s1.isEmpty);//2 false
s1.print();//1,2
while(1){
    if(s1.isEmpty){
        break;
    }
    console.log(s1.pop());
    //2
    //1
}
console.log(s1.peek());//null
```
## 栈溢出
当栈已经满了不能再继续添加元素就是栈溢出。js的数组是动态数组，向数组添加元素时数组长度会自动增加，所以这里不用考虑溢出。

## 栈应用
使用栈实现10进制到2进制的转换。
```js
function toBinaryStr(number){
    let remStack = new Stack(),
        rem,
        result = '';

    while(number > 0){
        rem = Math.floor(number % 2);//如果number有小数Math.floor就很有必要
        remStack.push(rem);
        number = Math.floor(number / 2);
    }

    while(!remStack.isEmpty){
        result += remStack.pop().toString();
    }

    return result;
}

console.log(toBinaryStr(3));//11
```
## 相关链接
* [返回目录](/README.md)