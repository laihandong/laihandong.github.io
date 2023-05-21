# **1 简介**

JavaScript是一门高级、动态、解释型编程语言
JavaScript是Netscape在web诞生初期创造的，后来移交给Ecma International进行标准化，更名为ECMAScript（由于很拗口，大家仍称其为JavaScript），不过在正式场合一般仍使用该名称（缩写为ES）
JavaScript早期的宿主环境是浏览器，后来node赋予了JavaScript访问整个操作系统的权限

# **2 词法结构**

类的定义：
```javascript
class MyClass {
    constructor(x,y){  //构造函数，不需要return
        this.x = x;
        this.y = y;
    }
    myFunc(){
        return this.x + this.y;
    }
}

let c = MyClass(1,2);

c.myFunc();  //=> 3
```
ES6之后，定义函数的简介方法：
```javascript
const fun1 = x => x+1;
const fun2 = x => x**2;

fun1(3);  //=> 4
fun2(4);  //=> 16
```



JavaScript除了常规空格`\u0020`，其将制表符、各种ASCII控制符和Unicode间隔识别为**空格**


JavaScript使用Unicode字符集编写，天然支持unicode
```javascript
let é = 1;//   法语字母 é 作为变量名
\u00e9    //=> 1
\u{E9}    //=> 1 等同于\u00e9
\u{1F600} //=> 1 打印一个笑脸符号， \u{} 是ES6推出的，为了兼容16位以上的unicode
```
关于unicode，一个相同的字符可以对应多种unicode编码：
法语字母 é 既可以编码为`\u00e9`，也可以编码为一个常规ASCII字符`e`再加上一个重音组合标记`\u0301`
但是**不同的编码方式对应的二进制却是不同的**，所以在用任何文本编辑器或相关工具处理自己的JavaScript源代码时，务必注意执行Unicode归一化（Unicode标准为所有字符规定了首选编码和归一化例程）

JavaScript的分号`;`不像`C C++`那样严格，但也要避免由缺失分号带来歧义的情况（见《JavaScript权威指南 李松峰译 原书第七版》p22）

# **3 类型、值和变量**

--《JavaScript权威指南--李松峰译--书第七版》



JavaScript的类型可以分为**原始类型和对象类型**：
原始类型：数值、字符串、布尔值、unll、undefined、symbol
对象类型：任何不是原始类型的值
原始类型的是不可变的immutable，对象类型是可变的mutable


数值：
全称数值字面量`number literal`，分整数字面量、浮点字面量
```javascript
123              //=> 123
0xff             //=> 255 (15*(16**1) + 16*(16**0))
0o377            //=> 255 (3*(8**2) + 7*(8**1) + 7*(8**0))
0b10101          //=>21 (1*(2**4) + 0 + 1*(2**2) + 0 + 1*(2**0))

3.14             //=> 3.14
.33              //=> 0.33
6.02e23          //=> 6.02 x 10^23
1.43E-12         //=> 1.43 x 10^(-12)

//分隔符的使用，将字面量分隔以容易看清
let number = 1_000_000_000
let bytes = 0x3a_38_9f
let bits = 0b0001_1101_0111
let fraction = 0.123_345_353   // 每隔三位分隔一次只是约定俗成

NaN              //=> Not a number 非数值
Infinity         //=> 无穷值 （上溢出）
-0               //=> 负零值 比最小可表示数值 更为 接近0 （下溢出）
0                //=> 常规的0
/*
 * 负零值和正零值（严格）相等，即 0 === -0 为 true，除了作为除数使用，几乎无法区分这两个值
 * NaN与任何值比较都不相等，包括它自己
 * 不能通过 x === NaN 的方式判断是否为NaN
 * Number.isNaN() 可以用于判断数值是否为NaN，也适用于参数为NaN（返回True）
 */

let x = .3 - .2;
let y = .3 - .2;
x === y;         //=> false JavaScript中也存在这种问题，所以尽量避免去比较类似的情况

1234n            
0b111111n
0o723n           //   在字面量后跟小写字母n，表示BigInt字面量(ES2020新增)
1000n / 330n     //=> 3n 只有 / 运算会舍弃余数并向下取商，其它运算和常规JavaScript类似
1000n + 123      //   JavaScript不允许BigInt操作数和常规JavaScript操作数混用，
0 === 0n         //=> false 但比较远运算符例外
```

字符串：
```javascript
/*
 * 字符串是 16位值的 不可修改的 有序序列
 * 字符串的length属性是它包含16位值的个数
 * 有些超出16位范围的Unicode字符（surrogate pair “代理对”），就需要长度为2的JavaScript字符串来表示了
 * 以此类推，JavaScript字符串的操作方法，一般操作的都是16位值，而不是字符
 */

`abc`
"abc"
'abc'        //   JavaScript中单引号、双引号和反引号(ES6新增)，都可以用来包含字符串字面量

             //   JavaScript将换行符、回车和回车 `/`换行序列识别为行终止符

let name = "handong";
let s = `hi! ${name}`;
/*
 * 模板字面量 
 * ${表达式} 
 /
String.raw`\n`.length  //=> 2 非常特殊的情况下，反引号`可以充当函数括号

/*
 * 一对斜杠之间的文本构成正则表达式字面量（RegExp）
 * 这对斜杠的第二个后面还可以跟一个或多个字母，用于修改模式的含义
 */
/^HTML/;      //   匹配字符串开头的字母HTML
```

布尔值：
```javascript
/*
 * JavaScript中的任何值都可以转换为布尔值，或者当成布尔值使用，称为假性值falsy、真性值truthy
 * false: undefined null 0 -0 NaN 空字符串
 * true: 除了false的所有其他值
 * 唯一的API: toString() 将自己转换为字符串"true"或"false"
 */


```

null和undefined:
```javascript
null         // 语言关键字，可以看成该特殊类型的唯一成员，类似的由python的None
undefined    // 预定义的全局常量，表示更深层次的不存在，也视为该特殊类型的唯一成员
             // 它们没有API可调用

> typeof undefined
'undefined'
> typeof null
'object'
```

符号：
```javascript
/*
 * 符号 是ES6新增的 原始类型
 * 符号 没有字面量，只能通过 Symbol()函数 创建，其接受一个 字符串参数，返回一个唯一的符号值
 * 实践中，通常作为一种语言拓展机制，之后再了解
 */
```

对象：
```javascript
/*
 * 全局对象：
 * 全局对象的属性是全局性定义的标识符，可以在JavaScript程序的任何地方使用
 * JavaScript解释器启动后（或每次浏览器加载页面），都会创建一个新的全局对象并初始化一组属性
 * ES2020最终定义了globalThis来作为引用全局对象的标准方式
 * 
 * 对象有时候被称为引用类型（reference type），以区别于JavaScript的原始类型
 * 对象值就是引用，对象的比较就是按引用比较的，引用同一个底层对象才视为相等
 */



```

类型转换：
```javascript
/*
 * JavaScript会根据需要把你提供的值转为布尔值，但有时也会转换为字符串、数字或NaN
 * | 你提供的值 | 转换为字符串 | 转换为数值 | 转换为布尔值 |
 * 更多内容见《JavaScript权威指南 李松峰译 原书第七版》p44
 * 
 * == 会在比较之前按需进行类型转换，但绝不会将操作数转换为布尔值 
 * 可以用函数显示转换，比如Boolean() Number() String() 等等
 * 
 * 对象转换为原始类型：p49（比较繁琐和细节） 
 */
```

变量的声明与赋值：
```javascript
/*
 * JavaScript的标识符的命名：首字母必须以字母、`_`或`$`开头，后续字符可以是字母、数字、下划线或美元符号
 * 常量用`const`声明，变量用`let`声明
 * const 必须在声明时初始化常量，前后续不可更改（否则引起TypeError）
 * 
 * 解构赋值：let [x,y] = [1,2];  //=> x = 1, y = 2 
 *          两侧的变量和值数量不等时，要么被忽略，要么被设置为undefined
 *          左侧的变量列表中，可以包含额外的逗号，以跳过右侧的某些值
 *          把右侧未使用或剩余的收集，就在左侧的最后一变量名前加上三个点...
 *          支持嵌套解构
 *          右侧值的类型数组、字符串、对象都适用
 *          当右侧为对象时，左侧的变量名则会自动和对象的属性名对应（否则被设置为undefined）。当然，这样就局限了变量的命名，所以可以在属性名后加上冒号，填写自定义的变量名
 *          
 */
 const {cos: myCos, sin: mySin} = Math; //=>const myCos = Math.cos, mySin = Math.sin;
```

# **4 表达式与操作符**

--《JavaScript权威指南--李松峰译--书第七版》

主表达式（primary expression）：独立存在，不再包含更简单的表达式的表达式
```javascript
1.23
"abc"
/^some/      // 字面量

true
false
null
this         // 保留字

i
sum
undefined    // 变量、常量、全局对象属性的引用
```

对象和数组 初始化程序 也属于表达式，但不是主表达式：
```javascript
[]
[1,3]
[[1,2],[3,,3]]

let a = {x:1,y:3}
let b = {}

// 这些初始化程序，有时也称 数组字面量、对象字面量


```

函数定义 表达式：
```javascript
let f = function(x){return x*x;};    // 关键字function 参数列表 函数体 （匿名函数例外）
```

属性访问 表达式：
```javascript
/* 
 * expression . identifier
 * expression [ identifier ]
 * 句点 中括号 前面的表达式都会先求值，若为null | undefined，则抛出TypeError
 */
x.pos
arr[0]
dic["name"]    

/*
 * ES2020 新增两种属性访问表达式
 * expression ?.  identifier
 * expression ?.[ identifier ]
 * 这两种表达式，当前面的表达式为null | undefined 时，返回undefined
 */
```

调用表达式：
```javascript
/* 
 * expression([args,,,])    中括号部分为可选内容
 * 括号前面的表达式都会先求值，若不为函数（包括null undefined的情况），则抛出TypeError
 * 要么为函数体return的值，要么为undefined
 */
f(3)
Math.max(5,9,10);

/*
 * ES2020 新增条件式调用表达式
 * expression ?.()
 * 仅当 前面的表达式为null | undefined 时，不执行函数，返回undefined
 */

```

对象创建 表达式：
```javascript
new Object()
new Pos(3,3)    // 自动执行对象的构造函数来初始化
```

操作符：
具体表格见《JavaScript权威指南--李松峰译--书第七版》p66
左值：一个可以合法地出现在赋值表达式左侧的表达式
JavaScript中，变量、对象属性和数组元素都是“左值”

算术表达式：
```javascript
//《JavaScript权威指南--李松峰译--书第七版》p70
// 相关细节很多，建议看书
```

关系表达式：
```javascript
/* 严格相等 ===
 * 两个值类型不同                               => false
 * 两个值都为null，或都为undefined              => true
 * 两个值中有NaN(1或2个)                        => false
 * 两个值都是数值且值相等                       => true
 * 0 = -0                                      => true
 * 两个值都是字符串且相同位置的16位值完全相同    => true
 * 两个值都是引用的同一对象、数组或函数          => true
 * 
 * 不满足的严格相等的， !== 就为true
 */


/*
 * 基于类型转换的相等 ==
 * 如果两个值的 类型不同，它会尝试做类型转换
 * 如果两个值的 类型相同（本来就相同 或 转换后相同），它会按照严格相等来进行比较
 * 类型转换细则(下面的?表示代指，无意义)：
 *     特殊：null == undefined   => true
 * 
 *     ?  == 字符串   -->  ? == 数值
 *     ?  == true     -->  ? == 1
 *     ?  == false    -->  ? == 0
 *     ?  == 对象     -->  ? == 原始值
 * 
 * 关于对象如何转换为原始值：
 *     JavaScript会尝试使用对象内的toString()方法，或者使用valueOf()方法
 *     通常一般的对象都会优先使用valueOf()，但Date类是例外，它优先执行toString()
 *     前者失败（或不存在该方法）后，再考虑toString() | valueOf()
 * 
 * 不满足基于类型转换的相等的， != 就为true
 */

/*
 * 比较操作符
 * 只能比较数值和字符串（若不是则会进行转换，再比较
 * 
 * JavaScript字符串是16位整数值的序列，而字符串比较是比较两个字符串的数值序列
 * Unicode定义的数值编码顺序会受不同地区和特定语言使用的传统 校正顺序（collation order）匹配
 * 不同地区比如不同国家
 * 特定语言，比如ASCII，它的 大写字母的数值 排在 小写字母前（这样的话 Z > a 反而成立）
 * 可以考虑String.localeCompare()
 * 
 * = 和 <= 不依赖于之前的等于比较符，仅代表 > 或 < 相反情况的
 */


/*
 * in 操作符
 * in期待左侧是可以转换成字符串的任何值，右侧是对象
 */
let data = [1,3]
"0" in data        //=> true

let pos = {x:1,y:2}
x in pos           //=> true


/*
 * instanceof 操作符
 * 左侧操作数为对象，右侧操作数是对象类的标识
 * 当左侧的对象 是 右侧类 的 实例 时，返回true，否则为false
 * 
 */
 let d = Date()
 d instanceof Date    //=> true
```

逻辑表达式：
```javascript
/*
 * &&
 * 总是返回真值或假值（注意，这个范围很大）
 * 它不要求左右两侧操作数为布尔值（比如真/假性值），也并不总是返回true或false
 * 
 * 当左侧操作数为假值，右侧操作数不会被求值，返回左侧的操作数的值
 * 当左侧操作数为真值，返回右侧操作数
 * 
 * || 同上（相反）
 */
 "a" && "b"    //=> "b"
 "a" && 0      //=> 0
 
/*
 * ! 逻辑非
 * 总是返回布尔值：true或false
 */
```

赋值表达式：
```javascript
a = 1         // 普遍情况
a = b = c = 1 // 连续赋值的简写

/*
 * 拼接操作符
 * += -= **= 等等
 * 唯一不同的是，拼接后，左操作数只会被求值一次，而原来会被求值两次
 * 这可能会带来负效应
 */ 

a[i++] += 1        
a[i++]  = a[i++] + 1 
```

eval操作符（也是函数）：
相关细节见p84

其它操作符：
```javascript
/* 
 * 条件操作符： 
 * ?: 
 * expr1 ? expr2 : expr3
 * expr1 为true时，返回expr3，否则返回expr3
 */ 

let s = "hi," + (name ? name : "there")

/* 
 * 先定义（first-defined）操作符
 * ES2020新增，正式名为缺值合并（nullish coalescing）操作符
 * ??
 * expr1 ?? expr2
 * 只有在expr1 为null或undefined时 才会返回第二个操作数
 */ 

/* 
 * typeof 操作符，总是返回如下值
 *     "undefined" 
 *     "object"
 *     "boolean"
 *     "number"
 *     "bigint"
 *     "string"
 *     "symbol"
 *     "function"
 */ 
typeof null    //=> "object": 所以要区分对象和null，需要显式测试


/*
 * delete 操作符
 * 删除对象属性（包括数组元素），将属性设置为undefined的同时，该属性也不存在了
 */
 let a=[1,3,3]
 delete a[2]    // 删除数组最后一个元素
 a.length       //=> 3:长度不变


/*
 * await 操作符
 * ES2017新增，期待一个Promise对象作为唯一操作数
 * 只能出现在已经通过async关键字声明为异步的函数中
 */


/*
 * void 操作符
 * 它可以放在任意类型的操作数前面，求值这个操作数，返回undefined
 */
 let i = 0;
 const f = () => void i++;
 f();    //=> undefined
 i       //=> 1


/*
 * , 逗号操作符
 * 求值左侧操作数然后丢弃，再求值右侧操作数并返回
 */
```

# **5 语句**

JavaScript程序就是一系列语句，以分号分隔

表达式语句：有副效应的表达式
```JavaScript
i++;      // 赋值表达式

myFun();  // 调用表达式
```

复合语句和空语句
```javascript
i++,j++,c++;    // 复合语句

{
  i++;
  j++;
  c++;          // 语句块，也是复合语句
}

;               // 空语句
```

分支语句：
```javascript
if(expr) 
	statement;    // expr为真值，则执行statement；反之
// 可选
// else if (expr2) {statement1;statement2;}
// else {statement3;statement4;}

switch(expr){
	statement;
	// 可选
	// case expr1:
	//     statement2;
	//     break;
	// default:
	//     statement3;
	//     break;
}                 // 其中case的使用，是以===来判断相等的，这个例子里是比较expr===expr1
```

循环语句：
```javascript
while(expr){
	statement;
}



do
	statement;
while(expr)



for(init; test; increment)
	statement;


// ES6新增 for/of 专门用于可迭代对象（与for关键字完全不同）
let myArr = [1,3,4,5,];
for(let element of myArr)
	statement;                  // 数组属于可迭代对象

let myStr = "abcdefg";          // 字符串是按 码点 迭代的，而不是16位值

let myObj = {x:1,y:2,z:3};      // 对象默认不可迭代
							    // 需要用Object.keys()、Object.values()或Object.entries()

let mySet = new Set(["1","2","3"]);

let myMap = new Map([[1,"one"],[2,"two"]]);


// ES2018新增 异步迭代器 和 异步迭代器的for/await循环
async function myFun(str){
	for await(let s in str){
		console.log(s);
	}
}


// for/in循环 适用于任意对象，循环指定对象的属性名
for(variable in object)
	statement;
/*
 * javascript首先对object求值，若为null 或 undefined则会跳过循环，执行下文语句
 * 但并不会枚举对象中的所有属性，比如符号类的不可枚举等等
 */
```

跳转语句：
```javascript
/*
 * 常见的continue break return throw yield 
 * 它们都具有跳转功能，让JavaScript解释器跳转到新的代码位置
 */

// 语句标签，给语句起个名字，可以在程序的其它地方通过这个名字引用它
// 标签 和 变量与函数 不在同一个命名空间
identifier: statement;


// break 只在循环和switch中使用，达到跳出的作用，
// break 后可以接语句标签，达到跳出后，跳转到该语句标签的作用


// return 只在函数体中使用，返回的值作为函数的值，若没有则为undefined


// yield 之用在ES6新增的生成器函数中
// 它非常类似return，详见第12章（迭代器和生成器）


// throw 抛出异常，可用try/catch/finally捕获


// try/catch/finally JavaScript的异常处理机制
try{

} catch(e){// ES2019 可以省略 (e)

}
finally{

}
```

其他语句：
```javascript
// with 用于创建临时作用域
// 基本被废弃，不推荐使用
with(object)
	statement;



// debugger 像一个断点，执行时JavaScript会停止，我们可以查看调用栈等信息




'use strict'    // 属于 指令（和语句很相近）
				// 具体作用见p115
```


声明（看起来很像语句，一并介绍）：
```javascript
/*
 * const let var function class import export 这些关键严格来讲都是声明
 */

const     // 声明常量
let       // 声明变量
var       // ES6之前唯一的声明变量方式

function  // 用于定义函数


/*
 * class 用于创建一个新类
 * 注意，类不会被“提升”，不能在类被声明之前使用它
 */
class myClass{
	constructor(init1){this.init1=init1}  // 构造函数
	statement;
}

/*
 * import 
 * 每一个js文件都是一个模块，之间完全无关，
 * import可以将其他模块的值导入，从而可以使用其他模块的值
 * 
 * export
 * 每一个js文件中的值都是 私有的，除非被显式导出，否则其他模块无法导入
 */
// index.js
import {xx,yy} from './xyz.js'

// xyz.js
const xx = 1;
const yy = 2;
export {xx,yy};

```

# **6 对象**

对象是一个 属性的无序集合，每个属性都有名字和值 和 三个属性特性（property attribute）
+ 可写（writable）
+ 可枚举（enumerable）
+ 可配置（configurable）

创建对象：
```javascript
let myDict = {}                         // 通过字面量创建 

let o = new Object()                    // 通过new关键字创建

/*
 * 几乎所有对象都有 原型，但只有少数对象有prototype属性。
 * 正是这些有prototype属性的对象为所有其他对象定义了 原型
 * Object.prototype是为数不多的没有原型的对象
 */

let o1 = Object.create({x:1,y:2,z:3})   // 通过Object.create()创建
```

查询和设置属性：
```javascript
o.property
o.["property"]   // 要求方括号中是字符串或可以转换成字符串的值


/*
 * JavaScript的对象可以视为 关联数组，可以动态添加属性
 */


/*
 * JavaScript的对象支持继承，参考“原型链”的概念
 * 同时，JavaScript只在查询时会用到原型链，而设置属性时不影响原型链
 */

let myName = class?.student?.name    // 访问属性时，
                                     // 对象为null或undefined时，报错TypeError
                                     // 属性不存在时，返回undefined
                                     
                                     // 同理，设置属性时，
                                     // 对象为null或undefined时，报错TypeError
                                     // 以及对象不允许添加新属性时，也会报错
                                     // 还有其他较复杂的情况，参考p128
```


删除属性：
```javascript
// delete 情况同上 见p128
```

测试属性：
```javascript
in 
hasOwnProperty()
propertyIsEnumerable()
!= undefined                    // 都可以用来判断某对象是否含有某属性
```

枚举类型：
```javascript
for/in
Object.keys() + for/of
Object.getOwnPropertyNames()
Object.getOwnPropertySymbols()
Object.ownKeys()                // 都可以用来枚举对象的属性
```

拓展对象：
```javascript
// 方式一：手动设置属性

// 方式二：ES6新增 Object.assign()
```


序列化对象(serialization)：把对象转为字符串，之后可以从中恢复对象的过程
```javascript
JSON.stringify()  // 序列化
JSON.parse()      // 恢复
                  // JSON是Javascript语法的子集，有些对象是不支持的
```

对象方法：
```javascript
toString()
valueOf()
toLocaleString()
toJson()           // 可以由JavaScript本身提供，也可以自己去重写或新增
```

对象字面量拓展语法（ES6新增）：
```javascript
// 属性名和变量名一样，可以简写
let x=1,y=2
let o = {x,y}   // o.x=1 o.y=2

// 计算的属性名 动态的计算出属性名 以下是一个通过代码来设置版本的例子
const PROPERTY_NAME="version"
function computePropertyName(s){return s + " " + PROPERTY_NAME}
let o = {
	[PROPERTY_NAME]:0.1,
	[computePropertyName("dev")]:0.2
}
```


符号作为属性名：
```javascript
// 接上，该拓展语法也可用于符号类属性
const extensionProperty = Symbol("my extension symbol property");
let o = {
	[extensionProperty]: {/*你的数据*/}
}
o[extensionProperty].x = /*你的值*/ // 这样的话，这种属性就不会和当前的任何属性冲突了
```

拓展操作符：
```javascript
// ... 三个点，当用于字面的对象时，可以起到这样的作用，所以称为“拓展操作符”
// 但放在其它场景的作用会完全不同
let o1 = {x:1,y:2}
let o2 = {z:3,p:5}
let o3 = {...o1, ...o2} //=> o3 = {x:1,y:2,z:3,p:5}

// 注意性能，单次拓展属性O(n)，循环则是O(n^2) !!!
```

简写方法（属性）：
```javascript
let o = {
	area: function(){return 0;}, // 简写 area(){return 0;}
}
o.area() //=> 0

// 支持符号、计算的属性名
const extensionProperty = Symbol("my extension symbol property");

let o2 = {
	[extensionProperty](){/*你的函数体*/},
}
```

属性的 获取方法 和 设置方法（ES5引入，ES6支持字面量拓展）：
```javascript
/*
 * JavaScript还支持为对象定义 访问器属性（accessor property）
 * 这种属性不是值，而是多个访问器方法
 * 获取方法：getter
 * 设置方法：setter
 * 
 * 当访问一个对象的属性时，就会调用对应的获取方法；对应的，设置属性值，调用设置方法
 * 若没有设置方法，则该属性不可写；对应的，若没有读取方法，则该属性不可读；都有，则可读写
 * 
 * 只需要在属性前加上set 或 get，然后跟定义方法属性一样
 * 支持符号和计算的属性名
 */
let o = {
	x:1,
	
	get p1(){return this.x},
	set p1(x){this.x = x;},

	get p2(){/**/},
	// 可以有多个，也可以没有
};
o.p1      //=> 1
o.p1(5)   //=> o.p1 = 5
```

# **7 数组**

数组可以看成特殊的对象，只不过属性名刚好是整数，也正是该特性让其访问起来效率比一般的对象更高
ES6推出了定型数组（typed Array），要求固定长度和固定数值元素类型。它性能极高，支持二进制可访问字节级的数据


创建数组：
```javascript
/*
 * 数组字面量
 * 逗号之间没有元素，就是稀疏数组，访问该位置返回undefined
 * 可以嵌套
 * 元素类型可以不一样，甚至可以是表达式
 */
let a = [1,,,3,]

/*
 * 对可迭代对象使用...拓展符(ES6新增)
 * 用来包含另一个数组（字符串或集合等可迭代对象）的所有元素
 * 实质是使用for/of，迭代元素并创建（浅）副本
 * 
 *  (忽略该数组最后一个元素后的逗号）当包含[1,2,]或[1,2]效果是一样的
 */
let b = [5,...a,7]      // => b = [5,1,,,3,7]
let c = [...new Set([..."hahaha"])]  // => c = ['h','a']：包含了字符串、集合以及去重等示例

/*
 * Array()构造函数
 */
let d = new Array()
let e = new Array(10)       // 指定长度为10的数组（此时，甚至索引0,1,2等都没有定义）
let f = new Array("1","2","a",1,4)
let g = new Array(..."abc") // => g = ['a','b','c']

/*
 * 工厂方法Array.of()
 */
let h = Array.of(1)         // => h = [1]:
						    // 弥补了Array()不能创建只含有一个数值元素的数组的缺陷
						    // 其余功能和Array()构造函数一样
/*
 * 工厂方法Array.from()
 * 第一个参数：可迭代对象 或 类数组对象
 *     当为可迭代对象时，同...
 *     当为类数组对象（有length属性，每个属性的键也是整数），作用是创建真正的数组副本
 * 第二个参数：函数
 *     把前者的每一元素传给这个函数，函数的返回值为新数组的元素
 *     （推荐在构建数组时就处理好，而不是用这个函数）
 */						    
let i = Array.from(h)       // => i = [1] 

```

读写数组元素：
```javascript
/*
 * 使用 [] 访问数组元素，方括号左侧是数组的引用，方括号中是非负整数
 * 
 * 数组是一种特殊的对象，用方括号访问数组元素和用方括号访问对象属性是类似的
 * JavaScript会把数组索引（整数）转为字符串，作为属性名
 * 数组所有索引都是属性名，但只有介于0~2^32-2之间的整数属性名才是索引
 * 可以给数组新建不属于索引范围内的属性名，比如a[-10]=9 => {"-10":9}
 * 
 * 数组长度由JavaScript内部自动维护，只对索引有效，其他属性名不会被统计在内
 */
let a = [..."abc"]
a[1]                  // => 'b'
a[1] = 3
a[1]                  // => 3
```

稀疏数组（包含undefined的非稀疏数组）：
```javascript
/*
 * 即时中间含有多个undefined元素，长度仍以最高索引为准
 */
let a = [,]         // => a没有元素，但a.length=1，且此时索引都没有被定义 0 in a = false
let b = [undefined] // => a有一个元素undefined，且a.length=1，索引被定义了 0 in b = true
```

数组长度：
```javascript
/*
 * 任何情况，数组索引都不会大于或等于数组长度
 * 可以修改数组length属性：
 *     要么删除所有等于或大于length的索引对应的数组元素
 *     要么在末尾增加一堆的undefined
 */
```

增加/删除数组元素
```javascript
/*
 * 主要注意，是否会修改length，以及元素的值和索引的变化
 */
let a = []

a[0] = 1         // => a = [1]
                 //    a.length=1
a.push("a","b")  // => a = [1,'a','b']
                 //    a.length=3
a.pop()          // => 返回'b'
                 //    a=[1,'a']
                 //    a.length=2

a.unshift("c")   // => a = ['c',1,'a']
                 //    a.length=3
a.shift()        // => 返回'c'
                 //    a=[1,'a']
                 //    a.length=2
 delete a[1]     // 删除索引2（也意味着删除了数组元素）
                 // a=[1,undefined]
                 // a.length=2 不影响数组长度
```

迭代数组：
```javascript
let arr = [..."abc"];

/*
 * for/of
 * forEach()
 * 除了用法不一致，两者对稀疏数组的处理不一样：
 *     前者对稀疏数组无感，若不存在便会得到undefined，后者则不会对undefined调用函数
 */
for(let i of arr) console.log(i);

// 数组提供的一种用于自身迭代的函数式方法：forEach
// 它接收一个函数作为参数，数组的每一个元素都会调用这个函数
arr.forEach(i => {console.log(i);})
```

# 8 函数

**定义函数**有多种方式：

1. 通过`function`关键字定义函数

   ```javascript
   //打印对象o的每个属性的名字和值。返回undefined
   function myFunc(o){
       for(let p in o){
           console.log(`${p}:${o[p]}\n`)
       }
   }
   
   
   ```

   

2. 箭头函数，很适合**将函数作为参数传给另一函数**的场景

   ```javascript
   const sum=(x,y)=>{return x+y;0};  
   ```

   

3. （特殊情况）对象字面量+类定义的快捷语法

4. （特殊情况）get和set定义的特殊**获取和设置方法**

5. （特殊情况）函数表达式

   ```javascript
   //不带函数名
   const square = function(x){return x*x;};
   //带函数名--适用于需要使用到函数的情况，比如递归
   const square = function fact(x){if(x<=1)return 1;else return x*fact(x-1);};
   //直接作为参数
   [3,2,1].sort(function(a,b){return a-1;})
   //定义并立即调用
   let sq = (function(x){return x*x;}(10))
   
   
   //以上，带函数名只是可选项。 
   ```

   

   

要注意函数声明，一是函数的名字变味了一个变量，其值就是函数本身；二是函数声明语句会被“提升”到包含脚本、函数或代码块的顶部   

  