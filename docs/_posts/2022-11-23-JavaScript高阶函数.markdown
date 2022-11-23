能够接收函数作为参数的函数，在js中被称为高阶函数

# map/reduce
`map()`方法定义在js的`Array`中，它能接收函数，并根据该函数对`Array`中的元素进行处理，返回结果仍是以`Array`的形式
```javascript
'use strict';

function pow(x) {
    return x * x;
}

//将数组中的元素全部取平方，存入新的数组
var arr = [1, 2, 3, 4, 5, 6, 7, 8, 9];
var results = arr.map(pow); // [1, 4, 9, 16, 25, 36, 49, 64, 81] 直观明了
```

`reduce()`方法同样定义在js的`Array`中，
# filter


# sort


# Array
