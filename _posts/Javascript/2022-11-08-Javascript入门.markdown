---
layout: post
title:  "Javascript入门"
date:   2022-11-27 20:30:00 +0800
permalink: blog/编程语言/Javascript/Javascript入门.html
typora-root-url: ../../
author: handong
tag1: 编程语言
tag2: Javascript
---



[看廖雪峰大大的教程入门的](https://www.liaoxuefeng.com/wiki/1022910821149312)

# 1. JavaScript简介
首先，它和java基本是没有关系的。之所以名字会有java，是因为当年网景公司在那个java火遍全球的时代，推广他们自己的语言（1995），所以在名字里加上了java，也在一些语法上碰瓷java，仅此而已。

javaScript是一门动态解释语言，在web世界中，只有JavaScript能跨平台、跨浏览器驱动网页，与用户交互。（现在typescript基于JavaScript继续开发，是JavaScript的超集，推荐继续学习typescript）

# 2. 入门

## 2.1 hello,world!

JavaScript代码可以直接嵌在网页的任何地方，一种是把JavaScript代码放到<head>中，另一种是放在一个单独的`.js`文件，然后在HTML中引入这个文件，示例如下：

  ```html
<html>
<head>
  <!--方法一：-->
  <script>
    alert('Hello, world');
  </script>
    
  <!--方法二：-->
  <script src="/static/js/abc.js"></script>
</head>
<body>
  ...
</body>
</html>
  ```



## 2.2 基本语法

+ 赋值

  ```javascript
  var x = 1;
  ```

+ 注释

  ```javasript
  // 单行注释
  /*
  多行注释
  */
  ```

+ 语句的结束

  ```javascript
  ;表示一个语句结束，不建议一行多个语句
  ```

+ 语句块

  ```javascript
  if (x > 2) {
      //这里写语句，可以嵌套
  }
  ```

  

## 2.3 数据类型

### 2.3.1 number

+ 不区分整数和浮点数，统一用Number表示

  ```javascript
  123;
  0.4;
  NaN;//Not a number
  Infinity;//无限大
  ```

+ Number的四则运算

  ```javascript
  1+2;
  1/2;
  1-2;
  1*2;
  1%2;//求余 结果 1
  
  ```


### 2.3.2 字符串

+ 字符串

  ```javascript
  '';//单引号
  "";//双引号 中的任意文本
  
  "\'\'"; //'' 
  		//用转义字符串\，来标识需要特殊处理的字符
  
  '\x41'; //'A' 
  		//\x## 可以以十六进制表示一个ascii字符
  
  '\u4e2d\u6587'; //'中文'
  				//\u#### 可以表示一个unicode字符
  
  `
  行1
  行2
  `;//反引号内可添加多行字符串
  	
  
  ```

+ 模板字符串

  ```javascript
  var name = 'world！\n';
  var message = 'hello, ' + name;//用+拼接字符串
  
  var say_hi = 'hello';
  var message2 = `${say_hi}, ${name}`;//用${}插入替换字符串
  ```

  <font color=red>仅反引号支持ES6中的模板字符串</font>

+ 操作字符串

  ```javascript
  var s = 'hello,world!';
  
  s.length;//获取长度
  s[0];//访问字符串元素方式，只能访问，不能重新赋值
  s.toUpperCase();//全部变为大写
  s.toLowerCase();//反之
  s.indexOf('l');//搜索指定字符出现的位置，没有则返回-1
  s.substring(0,1);//返回指定区间的子串，左闭右开
  
  
  ```

### 2.3.3 布尔值

+ 布尔值

  ```javascript
  true;
  false;
  ```

+ 逻辑运算

  ```javascript
  &&;//与
  ||;//或
  !;//非
  ==;//比较 会自动转换数据类型再比较，有时会产生诡异的结果，建议弃用
  ===;//比较 不会自动转换数据类型，如果数据类型不一致，返回false，如果一只，再比较
  ```

  + 特殊说明：

    + `NaN`与所有值都不想等，包括它自己（`NaN === NaN; // false`）。唯一判断方法是`isNaN(NaN);//true`

    + 浮点数比较，只能计算它们之间的绝对值:

      ```javascript
      Math.abs(1 / 3 - (1 - 2 / 3)) < 0.0000001; // true
      1 / 3 === (1 - 2 / 3); // false
      ```



### 2.3.4 null 和 undefined

+ null 和 undefined

  `null`表示”空“（不可与`0`、`''`混淆，0是数值，''是长度为0的字符串）

  `undefined`表示值未定义，不常用

## 2.4 数组

+ 数组

  ```javascript
  [1, 2, 3.14, 'Hello', null, true];//一个数组，可包含任意数据类型
  new Array(1,2,3);//创建数组的方式 [1,2,3]
  var arr = [1,2,3];//推荐用这种方式
  var = [1,2];
  var[0];//1 访问数组元素的方式
  ```
  
  + 数组长度
  
    + 注意：可以重新赋值`arr.length`直接改变数组长度，如下
  
      ```javascript
      arr.length; // 3
      arr.length = 6;
      arr; // arr变为[1, 2, 3, undefined, undefined, undefined]
      arr.length = 2;
      arr; // arr变为[1, 2]
      ```
  
    + 注意：访问不存在的索引，会自动修改数组长度
  
      ```javascript
      arr[5] = 'x';
      arr; // arr变为[1, 2, 3, undefined, undefined, 'x']
      ```
  
    + <font color=red>以上情况最好不要出现在项目中，尽管不报错</font>
  
  + indexOf()
  
    ```javascript
    arr.indexOf(1);//元素1的索引
    arr.indexOf('1');//元素'1'的索引
    ```
  
  + slice()
  
    ```javascript
    arr.slice(0,3);//截取Array[0,1,2]，然后返回一个新的Array，左闭右开，可以不包括结束索引
    arr.slice(0);//截取从0开始到结尾
    arr.slice();//截取整个数组
    
    ```
  
  + push pop
  
    ```javascript
    arr.push('3');//末尾添加'3'
    arr.pop();//返回末尾元素（同时数组长度减一，即删除末尾元素）或者undefined
    ```
  
  + unshift shift
  
    ```javascript
    arr.unshift('3');//头部添加'3'
    arr.shift();//返回头部元素（同时数组长度减一，即删除头部元素）或者undefined
    ```
  
    <font color=red>注意：以上对空数组删除元素操作时，不会报错</font>
  
  + sort
  
    ```javascript
    arr.sort();//对数组元素（按默认规则）排序
    ```
  
  + reverse
  
    ```javascript
    arr.reverse();//对数组反转
    ```
  
  + splice
  
    ```javascript
    var arr = ['Microsoft', 'Apple', 'Yahoo', 'AOL', 'Excite', 'Oracle'];
    // 从索引2开始删除3个元素,然后再添加两个元素:
    arr.splice(2, 3, 'Google', 'Facebook'); // 返回删除的元素 ['Yahoo', 'AOL', 'Excite']
    arr; // ['Microsoft', 'Apple', 'Google', 'Facebook', 'Oracle']
    // 只删除,不添加:
    arr.splice(2, 2); // ['Google', 'Facebook']
    arr; // ['Microsoft', 'Apple', 'Oracle']
    // 只添加,不删除:
    arr.splice(2, 0, 'Google', 'Facebook'); // 返回[],因为没有删除任何元素
    arr; // ['Microsoft', 'Apple', 'Google', 'Facebook', 'Oracle']
    ```
  
    <font color=blue>修改数组的“万能方法”</font>
  
  + concat
  
    ```javascript
    var arr1 = [1,2];
    var arr2 = [3];
    var arr3 = arr1.concat(arr2);//[1,2,3] concat()方法并没有修改当前Array，而是返回了一个新的Array
    ```
  
  + join
  
    ```javascript
    var arr4 = ['1',1];
    arr.join('-');//'1-1' 把当前Array的每个元素都用指定的字符串连接起来，然后返回连接后的字符串，元素不是字符串，将自动转换为字符串后再连接
    ```
  
    <font color=red>数组支持嵌套</font>

## 2.5 对象

+ 对象：

  ```javascript
  var car = {
      color: 'red',
      size: '100',
      price: '1000',
      owned: null,
      'man-own':100
  };//定义了一个对象car
  ```
  
  对象是一组由键-值组成的无序集合
  
  对象的`键都是字符串类型，值可以是任意数据类型`，但是键中含特殊的字符需要用`''`括起来，同时访问的方式也有所区别。
  
  ```javascript
  car.color;//'red' 访问对象属性的方式一
  car['man-own'];//100 方位对象属性的方式二
  car.what;//undefined 访问不存在的对象不会报错 
  ```
  
  检测对象中是否含有某种属性，用`in`或者`hasOwnProperty()`
  
  ```javascript
  'name' in car; // false 
  car.hasOwnProperty('size'); // true 相比较下，前者还会往继承的基类中找，后者只判断自身拥有的
  ```
  
  可以在外部删除对象的属性，用`delete`
  
  ```javascript
  delete car.size; // 删除age属性
  ```
  
  

## 2.6 变量

+ 变量

  变量用变量名表示

  变量名命名规则：大小写英文、数字、`$`和`_`的组合，且不能用数字开头。变量名也不能是JavaScript的关键字。（可以是中文，但不建议）

  声明：用`var`

  ```javascript
  var a; // a此时为undefined
  var $b = 1; // $b的值为1
  var s_007 = '007'; // s_007是一个字符串'007'
  var Answer = true; // Answer是一个布尔值true
  var t = null; // t的值是null
  ```

  用`=`赋值变量，但变量只能用var申明一次

  JavaScript是动态语言，同一变量可以被重新赋不同数据类型的值

  <font color=red>历史遗留问题：strict模式</font>

  ```txt
  JavaScript在设计之初，为了方便初学者学习，并不强制要求用var申明变量;
  如果一个变量没有通过var申明就被使用，那么该变量就自动被申明为全局变量，很容易带来严重的问题；
  ECMA在后续规范中推出了strict模式，在strict模式下运行的JavaScript代码，强制通过var申明变量，未使用var申明变量就使用的，将导致运行错误；
  启用strict模式的方法是在JavaScript代码的第一行写上：'use strict'
  ```

  

## 2.7 条件判断

JavaScript使用`if () { ... } else if{ ... } else {...}`来进行条件判断，可嵌套，`else if 和 else`不是必须的

JavaScript把`null`、`undefined`、`0`、`NaN`和空字符串`''`视为`false`，其他值一概视为`true` 



## 2.8 循环



 一种是`for`循环，通过初始条件、结束条件和递增条件来循环执行语句块：

```javascript
var x = 0;
var i;
for (i=1; i<=10000; i++) {
    x = x + i;
}
```

另一种是`for 和 in 组合`，循环读取`对象`的**属性**、`数组`的**索引**（*注意*，`for ... in`对`Array`的循环得到的是`String`而不是`Number`）

```javascript
var obj = {
    name: 'Jack',
    age: 20,
    city: 'Beijing'
};
for (var key in obj) {
    console.log(key); // 'name', 'age', 'city'
}

var a = ['A', 'B', 'C'];
for (var i in a) {
    console.log(i); // '0', '1', '2' 是string类型  
    console.log(a[i]); // 'A', 'B', 'C'
}

```

还有一种是`while`，以及变体`do {...} while`



## 2.9 Map

Map 属于 ES6新增的数据类型：键值对的集合

```javascript
var m1 = new Map();//空Map
var m2 = new Map( ['name',1], ['size',99], ['price',10]);//初始化Map同时赋值

m1.set('name',3);//添加新的键-值对，添加已存在的键-值对，会将之前的覆盖掉

m1.has('name');//是否存在键：'name'

m1.get('name');//获取键'name'对应的值

m1.delete('name');//删除键'name'

```



## 2.10 Set

Set 属于 ES6新增的数据类型：键的集合，键不能重复

```java
var s1 = Set();//空Set
var s2 = Set([1,2,3]);//初始化Set同时赋值，只能用列表

s1.add(4);//给Set添加元素，若已存在则没有影响

s1.delete(4);//删除元素
```





## 2.11 iterable

`Array`、`Map`和`Set`都属于`iterable`类型，为了统一实现循环遍历

对`iterable`类统一采用`for ... of`循环遍历集合

```javascript
var a = ['A', 'B', 'C'];
var s = new Set(['A', 'B', 'C']);
var m = new Map([[1, 'x'], [2, 'y'], [3, 'z']]);
for (var x of a) { // 遍历Array
    console.log(x);
}
for (var x of s) { // 遍历Set
    console.log(x);
}
for (var x of m) { // 遍历Map
    console.log(x[0] + '=' + x[1]);
}
```

<font color=red>注意，`for ... in`是存在遗留问题的，所以ES6中提出了`for ... of`</font>

一个`Array`数组实际上也是一个对象，它的每个元素的索引被视为一个属性，在手动给Array添加了额外的属性后，使用`for ... in`会遍历所有的属性，不只是索引，示例如下：

```javascript
var a = ['A', 'B', 'C'];
a.name = 'Hello';
for (var x of a) {
    console.log(x); // 'A', 'B', 'C'
}
```

<font color=blue>ES5.1中提出了直接使用`iterable`内置`forEach`方法，它接收一个函数，每次迭代就自动回调该函数</font>

```javascript
a.forEach(function (element, index, array) {
    // element: 指向当前元素的值
    // index: 指向当前索引
    // array: 指向Array对象本身
    console.log(element + ', index = ' + index);
});

var s = new Set(['A', 'B', 'C']);
s.forEach(function (element, sameElement, set) {
    console.log(element);//Set没有索引，所以前两个参数返回的值是一样的
});

var m = new Map([[1, 'x'], [2, 'y'], [3, 'z']]);
m.forEach(function (value, key, map) {
    console.log(value);
});
```











# 3. 函数
## 3.1 定义第一个函数

javascript函数可以像变量一样，有很强的抽象能力。
定义方式之一：

```javascript
function abs(x) {
  if ( x >= 0) {
    return x;
  } else {
    return -x;
  }
}
```
定义方式之二：<font color=red>即，将函数视为变量，去定义这个函数</font>
```javascript
var abs = function (x) {
  if ( x >= 0) {
    return x;
  } else {
    return -x;
  }
};
```
+ return 并不是必须的，JavaScript会默认返回undefined
+ 方式二中，`function (x) { ... }`就是一个匿名函数，不过要注意**在结尾加上分号**

## 3.2 调用这个函数吧

+ JavaScript允许传入任意个参数（即使函数没有定义）
  + **多了**不影响返回值，不影响调用

  + **少了**则返回NaN，因为会把当`undefined`传入，结果为`NaN`
    
    + 解决方法之一是==加一个传入值的类型判断==
    
    	```javascript
        if ( typeof x !== 'number') {
          throw 'Not a number';
        }
    	```
    
    + 另一个方法就是使用`arguments`
  
+ arguments：
  + ==只在函数内部==起作用的关键字
  
  + 永远指向当前函数的调用者传入的==所有参数==
  
  + 像array，但不是array
  
    ```javascript
    function abs() {
        if (arguments.length === 0) {
            return 0;
        }
        var x = arguments[0];
        return x >= 0 ? x : -x;
    }
    ```
  
  + 若想让函数中有可选参数，也是通过`arguments`
  
    ```javascript
    function foo(a, b, c) {
        if (arguments.length === 2) {
            // 实际拿到的参数是a和b，c为undefined，以下操作把b处理成了默认参数，实现类似function(a[,b],c)的效果  
            c = b; // 把b赋给c
            b = null; // b变为默认值
        }
    }
    ```
  
  + 获取==额外的参数==
  
    ```javascript
    function foo(a, b) {
        var i, rest = [];
        if (arguments.length > 2) {
            for (i = 2; i<arguments.length; i++) {
                rest.push(arguments[i]);//获取额外的参数
            }
        }
    }
    ```
  
    但会显得有些别扭，ES6中引入了==`rest`==参数
  
    ```javascript
    function foo(a, b, ...rest) {
        console.log(rest);//这就是额外的参数（没有的话是个空数组，不是undefined）
    }
    ```
  
  + 注意==return==
  
    JavaScript引擎在行末自动添加分号的机制，使得return最好不要和返回值拆成两行写，如果要的话，记得用上`{}`
  
    ```javascript
    function foo() {
        return { // 这里不会自动加分号，因为{表示语句尚未结束
            name: 'foo'
        };
    }
    ```
  
    



## 3.3 变量作用域

+ 和大多数语言一样，变量只在声明它的函数体内有效

+ 嵌套函数中有同名变量，内部函数的变量会“屏蔽”外部函数的变量

+ JavaScript特性：==变量的声明“提升”但赋值不会==

  ```javascript
  function foo() {
      var x = 'Hello, ' + y;//这里不会报错，只不过此时y是undefined（所以建议养成好习惯，在函数头部尽量初始化好所有变量）
      console.log(x);
      var y = 'Bob';
  }
  ```

+ 不在任何函数体内定义的变量，会被默认绑定到全局对象`window`，作用域为==全局作用域==

  ```javascript
  var x = 'hello';
  alert(course); // 'hello'
  alert(window.course); // 'hello'
  ```

  同理可推，**在将函数视为变量的JavaScript中**，会有以下情况发生：

  + *顶层函数的定义也被视为全局变量*，可以用window访问，自定义的（甚至是alert）均是如此

  + 类似`window.alert`可以赋值给别的变量，也可以被赋值

    ```javascript
    window.alert('调用window.alert()');  //这里还可以正常调用
    
    var old_alert = window.alert;  //old_alert可以实现alert的功能了，old_alert("hello world")
    
    window.alert = function () {}  
    alert('无法用alert()显示了!');	//到这里，alert就用不了了（但不会报错，因为已经是个空函数了），这句代码不会有任何消息产生
    
    // 恢复alert:
    window.alert = old_alert;
    alert('又可以用alert()了!');	//这里就恢复使用了，打印出'又可以用alert()了!'
    ```

+ 从以上可以发现，==JavaScript只有一个全局作用域==，任何变量都会在当前作用域查找（若没有则往上，直至全局作用域），否则报`ReferenceError`错误

+ 在**多个js文件**共同工作时，**很容易发生全局作用域中变量命名重复的情况**

  + 解决方法之一是==命名空间==

    ```javascript
    // 唯一的全局变量MYAPP:
    var MYAPP = {};
    
    // 其他变量:
    MYAPP.name = 'myapp';
    MYAPP.version = 1.0;
    
    // 其他函数:
    MYAPP.foo = function () {
        return 'foo';
    };
    ```

    事实证明，这是有效的。比如jQuery、YUI等等js库

+ 局部作用域事实是以函数体为最小单位，从而引出了==块级作用域==，并引入`let`来区分`var`定义变量的作用

  ```javascript
  function foo1() {
      for (var i=0; i<100; i++) {
          //
      }
      i += 100; // 仍然可以引用变量i
  }
  function foo2() {
      var sum = 0;
      for (let i=0; i<100; i++) {
          sum += i;
      }
      i += 1;// 这行代码会报错SyntaxError
  }
  ```

+ 有变量，自然就有==常量==

  + ES6之前，一般全部大写约定这是常量

    ```javascript
    const PI = 3.14;//（“不要修改它！”）
    ```

  + ES6开始，引入`const`来定义常量，且也具有**块级定义域**（*上面的约定俗成也是好习惯*）





## 3.4 解构赋值
ES6引入了解构赋值，能实现同时对一组变量进行赋值，具体应用举例如下：

```javascript
var [x,y,z] = ['hello', 'world', '!'] // 注意：对数组元素进行解构赋值时，多个变量要用[]括起来

let [x,[y,z]] = ['hello', ['world', '!']] // 注意：若存在嵌套，嵌套层次和位置要保持一致

let [, , z] = ['hello', 'world', '!'] // 注意：这样做可以忽略前两个元素，直接赋值第三个元素给z

```

除了数组元素，还能应用于`对象`
```javascript
var person = {
  name : 'lhd',
  age : 21,
  gender : 'male'
};

var {name,gender} = person; // 注意：使用属性名来获取相应对象属性

var student = {
  name : 'xiaoming',
  age : 18
  class : {
    level : 6,
    id : 3
  }
};

var {name, class:{level, num}} = student; //注意：有多级嵌套的对象，保证对应层次一致即可；
                                          //注意：不存在的属性，比如num，会赋值为undefinedclass
                                          //注意：class不是变量，而是为了让city和num获得嵌套的class对象的属性；
                                          //注意：此时访问class，会提示Uncaught ReferenceError: class is not defined
                                          
var {name: myName, age: myAge} = student; //注意：如果要使用的变量名和属性名不一致，可以这样获取
                                          //注意：name，age此时不是变量，而是让变量myName，myAge获得name，age属性；
                                          //注意：此时访问name或age，会提示Uncaught ReferenceError: class is not defined
                                          
var {name, gender='male'} = student; //注意：避免获取不存在的属性而导致返回undefined，可以采取默认赋值


```

如果变量已经被声明了，再次赋值的时候，正确的写法也会报语法错误：
```javascript
var x, y;

{x, y} = { name: '小明', x: 100, y: 200};
// 语法错误: Uncaught SyntaxError: Unexpected token =

//此时应该改为用小括号将整个句子括起来（原因是js引擎将{开头的语句当作了块处理，使用=变为不合法）
({x, y} = { name: '小明', x: 100, y: 200});
```


解构赋值的场景很丰富，举例如下
```javascript
//场景一：交换
var x1=1, x2=2;
[x1, x2] = [x2, x1];

//场景二：快速获取页面属性（比如域名和路径）
var {hostname:domain, pathname:path} = location;


//场景三：函数传参
function test( {year, month, day, hour=0, minute=0, second=0} ) {
  return {year+'/'+month+'/'+day+' '+hour+':'+minute+':'+second};
}//好像和普通传参没多大区别吧

```


## 函数之方法

在一个对象中绑定函数，称为这个对象的方法，和很多语言中的“方法”类似

### this关键字
js中有个关键字this（没错，跟c/c++中的作用类似），常用在方法中，访问对象的本身的属性用，但却有些设计上的缺陷

+ 在**方法**中调用this，自然指向当前的对象
+ 当**函数**中调用了this，js是不会报错的，而是**视情况而定**this该指向谁（大坑）
  ```javascript
  function getAge() {
    var y = new Date().getFullYear();
    return y - this.birth;
  }
  
  var xiaoming = {
      name: '小明',
      birth: 1990,
      age: getAge
  };
  ```
  + 以*对象*形式调用*方法*，this指向当前对象
    ```javascript
    xiaoming.age(); //返回一个正常数字
    ```
  + 单独调用*函数*，this指向**全局**对象（在js中也就是window）
    ```javascript
    getAge(); // NaN
    ```
  + 声明变量获取对象的方法，然后直接使用，this会指向`undefinded`
    ```javascript
    var fn = xiaoming.age; // 先拿到xiaoming的age函数方法
                           // 注意：ES6在strict模式下，这句会报错Uncaught TypeError: Cannot read property 'birth' of undefined
                           // 但没有解决this指向正确位置的问题
    fn(); // NaN
          
    ```

+ 在对象的方法内定义的函数，this又会指向`undefined`（无语=.=）
  ```javascript
  'use strict';
  
  var xiaoming = {
      name: '小明',
      birth: 1990,
      age: function () {// 在这一层，this会指向xiaoming，即当前对象
          function getAgeFromBirth() {// 注意：在这一层，this又指向了`undefined`;且在非strict模式下，它重新指向全局对象`window`
              var y = new Date().getFullYear();
              return y - this.birth;
          }
          return getAgeFromBirth();
      }
  };
  
  xiaoming.age(); // Uncaught TypeError: Cannot read property 'birth' of undefined
  
  //解决方法：在方法内部一开始就捕获this
  var xiaoming = {
    name: '小明',
    birth: 1990,
    age: function () {
        var that = this; // 在方法内部一开始就捕获this
        function getAgeFromBirth() {
            var y = new Date().getFullYear();
            return y - that.birth; // 用that而不是this
        }
        return getAgeFromBirth();
    }
  };
  
  xiaoming.age(); // 25
  ```
### apply() call()
这两个函数属于函数本身的方法，可以起到改变this指向的作用
+ apply()
  + 第一个参数是需要绑定的this变量
  + 第二个参数是Array，表示函数本身的参数

+ call()
  + 和apply几乎相同，唯一不同如下：
    ```javascript
    Math.max.apply(null, [3, 5, 4]); // 5
    Math.max.call(null, 3, 5, 4); // 5 call()把参数按顺序传入
    ```
    对于需要对this进行特殊处理的函数，使用applay或call会很有帮助，普通函数使用的话，通常让this绑定为null

## 装饰器

**JavaScript的所有对象都是动态的，即使内置的函数，我们也可以重新指向新的函数**
尝试理解下面的例子：
```javascript
'use strict';

var count = 0;
var oldParseInt = parseInt; // 保存原函数

window.parseInt = function () {
    count += 1;
    return oldParseInt.apply(null, arguments); // 调用原函数，apply视情况使用（主要为了避免this指向位置的问题）
};
```


形象地说，parseInt函数被“夺舍”后，先执行我们给它的语句count+=1，再执行它自己，并返回结果




























> 2022.11.09算了，去学typescript了
> 2022.11.10 不行，JavaScript是基础，学web就得把这个打牢。typescript只是超集，以后再学，其不能替代JavaScript

