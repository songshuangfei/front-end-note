# 冒泡排序
冒泡排序原理是从左到右依次遍历数组每一个元素，将当前元素与其后一个元素对比，若遍历到的当前元素比后一个元素大（逆序）则将当前元素与其后一个元素交换位置。如果当前元素比后一个元素小（顺序）则不做交换处理，继续向后遍历。当第一次遍历后就能将被排序元素中的最大数放到最后位置。如果总共有n个元素，第二次遍历就只需遍历前n-1个元素，第三次遍历前n-2个元素，以此类推。

双重for循环实现冒泡排序。
```js
Array.prototype.BubbleSort = function(){
    let l = this.length;
    for( let i=0; i<l; i++){
        for(let j=0; j<l-i-1; j++){//j小于
            if(this[j]>this[j+1]){
                [this[j],this[j+1]]=[this[j+1],this[j]];//es6解构赋值交换数组元素位置
            }
        }
    }
}
var a =[5,2,3,4,23,45,88,22];
a.BubbleSort();
console.log(a);//Array(8) [2, 3, 4, 5, 22, 23, 45, 88]
```
while循环和for循环实现冒泡排序。
```js
Array.prototype.BubbleSort =function () {
    let i = this.length;
    while(i > 0){
        for(let j = 0; j < i-1; j++){
            if(this[j] > this[j+1]){
                [this[j],this[j+1]] = [this[j+1],this[j]];
            }
        }
        i--;
    }
}
var a =[5,2,3,-1,4,23,45,88,22];
a.BubbleSort();
console.log(a);//Array(8) [-1,2, 3, 4, 5, 22, 23, 45, 88]
```
在冒泡排序中会交换元素的位置。当源数据大多数排列是倒序时就会交换很多次元素位置。所以当源数据是基本顺序部分倒序时使用冒泡排序比较合适。
## 相关链接
* [返回目录](/README.md)