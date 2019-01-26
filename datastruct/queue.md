# 队列
队列可以想象成一个单行管道，只能从一端进从另一端出。只有之前进入队列的元素出去了后面的元素才可以出去。即“先进先出，后进后出”。
## 普通队列
```js
var Queue = (function(){
    let items = [];
    function Queue(){};
    Queue.prototype = {
        get isEmpty(){
            return items.length === 0;
        },
        get size(){
            return items.length;
        },
        enqueue(i){//入队列
            items.push(i);
        },
        dequeue(){//出队列
            if(items.length === 0){
                return null;
            }
            return items.shift();
        },
        front(){
            return items[0];
        },
        print(){
            console.log(items.toString());
        }
    };

    return Queue;
})()

let q = new Queue();
console.log(q.isEmpty);
q.enqueue(1);
q.enqueue(2);
console.log(q.size,q.front());
console.log(q.dequeue(),q.size,q.front());
```
## 优先队列
优先队列和普通队列区别就是在入队列的时候需要给元素一个优先级，按优先级的大小在当前队列中相应的位置插入这个新元素。出队列时任然是和普通队列是一样的行为。
```js
var PriorityQueue = (function(){
    let items = [];

    function QueueElement(element,priority){
        this.element = element;
        this.priority = priority;
    };

    function PriorityQueue(){};

    PriorityQueue.prototype = {
        get isEmpty(){
            return items.length === 0;
        },
        get size(){
            return items.length;
        },
        enqueue(element, priority){//入队列,同时必须要给一个优先级
            let newElement = new QueueElement(element, priority);

            let added = false;
            for(let i=0; i < items.length; i++){
                if(newElement.priority < items[i].priority){
                    items.splice(i, 0, newElement);
                    added = true;
                    break;
                }
            }
            if(!added){
                items.push(newElement);//没有更大的优先级数，就添加再尾部
            }
        },
        dequeue(){//出队列
            if(items.length === 0){
                return null;
            }
            return items.shift();
        },
        front(){
            return items[0];
        },
        print(){
            console.log(items.toString());
        }
    };

    return PriorityQueue;
})()

let q = new PriorityQueue();
console.log(q.isEmpty);
q.enqueue("a",1);
q.enqueue("b",2);
q.enqueue("c",1);
q.enqueue("d",3);
q.enqueue("e",1);
q.enqueue("f",2);
while(!q.isEmpty){
    console.log(q.dequeue());
}
// QueueElement { element: 'a', priority: 1 }
// QueueElement { element: 'c', priority: 1 }
// QueueElement { element: 'e', priority: 1 }
// QueueElement { element: 'b', priority: 2 }
// QueueElement { element: 'f', priority: 2 }
// QueueElement { element: 'd', priority: 3 }
```
## 相关链接
* [返回目录](/README.md)
