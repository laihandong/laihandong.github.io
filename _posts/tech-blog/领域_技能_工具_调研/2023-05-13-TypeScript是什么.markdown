+ **TS是什么？**

	Typescript是Javascript的**超集**，简称TS。（甚至可以说所有合法的js代码就是ts）

	![image-20230516134556276](https://images-1258290850.cos.ap-guangzhou.myqcloud.com/blog/202305161346547.webp)

	在完全兼容js的基础上，添加了**类型支持**

	ps:用过js就知道没有类型检查给开发带来的负面影响有多么不可忽视



+ **TS的优势？**

  + 类型系统、类型推断机制、静态检查、全局代码提示，提升开发效率，还方便重构
  + 支持最新ECMAScript语法，走在技术前沿

+ **如何获取TS？**

  + `npm i -g typescript`

  + 由于ts编译器推广的范围有限，主流仍是以js为准，如下：

    ![image-20230516143136292](https://images-1258290850.cos.ap-guangzhou.myqcloud.com/blog/202305161431470.webp)

+ **TS-->"Hello World!"?**
  + 创建文件`hello.ts`，内容：`console.log('Hello World!');`
  + 方法一：
    + 将ts编译为js：`tsc hello.ts`
    + 执行js：`node hello.js`
  + 方法二：
    + 安装ts-node包：`npm i -g ts-node`
    + 使用ts-node运行ts：`ts-node hello.ts`
+ 
