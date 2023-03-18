---
layout: single
title:  "Javascript闭包"
author: laihandong
tags: 编程语言 Javascript
---



高阶函数除了可以接受函数作为参数外，**还可以把函数作为结果值返回**：
**内部函数**可以引用**外部函数**的参数和局部变量，当返回函数时，**相关参数和变量都保存在返回的函数中**，这种称为**闭包（Closure）**


# 函数作为返回值
```javascript
function lazy_sum(arr) {
    var sum = function () {
        return arr.reduce(function (x, y) {
            return x + y;
        });
    }
    return sum;
}

var f1 = lazy_sum([1, 2, 3, 4, 5]);
var f2 = lazy_sum([1, 2, 3, 4, 5]);
f1 === f2; // false
f1(); // 15
```
注意：`f1()`和`f2()`的调用互不影响，即时传入的参数相同，因为每次调用`lazy_sum`返回的都是新的函数

# 闭包
注意以下几点，把握住闭包：
+ **返回的函数**在其定义内部会**引用父函数传入的局部变量**，所以，其内部的局部变量还会被**新函数**引用。并且，返回的函数**不会立即执行，直到被调用**了才执行。这就会产生一个问题：

  ```javascript
  function count() {
      var arr = [];
      for (var i=1; i<=3; i++) {
          arr.push(function () {
              return i * i;
          });
      }
      return arr;
  }
  
  var results = count();
  var f1 = results[0];
  var f2 = results[1];
  var f3 = results[2];
  
  f1(); // 16
  f2(); // 16
  f3(); // 16
  ```

  <font color = blue>返回的函数引用了变量`i`，但它并非立刻执行。等到3个函数都返回时，它们所引用的变量`i`已经变成了`4`，因此最终结果为`16`。</font>

+ 

