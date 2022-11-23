高阶函数除了可以接受函数作为参数外，**还可以把函数作为结果值返回**：
**内部函数**可以引用**外部函数**的参数和局部变量，当返回函数时，**相关参数和变量都保存在返回的函数中**，这种称为**闭包（Closure）**

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
注意：`f1()`和`f2()`的调用互不影响，即时传入的参数相同
