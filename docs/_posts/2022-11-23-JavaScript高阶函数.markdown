能够接收函数作为参数的函数，在js中被称为高阶函数

# map/reduce
`map()`方法定义在js的`Array`中，它能接收函数，并根据该函数对`Array`中的**每一个元素**依次进行处理，返回结果仍是以`Array`的形式
```javascript
'use strict';

function pow(x) {
    return x * x;
}

//将数组中的元素全部取平方，存入新的数组
var arr = [1, 2, 3, 4, 5, 6, 7, 8, 9];
var results = arr.map(pow); // [1, 4, 9, 16, 25, 36, 49, 64, 81] 直观明了

```

`reduce()`方法同样定义在js的`Array`中，它也能接收函数，但是必须传入两个参数，对`Array`中的**每两个元素**依次进行处理，返回结果视情况而定
```javascript
//把数组拼接变为整数
var arr = [1, 3, 5, 7, 9];
arr.reduce( function (x, y) {return x * 10 + y;} ); // 13579
```
注意：map**接收的是一个**参数，但传递**三个参数（数组元素，索引，数组本身）**，所以map中的函数要处理好这三个参数，否则会有bug

# filter
`filter()`方法同样定义在js的`Array`中，它也能接收函数。和map()不同的是，filter()把传入的函数依次作用于**每一个元素**，然后**根据返回值是true还是false决定保留还是丢弃该元素**
总的来讲，能起到**筛选过滤**的作用


# sort


# Array
