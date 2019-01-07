# typescript类型
## 原始类型
ts基本类型有以下：

|类型|说明|
|:-|:-|
|boolean（布尔）|略|
|number（数字）|与js一样是浮点型数|
|string（字符串）|略|
|array（数组）|略|
|Tuple（元组）|元组类型允许表示一个已知元素数量和类型的数组|
|enum（枚举）|略|
|any|不清楚类型的类型|
|Void|没有类型（与void相对）|
|Null 和 Undefined|与void相似，是所有类型的子类型。|
|Object|表示非原始类型|
### 数组
ts中数组可以限制元素类型唯一，也可以指定元素类型的范围。
```ts
var nums:number[] = [1,2,3];//元素只能为数字

//泛型
var list: Array<number|string> = [1,"1"];//限制元素类型为number或string

```
### 元组
元组的元素要和定义时数组位置的类型一样。
```ts
var x:[number,string] = [1, '1'];
x[0] = '1';//不能将string类型值赋值给number
```
## 枚举
枚举的第一个属性摩恩会是0，之后的属性依次加1。也可以自己设置起始值。
```ts
enum ActionTypes {
    add = 1,
    del,
    toggle
}
```
枚举用法。
```ts
enum ActionTypes {
    add,
    del,
    toggle
}

function S(type: ActionTypes){
    switch (type){
        case ActionTypes.add:
            //...
            break;
        case ActionTypes.del:
            //...
            break;
        case ActionTypes.toggle:
            //...
            break;
    }
}
```

##  any
any可以接受任何类型值。
```ts
let x:any;
x = "1";
x = 1;
```