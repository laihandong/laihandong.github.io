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
      owned: null
  };//定义了一个对象car
  ```

  对象是一组由键-值组成的无序集合

  对象的`键都是字符串类型，值可以是任意数据类型`

  ```javascript
  car.color;//'red' 访问对象属性的方式
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

  






















































































> 2022.11.09算了，去学typescript了
> 2022.11.10 不行，JavaScript是基础，学web就得把这个打牢。typescript只是超集，以后再学，其不能替代JavaScript

