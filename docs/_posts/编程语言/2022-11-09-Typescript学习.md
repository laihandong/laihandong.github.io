# Typescript简介

+ TypeScript 是 JavaScript 的一个超集，支持 ECMAScript 6 标准（[ES6 教程](https://www.runoob.com/w3cnote/es6-tutorial.html)）
+ TypeScript 由微软开发的自由和开源的编程语言。
+ TypeScript 设计目标是开发大型应用，它可以编译成纯 JavaScript，编译出来的 JavaScript 可以运行在任何浏览器上
+ TypeScript 是 JavaScript 的超集，扩展了 JavaScript 的语法，因此现有的 JavaScript 代码可与 TypeScript 一起工作无需任何修改，TypeScript 通过类型注解提供编译时的静态类型检查
+  TypeScript 可处理已有的 JavaScript 代码，并只对其中的 TypeScript 代码进行编译

+ TypeScript，可以使用你真正想要的方式编写[JavaScript](https://www.w3cschool.cn/javascript/)！

+ 代码最终编译为普通的JavaScript
+ TypeScript是纯面向对象与类，接口和静态类型。就像C＃或Java一样
+ 著名的JavaScript框架angular2.0是使用TypeScript编写的
+ 掌握TypeScript可以帮助程序员编写面向对象的程序并将它们编译为JavaScript，无论是在服务器端或客户端



# 入门



注释

- **单行注释 ( // )** − 在 // 后面的文字都是注释内容。
- **多行注释 (/\* \*/)** − 这种注释可以跨越多行。



TypeScript 是一种面向对象的编程语言。

```typescript
class Site { 
   name():void { 
      console.log("Runoob") 
   } 
} 
var obj = new Site(); 
obj.name();
```

编译后的JavaScript如下

```javascript
var Site = /** @class */ (function () {
    function Site() {
    }
    Site.prototype.name = function () {
        console.log("Runoob");
    };
    return Site;
}());
var obj = new Site();
obj.name();
```

等下，还是现学JavaScript吧，看网上的人说先学typescript就本末倒置了。所以说，用typescript来开发会更快一些，因为有些像c语言，很熟悉。