能够接收函数作为参数的函数，在js中被称为高阶函数

# map
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

# reduce
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
javascript的`sort()`函数默认先把所有元素转换为String，再根据ASCII码排序。但还好它也是一个高阶函数，**可以接收一个比较函数来实现自定义排序**

由于排序的核心是**比较两个元素**，所以js规定：对于两个元素x和y，如果认为x < y，则返回-1，如果认为x == y，则返回0，如果认为x > y，则返回1。

**从而编写自定义比较函数时，不需要关心排序的具体方法，只给出比较规则即可**，示例如下：
```javascript
var arr = [10, 20, 1, 2];
arr.sort(function (x, y) {
    if (x < y) {
        return 1;
    }
    if (x > y) {
        return -1;
    }
    return 0;
}); // [20, 10, 2, 1]

```
注意：**js使用sort()时，是直接在原数组上修改，而不是返回新的数组**


# every

every()方法可以判断数组的所有元素是否满足测试条件
```javascript
var arr = ['Apple', 'pear', 'orange'];
console.log(arr.every(function (s) {
    return s.length > 0;
})); // true, 因为每个元素都满足s.length>0
```
