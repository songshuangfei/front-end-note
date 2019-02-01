# 链表
在储存多个数据时我们多数时候会想到使用数组。像C语言这样的语言中创建数组时数组长度和内存都分配好了，当插入新的元素时就会改变元素内存，这样的使用数组的成本就很高。js数组虽然是动态的，其实底层也会是这样的。链表就比数组要高效，链表相比数组的特点：
* 链表并不像数组的每个元素是真正连续的，链表的每个元素都只是在概念上的连续。
* 链表的每个元素都不是在内存中连续的，每个元素使用指针指向其本身后面的元素（也可能有指向前的指针），这样就形成了一个链条（链表），拥有了概念上的先后顺序。
* 当要修改链表结构时就只需要修改链表元素指针的指向。并不会修改链表元素在内存中的结构，这样就更加高效了。
* 数组能直接访问到数组某个索引的元素，而链表要从表头开始迭代直到所需元素。
## 单向链表
定义一个链表类。
```js

let LinkedList = (function(){

    // 链表节点
    function Node(element){
        this.element = element;
        this.next = null;
    };

    let length = 0;
    let head = null;

    function LinkedList(){};

    _LinkedList.prototype = {
        get size(){
            return length;
        },
        get isEmpty(){
            return length === 0;
        }
    };

    return _LinkedList;
})();
```
Node是链表节点，节点上的next属性是下一个链表节点的引用。head是链表头节点。现在链表方法不完整，继续添加方法。
### 尾部追加元素
当头节点是null就把新增加的节点作为头节点。如果头节点非null就迭代链表到尾节点把尾节点的next指向新节点。
```js
_LinkedList.prototype.append = function(element){
    let node = new Node(element),
        current;
    if(head === null){
        head = node;
    }else{
        current = head;
        while(current.next){
            current = current.next;
        }
        current.next = node;
    }
    length++;
};
```
### 移除元素
移除头节点时就修改head指针指向第二个节点，移除其他节点时就将被移除节点前一个节点的next指向被移除节点后一个节点。被移除的节点就没有被继续引用了，将会被GC回收。
```js
_LinkedList.prototype.removeAt = function(position){
    if(position > -1 && position < length){
        let current = head,
            previous,
            index = 0;
        if(position === 0){
            head = current.next;
        } else {
            while(index++ < position){
                previous = current;
                current = current.next;
            }
            previous.next = current.next;
        };
        length --;
        return current.element;
    }else{
        return null;
    }
}
```
### 插入元素
插入元素和移除元素大致相似。在首位插入元素时将新元素的nex指向head再将head指向新节点。在其他位置插入元素时就将新元素的next指向被插入位置的元素。在将被插入位置前的元素的next指向新元素。
```js
_LinkedList.prototype.insert = function(position, element){
    if(position >= 0 && position <= length){
        let node = new Node(element),
            current = head,
            previous,
            index = 0;
        if(position === 0){
            node.next = current;
            head = node;
        } else {
            while(index++ < position){
                previous = current;
                current = current.next;
            }

            node.next = current;
            previous.next = node;
        }
        length++;
        return true;
    } else {
        return false;
    }
}
```
### 其他方法
遍历链表并把元素拼接成字符串。
```js
_LinkedList.prototype.toString = function(){
    let str = "",current = head;
    while(current){
        str = str + current.element + " ";
        current = current.next;
    }
    return str;
}
```
indexOf方法，返回第一个匹配的元素的index。
```js
_LinkedList.prototype.indexOf = function(element){
    let index = 0,
    current = head;

    while(current){
        if(current.element === element){
            return index;
        }
        index++;
        current = current.next;
    }
    return -1;
}
```
getHead方法，用来返回链表的头节点。其实如果链表方法实现得足够好，一般不要用这个方法。
```js
_LinkedList.prototype.getHead = function(){
    return head;
}
```
完整例子
```js
let LinkedList = (function(){

    // 链表节点
    function Node(element){
        this.element = element;
        this.next = null;
    };

    let length = 0;
    let head = null;

    function LinkedList(){};

    LinkedList.prototype = {
        get size(){
            return length;
        },
        get isEmpty(){
            return length === 0;
        },
        append(element){
            let node = new Node(element),
                current;

            if(head === null){
                head = node;
            }else{
                current = head;
                while(current.next){
                    current = current.next;
                }
                current.next = node;
            }
            length++;
        },
        removeAt(position){
            if(position > -1 && position < length){
                let current = head,
                    previous,
                    index = 0;

                if(position === 0){
                    head = current.next;
                } else {
                    while(index++ < position){
                        previous = current;
                        current = current.next;
                    }
                    previous.next = current.next;
                };
                length --;
                return current.element;
            }else{
                return null;
            }
        },
        insert(position, element){
            if(position >=0 && position <= length){
                let node = new Node(element),
                    current = head,
                    previous,
                    index = 0;
                if(position === 0){
                    node.next = current;
                    head = node;
                } else {
                    while(index++ < position){
                        previous = current;
                        current = current.next;
                    }

                    node.next = current;
                    previous.next = node;
                }
                length++;
                return true;
            } else {
                return false;
            }
        },
        toString(){
            let str = "",current = head;
            while(current){
                str = str + current.element + " ";
                current = current.next;
            }
            return str;
        },
        indexOf(element){
            let index = 0,
            current = head;

            while(current){
                if(current.element === element){
                    return index;
                }
                index++;
                current = current.next;
            }
            return -1;
        },
        getHead(){
            return head;
        }
    };
    return LinkedList;
})();


var l = new LinkedList();
console.log(l.size,l.isEmpty);//0 true
l.append("a");
l.append("b");
l.append("c");
console.log(l.insert(1,"s"));true
console.log(l.size,l.isEmpty);//4 false
console.log(l.toString());//a s b c
console.log(l.indexOf("a2"));//-1
```
## 双向链表
双向链表和单向链表的区别是每一个节点除了有一个next指针指向下一个节点外还有一个prev指针指向前一个节点。
```js
let DoublyLinkedList = (function(){
    let Node = function(element){
        this.element = element;
        this.prev = null;//增加
        this.next = null;
    };
    let length = 0;
    let head = null;
    let tail = null;//增加

    function _DoublyLinkedList(){};

    _DoublyLinkedList.prototype = {
        get size(){
            return length;
        },
        get isEmpty(){
            return length === 0;
        }

    };
    return _DoublyLinkedList;
})();
```
### 添加元素
在双向链表中添加了一个tail指针，指向尾节点。每次添加新节点时就不必再遍历到尾节点了。在添加了新节点后必须把tail指针指向新的尾节点。
```js
_DoublyLinkedList.prototype.append = function(){
    let node = new Node(element);
    if(head === null){
        head = node;
        tail = node;
    }else{
        let current = tail;
        current.next = node;
        node.prev = current;
        tail = node;
    }
    length++;
}
```
### 插入元素
插入元素和单向列表区别是在插入元素后要对新元素和插入位置后的一个元素的prev指针设置正确的值（指向本身前一个元素），在最后位置插入元素时要将tail指针指向尾节点。
```js
_DoublyLinkedList.prototype.insert = function(position, element){
    if(position >=0 && position <= length){
        let node = new Node(element),
        current = head,
        previous,
        index = 0;
        if(position === 0){
            if(!head){
                head = node;
                tail = node;
            } else {
                node.next = current;
                current.prev = node;
                head = node;
            }
        } else if (position === length){
            current = tail;
            current.next = node;
            node.prev = current;
            tail = node;
        } else {
            while(index++ < position){
                previous = current;
                current = current.next;
            }
            node.next = current;
            previous.next = node;

            current.prev = node;
            node.prev = previous;
        }
        length++;
        return true;
    } else {
        return false;
    }
}
```
### 移除元素
移除元素主要注意首尾两个位置。移除首节点要将head指向第二个元素，并且将新head的prev值改为null。移除尾节点时要将tail指针指向倒数第二个元素并将新tail的next值修改为null。
```js
_DoublyLinkedList.prototype.removeAt = function(position){
    if(position > -1 && position < length){
        let current = head,
        previous,
        index = 0;
        if(position === 0){//移除第一项
            head = current.next;
            if(length === 1){
                tail = null;
            } else {
                head.prev = null;
            }
        } else if (position === length-1){//移除最后一项
            current = tail;
            tail = current.prev;
            tail.next = null;
        }else{
            while(index++ < position){
                previous = current;
                current = current.next;
            }
            previous.next = current.next;
            current.next.prev = previous;
        }
        length--;
        return current.element;
    } else {
        return null;
    }
},
```
## 相关链接
* [返回目录](/README.md)