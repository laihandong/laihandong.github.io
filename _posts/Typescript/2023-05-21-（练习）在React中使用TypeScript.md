---
title: TS + React 实现一个简易 TodoList
date: 2023-05-21 08:00:00
categories: 
- [Typescript,练习]
---



## 新建React项目

1. `npm i install create-react-app -g`，安装脚手架CRA


1. `npx create-react-app demo --template typescript`，创建名为demo的且支持typescript的react项目
2. `cd demo`,`npm start`进入到项目根目录，启动项目

## 项目结构特别说明


1. 项目结构：

   1. 项目根目录的`tsconfig.json`配置文件：项目文件和项目编译所需的配置项（`tsc --init`可以自动生成该文件）

      除了使用该文件来配置编译选项，还可以通过命令行的方式设置编译配置`tsc hello.ts --target es6`

      tsc后**带有输入文件时**，将忽略`tsconfig.json`，否则就会启用`tsconfig.json`（推荐后者）

   2. React组件的文件拓展名为`.tsx`

   3. src目录中`react-app-env.d.ts`：React项目默认的类型声明文件

      ```typescript
      /// <reference types="react-scripts" />
      ```

      **三斜线指令**：指定依赖的其他类型声明文件，types表示依赖的**类型声明文件包**的名称

## React中常用类型

React是组件化开发模式，任务就是写组件

### 函数组件

1. 函数组件定义和基本使用

   + 组件的类型以及组件的属性（props），`FC`全称`FunctionComponent`

   + 实现下图中的效果的代码如下：

     ![image-20230521191810505](https://images-1258290850.cos.ap-guangzhou.myqcloud.com/blog/202305211918834.webp)

     ```react
     import React from 'react';
     import ReactDOM from 'react-dom/client';
     import './index.css';
     // import App from './App';
     import reportWebVitals from './reportWebVitals';
     import { FC } from 'react'
     
     // 定义一个类型
     type Props = { name: string; age?: number }
     // 写一个 React 函数组件
     const Hello: FC<Props> = ({ name, age }) => (
       <div>
         Hello, I'm {name}. I'm {age} years old.
         </div>
     )
     // 根据原生js的风格，来编写一个 React 组件
     // const Hello = ({ name, age }: Props) => (
     //   <div>
     //     Hello, I'm {name}. I'm {age} years old.
     //   </div>
     // )
     
     
     // 再写一个 React 函数组件
     const App = () => (
       <div>
         <Hello name="jack" age={17} />
       </div>
     )
     
     // 根据 React 语法渲染
     const root = ReactDOM.createRoot(
       document.getElementById('root') as HTMLElement
     );
     root.render(
       <React.StrictMode>
         <App />
       </React.StrictMode>
     );
     ```

     

   + 组件属性的默认值

     ```typescript
     //接上示例：给 Hello 函数组件的属性 age 设置默认值
     
     // ... 上面代码不变
     const Hello: FC<Props> = ({ name, age }) => (
       <div>
         Hello, I'm {name}. I'm {age} years old.
       </div>
     )
     Hello.defaultProps = { age: 18 }
     // ... 下面代码不变
     
     // ====================法二：使用原生js的风格========================
     // const Hello = ({ name, age = 18}: Props) => (
     //   <div>
     //     Hello, I'm {name}. I'm {age} years old.
     //   </div>
     // )
     ```

     

   + 事件绑定和事件对象

     ```typescript
     const App = () => {
       const onClick = (e: React.MouseEvent<HTMLButtonElement>) => { console.log('赞！')} // 给事件的函数参数，设置 事件对象
     
       return (
         <div>
           <Hello name="jack" age={17} />
           <button onClick={onClick}>点赞</button> // 给 button 的 onClick 属性绑定函数
         </div>
       )
     }
     ```

     绑定之后，可以触发相应的函数；而**设置事件对象后，可以拿到相应的对象属性/方法**

     **判断事件对象的类型**时，有个技巧如下：

​			![image-20230521193755133](https://images-1258290850.cos.ap-guangzhou.myqcloud.com/blog/202305211937450.webp)

### class组件

class组件继承泛型类`React.Componet`，第一个参数表示属性，第二个参数表示状态

![image-20230521194611357](https://images-1258290850.cos.ap-guangzhou.myqcloud.com/blog/202305211946647.webp)

同样使用class组件，完成之前函数组件的效果

![image-20230521195515520](https://images-1258290850.cos.ap-guangzhou.myqcloud.com/blog/202305211955777.webp)

```typescript
import React from 'react';
import ReactDOM from 'react-dom';

// 定义一个类型
type Props = { name: string; age?: number }

class Hello extends React.Component<Props> {
  static defaultProps: Partial<Props> = {
    age: 18
  }

  render() {
    const { name, age } = this.props
    //const { name, age = 18} = this.props // 使用原生js风格设置默认值
    return (
      <div>
        Hello, I'm {name}. I'm {age} years old.// 访问属性
      </div>
    )
  }

}


type State = { count: number }
class Counter extends React.Component<{}, State>{
  state: State = {
    count: 0
  }
  OnIncremnt = () => {
    this.setState({
      count: this.state.count + 1 // 访问、修改状态
    })
  }
  render() {
    return (
      <div>
        计数器：{this.state.count}
        <button onClick={this.OnIncremnt}>+1</button>
      </div>
    )
  }
}

const App = () => (
  <div>
    <Hello name='jack'></Hello>
    <Counter />
  </div>
)

ReactDOM.render(<App />, document.getElementById('root'))
```

class组件的**组件状态和事件**

## 实现 简易Todo工具

### 任务展示列表

#### 思路：

使用状态提升（在父组件提供状态，通过props传递给子组件）来实现**父→子**通讯



#### 步骤：

1. 为父组件App，提供状态（任务列表数据）和类型
2. 为子组件TodoList指定能够接收到的props类型
3. 将任务列表数据传递给TodoList组件



#### 优化

使用类型声明文件，实现类型共享



### 添加任务

#### 思路

子组件获取到文本框的值，通过**子→父**通讯，将文本框的值传递给父组件。然后，在父组件



#### 步骤

1. 为子组件添加状态和属性及其类型
   + 状态：文本框的值
   + 属性：回调函数，接收一个`string`类型的参数
2. 通过**受控组件**方式获取到文本框的值
3. 在子组件文本框**按下回车键**将数据传递给父组件
4. 父组件接收子组件传递过来的任务名称（文本框的值）
5. 将任务添加到父组件的状态数据中



#### 优化

空文本按下回车不应该添加

任务删除后，文本应该重置
