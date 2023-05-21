# 类型

## 类型注解

`let age: number = 18`

类似冒号后跟类型名的形式，就是类型注解。typescript是强类型，不同于python。

## 常用基础类型

`JS`已有类型：`number/string/boolean/null/undefined/symbol/`**object**

```typescript
// JS已有的基础类型
let age: number = 12

// 数组类型（推荐第一种、第三种）
let nums: number[] = [1,2,3]
let strs: Array<string> = ['a','b']
```



`TS`新增类型：联合类型、自定义类型（类型别名）、接口、元组、字面量类型、枚举、void、any等

+ **联合类型**，在TS中用 `|` 表示

  ```typescript
  // | 在TS中叫做 联合类型，
  let mixs: (number|string)[] = ['a',1,'b'] // 用法一：表示数组中的元素可以是number或string类型
  let mix2: number | string[] = 1 // 用法二：表示mix2可以是number或者是string数组类型
  ```

+ **类型别名**（自定义类型）,在TS中用`type`表示

  ```typescript
  type MyType = (number|string)[]
  let aVar: MyType = [1,'a']
  ```

+ **函数类型**（可以理解为函数参数和返回值的类型）

  ```typescript
  function add(num1: number, num2: number): number {
      return num1 + num2
  }
  // 稍微复杂的情景：匿名函数+函数表达式
  const add: (num1: number, num2: number) => number = (num1, num2) => { return num1 + num2 }
  // 可以理解为const add: (类型) = () => {}
  
  
  // 没有返回值：void
  function hello(): void { console.log('hello！') }
  ```

  + **可选参数**（可传可不传），在TS中用`?`表示

    ```typescript
    function mySlice(start?: number, end?: number): void {
        console.log('start:', start, 'end:', end)
    }
    // 可选参数应放在参数列表末尾，所有必传参数之后
    ```

+ **对象类型**

  ```typescript
  let person: { name: string, age: number, sayHi: ()=>void, greet(name:string):void } = {
      name: 'lhd',
      age: 23,
      sayHi() { console.log('Hi! I\'m lhd.') },
      greet(name){console.log(`Hi, my ${name}.`)}
  }
  
  // 可以理解为 let person: (类型) = {}
  type Person = { 
      name: string, 
      age: number, 
      sayHi: () => void, 
      greet(name: string): void }
  
  let me:Person = {
      name: 'lhd',
      age: 23,
      sayHi() { console.log('Hi! I\'m lhd.') },
      greet(name) { console.log(`Hi, my ${name}.`) }
  }
  ```
  + 分隔符

      ```typescript
      // 当参数被多行分隔的话，可以省略末尾的分隔符(;或者,)
      let person: { 
          name: string
          age: number
          sayHi: () => void
          greet(name: string): void } = {
          name: 'lhd',
          age: 23,
          sayHi() { console.log('Hi! I\'m lhd.') },
          greet(name) { console.log(`Hi, my ${name}.`) }
      }
      ```

  + 对象可选属性，用`?`表示

    ```typescript
    let person: { 
        name?: string // 被标记为可选
        age: number 
        sayHi?(): void // 被标记为可选
        greet(name:string):void } = {
        name: 'lhd',
        age: 23,
        //sayHi() { console.log('Hi! I\'m lhd.') },
        greet(name){console.log(`Hi, my ${name}.`)}
    }
    ```

    

+ **接口**（多用于复用的场景）

  ```typescript
  interface IPerson {
      name: string
      age: number
      sayHi(): void
  }
  
  // 复用
  let me: IPerson={
      name:'lhd',
      age:12,
      sayHi(){}
  }
  ```

  + 与类型别名的区别

    相同点：都可以给对象指定类型

    不同点：接口只能为 *对象*  指定类型；类型别名可以为 *任意类型*  指定别名

  + **接口继承**（在TS中用`extends`表示）

    ```typescript
    interface Point2D { x: number; y: number }
    interface Point3D { x: number; y: number; z: number }
    
    // 等效于如下：
    interface Point2D { x: number; y: number }
    interface Point3D extends Point2D { z: number }
    ```

    

+ **元组**（确切知道元素个数和类型的特例数组）

  ```typescript
  let position: [number, number] = [12,4]
  ```

  

## 类型推论

TS类型推论机制会帮助推断 **没有明确声明类型且立即初始化值的** 变量的类型

比如在`VSCode`中，将鼠标放在没有明确声明类型的变量上，就可以看到如下效果：

![image-20230516172432367](https://images-1258290850.cos.ap-guangzhou.myqcloud.com/blog/202305161729827.webp)

TS类型推论机制会帮助推断 **没有明确声明函数返回值类型的** 函数的返回值类型

![image-20230516173246860](https://images-1258290850.cos.ap-guangzhou.myqcloud.com/blog/202305161732093.webp)



## 类型断言

当只能获取一个**范围较为宽泛的类**但又想调用某个不在该类中的属性或方法，往往是失败的。

这时就要使用类型断言**指定更加具体的类型**，在TS中用`as`表示，也可以用`<>`表示

```typescript
// 推荐第一种
const aLink = document.getElementById('link') as HTMLAnchorElement
const aLink = <HTMLAnchorElement>document.getElementById('link')

aLink.href // 调用某个属性
```



## 字面量类型

仔细看如下两个变量的类型：

![image-20230516174803273](https://images-1258290850.cos.ap-guangzhou.myqcloud.com/blog/202305161748465.webp)

![image-20230516174828796](https://images-1258290850.cos.ap-guangzhou.myqcloud.com/blog/202305161748006.webp)

在这个例子当中

由`let`声明的变量`str1`是 **可变的**，可以为任意字符串，所以为`string`

由`const`声明的变量`str2`是 **不可变的**，所以类型为它本身，即`ss`。类似这样的类型，在TS中被称为 字面量类型



+ **使用模式**

  配合联合类型一起使用

+ **使用场景**

  表示一组明确可选的值列表

  ```typescript
  function changeDirection(direction: 'up' | 'down' | 'left' | 'right') {
      console.log(direction)
  }
  ```





## 枚举类型

枚举的功能类似 字面量类型+联合类型组合 的功能，可以**表示一组明确的可选值**，在TS中用`enum`表示

```typescript
enum Direction { Up, Down, Left, Right }

function changeDirection(direction: Direction) {
    console.log(direction)
}
```

+ 在TS中用`.`访问枚举类型的成员

  ```typescript
  console.log(Direction.Up)
  ```

+ 枚举的值 **默认是从0开始的自增自然数**，也可以在声明时手动赋值

  ```typescript
  enum Direction { Up = 2, Down = 4, Left = 6, Right = "right" }
  ```

+ **数字枚举**的赋值顺序默认是 **自增的**

  枚举的值都为数字的枚举被称为 数字枚举；同理由 字符串枚举，但它没有自增长行为。

  一个数字、字符串混合枚举的例子如下：

  ![image-20230516182131615](https://images-1258290850.cos.ap-guangzhou.myqcloud.com/blog/202305161821820.webp)

+ 枚举的特性和原理：**不仅用作类型，还提供值，是为数不多的非`JS`类型拓展之一**



综上，还是更推荐**字面量类型+联合类型组合**的方式，更简洁、直观、高效

```typescript
function changeDirection(direction: 'up' | 'down' | 'left' | 'right') {
    console.log(direction)
}
```



## any类型

原则：**不推荐使用any！**这会让`TypeScript`变为`AnyScript`（失去TS类型保护的优势）

```typescript
let obj: any = {x:0}

obj.bar=100
obj()
```

**以上操作不会有任何类型错误提示，即使可能存在错误**



除此之外，还要注意**隐式声明any**的情景：

```typescript
// 声明变量不提供类型和默认值
let a

// 函数参数不加类型
function add(n1,n2){return n1+n2}
```

## typeof运算符

ts-typeof：js只是打印类型（打印的结果本身类型是string），**ts中则是返回类型注解，本身也可以用来类型注解**



## 高级类型 class

class的基本使用：类似直接把变量和函数写在`class yourName {...}`就可以了

class的**构造函数**：要注意的四点就是声明必须且只有`constructor`，且**不能有**自定义的函数**返回值注解**，用`this`**访问自己**，**成员初始化后才能**用构造函数访问和赋值（重要）

class的方法：不需要由function声明，但函数类型注解的使用跟普通函数一样

```typescript
class Car {
    brand:string
    
    constructor(brand:string){
        this.brand = brand
    }
    
    get price(){
        return 1000
    }

    printBrand():void{
        console.log(this.brand)
    }
}

const c = new Car("红旗")
console.log(c.price)
c.printBrand()
```

### class 继承

class-继承：使用`extends`关键字, 用于继承父类的**所有**属性和方法

```typescript
class Animal{
    move(){console.log('Moving!')}
}
class Dog extends Animal{
    bark(){console.log('汪汪汪')}
}
```

### class 实现接口

> TypeScript中的接口（interface）是一种定义对象结构的方法，它允许您规定对象应具有哪些属性和方法，以及它们的类型。接口在编译时进行类型检查，但不会生成任何实际的JavaScript代码。使用接口可以帮助您确保代码遵循预期的结构和约束，从而提高代码的可读性和健壮性。



class-实现接口：使用`implements`让class**实现接口**，前提是**接口必须提供所有属性和方法**，**同时类必须实现接口所有的****属性和方法（重要）

```typescript
interface Singable {
    sing(): void
}
class Person implements Singable {
    sing() {
        console.log('Yi ya yi ya yi~')
    }
}
```



### class 可见性修饰符

class-可见性修饰符：使用`public、protected、private`放在属性或者方法前，表示对于class外的代码是否可见；默认是public，可以省略；三者对应的**生效范围分别是：实例、类、子类；类、子类；类**



### class 其它修饰符

class-其它修饰符：`readonly`（**只读修饰符**），表示该属性是只读的，**不能修饰方法**；额外需要注意的是，**需要手动指定注解**（不指定的话，会被推论成它的值本身）



## 类型兼容性

类型兼容性：常见的类型系统有两种`Structure Type System`（结构化类型系统）、`Nominal Type System`（标明类型系统）。**TS用的是前者**，也被称为“鸭子类型” 类型兼容*可以体现在，接受三个参数的函数，可以只传入1个参数；相同结构但不同的类，可以互相作为对方实例化时的注解*；等等

### 对象类型的兼容

对于对象类型，A的成员至少与B相同，则B兼容A（**成员多的可以赋值给少的**）

```typescript
class Point { x: number; y: number }
class Point3D { x: number; y: number; z: number }
const p: Point = new Point3D()
```



### 接口类型的兼容

对于接口（类似interface Point{x:number,y:number}），同上

```typescript
interface Point { x: number; y: number }
interface Point2D { x: number; y: number }
interface Point3D { x: number; y: number; z: number }

let p1: Point = { x: 1, y: 2 }
let p2: Point2D = { x: 11, y: 22 }
let p3: Point3D = { x: 111, y: 222, z: 333 }

p1 = p2
p2 = p3
```



### 函数类型的兼容

1. **参数个数**兼容：**参数少的可以赋值给多的**（看上去与之前的相反）

   ```typescript
   type F1 = (a: number) => void
   type F2 = (a: number, b: number) => void
   
   let f1: F1 = (x) => { console.log(`number ${x}`) }
   let f2: F2 = f1
   
   f2(3,4) // output: number 3
   ```

   

2. **函数参数类型**兼容：相同位置的参数类型要相同（原始类型）或兼容（对象类型），（判断兼容的技巧：把对象拆开，把每个属性看做一个个参数，然后基于参数个数兼容）

   ```typescript
   interface Point2D { x: number; y: number }
   interface Point3D { x: number; y: number; z: number }
   type F1 = (p: Point2D) => void
   type F2 = (p: Point3D) => void
   
   let f1: F1 = (p) => { console.log(`x: ${p.x}, y: ${p.y}`) }
   let f2: F2 = f1
   
   
   f2({ x: 3, y: 4, z: 5 }) // output: x: 3, y: 4
   ```

   

3. **返回值类型**兼容：如果返回值是原始类型，此时两个类型要相同；如果返回值是对象类型，则成员多的可以赋值给成员少的

    ```typescript
    type F1 = () => string
    type F2 = () => string
    type F3 = () => { name: string, age: number }
    type F4 = () => { name: string }
    
    let f1: F1 = () => { return "f1" }
    let f2: F2 = f1
    
    let f3: F3 = () => { return { name: "f3 name", age: 3 } }
    let f4: F4 = f3
    ```

综上，有关函数参数相关的，少的可以赋给多的；有关对象和接口的，多的可以赋给少的。

（ps：有点像 方法属性可以冗余，但参数不能溢出）

## 交叉类型

在TS中用`&`表示，功能类似于接口继承（extends），**用于组合多个类型为一个类型（常用于对象类型）**

```typescript
interface Person { name: string }
interface Contact { phone: string }
type PersonDetail = Person & Contact

let obj: PersonDetail = {
    name: 'jack',
    phone: '188...'
}

```

这样，新类型`PersonDetail`就同时具有`Person 和 Contact`的所有属性和方法了



**和接口继承extends的区别**：

相同点：都可以实现对象类型的组合

不同点：**对于同名属性时，处理类型冲突的方式不一样**

![image-20230521112052349](https://images-1258290850.cos.ap-guangzhou.myqcloud.com/blog/202305211120718.webp)

对于继承`extends`这样会报错；而对于交叉类型，效果如下：

![image-20230521112530475](https://images-1258290850.cos.ap-guangzhou.myqcloud.com/blog/202305211125772.webp)

```typescript
interface A {
    fn: (value: number) => string
}
interface B {
    fn: (value: string) => string
}

type C = A & B
let c: C = {
    fn(value: number | string) {
        return ''
    }
}
```

## 泛型

泛型，是可以在**保证类型安全（即 不丢失类型信息）前提下**，让函数等与**多种类型一起工作**，从而**实现复用**，常用于：函数、接口、class中

### 调用泛型函数

```typescript
function id<Type>(value: Type): Type { return value }

// 手动指定`<>`中的类型
const num = id<number>(10)
const str = id<string>('a')

// 省略<类型>（需要注意可能被推断为*字面量类型*）
const num = id(10)
const str = id('a')
```





### **类型变量**

如例子中的`Type`，它是一种**特殊类型的变量，它处理类型**而不是值

该变量相当于一个类型容器，**能够捕获用户提供的类型**（具体是什么类型**由用户调用该函数时指定**）

因为`Type`时类型，因此可以将其作为函数参数和返回值的类型，同时也代表参数和返回值具有相同的类型

类型变量`Type`，**可以是任意合法的变量名称**



### 泛型约束

默认情况下，泛型函数的类型变量`Type`可以代表多个类型，这导致**无法访问任何属性**，例子中，`Type`可以代表任意类型，无法保证一定存在`length`属性，所以会报错。

![image-20230521145027844](https://images-1258290850.cos.ap-guangzhou.myqcloud.com/blog/202305211450253.webp)



解决该问题，有两种方法：

1.指定**更加具体的**类型

```typescript
function id<Type>(value: Type[]): Type []{ 
    console.log(value.length)
    return value 
}
```

2.**添加约束**

```typescript
// 创建描述约束的接口，该接口要求提供length属性
interface ILength { length: number }

// 通过 extends 关键字使用接口，为泛型（类型变量） 添加约束
// 表示，传入的类型必须具有length属性
function id<Type extends ILength>(value: Type): Type {
    console.log(value.length)
    return value
}
```



### 多个变量类型 + 变量间的约束

泛型的类型变量可以有多个，并且类型变量之间还可以约束（比如，第二个类型变量受第一个类型变量约束）

```typescript
function getProp<Type, Key extends keyof Type>(obj: Type, key: Key) {
    return obj[key]
}

let person = { name: 'jack', age: 18 }
getProp(person, 'name')
```

代码示例解释：

1. 添加了第二个类型变量Key，两个类型变量之间使用`,`分隔
2. `keyof`关键字接受一个对象类型，生成其键名称（可能是字符串或数字）的联合类型
3. `Key`后面接`extends`表示受`keyof Type`的约束

### 泛型接口

在接口名称的后面添加<类型变量>，那么**这个接口就变成了泛型接口**

接口的类型变量，接口中的所有成员都可以使用

使用泛型接口时，需要**显式指定具体的类型**

```typescript
interface IdFunc<Type> {
    id: (value: Type) => Type
    ids: () => Type[]
}

let obj: IdFunc<number> = {
    id(value) { return value },
    ids() { return [1, 3, 5] }
}
```



**数组是泛型接口：**

`lib.es5.d.ts`

![image-20230521154341916](https://images-1258290850.cos.ap-guangzhou.myqcloud.com/blog/202305211543191.webp)



### 泛型类

`class`也可以配合泛型来使用，类似于接口，在`class`后面添加<类型变量>，那么**这个类就变成了泛型类**

```typescript
class GenericClass<Type>{
    defaultValue: Type
    showValue: () => Type = () => {
        console.log(this.defaultValue)
        return this.defaultValue
    }

    constructor(x: Type) {
        this.defaultValue = x
    }
}
const myClass = new GenericClass<number>(1)
myClass.defaultValue = 1
myClass.showValue()
```

### 泛型工具类型

TS中内置了常用的工具类型，简化了TS中的一些常见操作，**它们都是基于泛型实现**的

+ `Partial<Type>`用来构造（创建）一个类型，将`Type`的所有属性**设置为可选**

  ```typescript
  interface Props {
      id: string
      children: number[]
  }
  type PartialProps = Partial<Props>
  ```

  ![image-20230521160831272](https://images-1258290850.cos.ap-guangzhou.myqcloud.com/blog/202305211608555.webp)

+ `Readonly<Type>`用来构造（创建）一个类型，将`Type`的所有属性**设置为readonly（只读）**

  ```typescript
  interface Props {
      id: string
      children: number[]
  }
  
  type ReadonlyProps = Readonly<Props>
  ```

  ![image-20230521161049673](https://images-1258290850.cos.ap-guangzhou.myqcloud.com/blog/202305211610960.webp)

+ `Pick<Type, Keys>`从`Type`中**选择一组属性来构造新类型**（`Keys`只能是`Type`中存在的属性）

  ```typescript
  interface Props {
      id: string
      children: number[]
      other: string
  }
  
  type PicklyProps = Pick<Props, 'id' | 'other'>
  
  ```

  ![image-20230521161309235](https://images-1258290850.cos.ap-guangzhou.myqcloud.com/blog/202305211613504.webp)

+ `Record<Keys, Type>`**构造一个对象类型**，属性键为`Keys`，属性类型为`Type`）

  ```ty
  type RecordObj = Record<'name' | 'nickname', string>
  
  let obj: RecordObj = {
      name: 'my name',
      nickname: 'my nickname'
  }
  ```

  

## 索引签名类型

当**无法确定对象中有哪些属性**（或者说对象中可以出现任意多个属性），此时，就**用到索引签名类型**了

```ty
interface AnyObject {
    [Key: string]: number
}

let obj: AnyObject = {
    a: 1,
    b: 2
}
```

使用`[key: string]`**表示索引签名**，用来约束该接口中允许出现的属性名称类型。表示只要是`string`类型的属性名称，都可以出现在对象中

这样，对象obj中就**可以出现任意多个属性**

`key`**只是一个占位符**，可以换成任意合法的变量名称



数组中可以出现任意多个元素，在`Array`中的定义中也可以看到使用了索引签名类型

![image-20230521163154121](https://images-1258290850.cos.ap-guangzhou.myqcloud.com/blog/202305211631411.webp)

## 映射类型

**基于旧类型创建新类型（对象类型）**，减少重复，提高开发效率。映射类型本质是基于索引签名类型的，所以也使用了`[]`

注意：**映射类型只能在类型别名中使用，不能在接口中使用**



1.基于 联合类型 创建 对象类型

```typescript
type PropKeys = 'x' | 'y' | 'z'
type Type1 = { x: number; y: number; z: number }

// 使用 映射 减少重复，效果和Type1相同
type Type2 = { [Key in PropKeys]: number }
```



2.基于 对象类型 创建 对象类型

![image-20230521164810219](https://images-1258290850.cos.ap-guangzhou.myqcloud.com/blog/202305211648503.webp)

3.泛型工具类型 也是基于映射类型实现的

![image-20230521165120536](https://images-1258290850.cos.ap-guangzhou.myqcloud.com/blog/202305211651820.webp)

​	这就涉及 **索引查询（访问）类型**：`[]`访问的属性必须存在于被查询的类型中，否则报错，下面是几个例子



![image-20230521165612939](https://images-1258290850.cos.ap-guangzhou.myqcloud.com/blog/202305211656204.webp)

![image-20230521165945104](https://images-1258290850.cos.ap-guangzhou.myqcloud.com/blog/202305211659389.webp)

![image-20230521170146385](https://images-1258290850.cos.ap-guangzhou.myqcloud.com/blog/202305211701676.webp)



## TS中的两种文件类型

`.ts`是`implementation`代码实现文件，`.d.ts`是`declaration`类型声明文件

+ 文件格式为`.ts`
  1. 既可以包含 类型，也可以包含 可执行代码
  2. 可以被编译成 `.js`文件，然后执行代码
  3. 用途：编写程序代码的地方

+ 文件格式为`.d.ts`
  1. 只可以包含 类型，不可以出现 可执行代码
  2. 不会被编译成 `.js`文件，仅仅提供类型信息
  3. 用途：为JS提供类型信息

### 类型声明文件

项目使用到的JS第三方库几乎都有相应的TS类型，是因为有类型声明文件**用来为已存在的JS库提供类型信息**

*下载一个JS第三方库，在其目录下的`.d.ts`结尾的就是类型声明文件*



+ 使用已有的类型声明文件

  + 内置类型声明文件：**TS为JS运行时可用的所有标准化内置API都提供了声明文件**，例如`lib.es5.d.ts`

  + 第三方库类型声明文件：

    几乎所有常见的第三方库都有相应的类型声明文件，当下载一个第三方库的包时，可以在包的目录中发现`index.d.ts`（`packages.json`中的`typings`会指定加载哪个声明文件）

    第二种方式，由`DefinitelyTyped`提供（是一个github上的库，用来**提供高质量TypeScript类型声明**）。可以通过`npm/yarn`来下载该仓库提供的TS类型声明包，这些包的名称格式为：`@types/*`，下载安装后，**TS也会自动加载该类型声明包**

  ​	

### 编写自己的类型声明文件

+ **项目内共享类型**：如果多个`.ts`文件中都用到同一个类型，此时可以创建`.d.ts`文件提供该类型，实现类型共享

1. 创建`index.d.ts`类型声明文件
2. 创建需要共享的类型，并使用`export`导出
3. 在需要使用的共享类型的`.ts`文件中，通过`import`导入

```typescript
  // index.d.ts
  type Props = { x: number; y: number }

  export {Props}
  
  
  // main.ts
  import { Props } from ".";
  
  let p: Props = {
      x: 1,
      y: 2
  }
  
```



+ **为已有JS文件提供类型声明**

​	在将JS项目迁移到TS项目中时，为了让已有的`.js`文件由类型声明，或者自己开发了一个库分享给别人使用。但是需要注意的是，和上面的情景不一样，**类型声明文件的编写于模块化方式相关**。JS模块化的发展可谓百家争鸣（AMD、CommonJS、UMD、ESModule等），所以导致TS在支持所有模块化的基础上，其声明文件的相关内容又多又杂



机制说明：

1. TS项目中也可以使用`.js`文件

2. 在导入`.js`文件时，TS会自动加载与`.js`同名的`.d.ts`文件，以提供类型变量

   

`declare`关键字：用于类型声明，为其他地方（比如，`.js`文件）已存在的变量声明类型，而不是创建一个新的变量

1. 对于type、interface等这些明确就是TS类型的（只能在TS中使用），可以省略`declare`关键字
2. 对于let、function等具有双重含义（在JS、TS中都能用），应该使用`declare`关键字，明确类型

```typescript
// main.js
let count = 10

function add(x, y) {
    return x + y
}

const formatPoint = point => {
    console.log(point)
}

// main.d.ts
declare let count: number

declare function add(x: number, y: number): number

interface Point {
    x: number
    y: number
}
declare let point: Point

declare const formatPoint: (point: Point) => void


// 注意：类型提供好以后，需要使用 模块化方案 中提供的
//      模块化语法，来导出声明好的类型，然后，才能在
//      其它的 .ts 文件中使用
// 以下时ES6的模块化语法
export { count, add, Point, point, formatPoint }
```





