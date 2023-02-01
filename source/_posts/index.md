---
title: typescript学习记录
date: 2021-03-30 20:18:22
tags: typescript
---

## typescript 实践
  添加了类型系统的javascript，适用于任何模块。
  js是动态类型，运行时报错。ts位静态类型，编译时进行类型检查。
  js也是弱类型，允许隐式类型转换。在完整的保留js运行时的基础上，引入静态类型系统，提高代码的可维护性。
  Typescript 是一个强类型的 JavaScript 超集，支持ES6语法，支持面向对象编程的概念，如类、接口、继承、泛型等。Typescript并不直接在浏览器上运行，需要编译器编译成纯Javascript来运行。
  
  - 原始数据类型： boolean、string、void（没有任何返回值）、unll和undefined（所有类型的子类型）
  - 任意值any： 赋值时可改变类型，可访问任意属性和方法。未声明类型的变量会被识别为任意值。
  - 类型推论： 定义并赋值 推断为赋值类型。 定义未赋值推断为any。
  - 联合类型： 取值可为多种类型用｜隔开 eg. number | string 
  - 对象的类型——接口interfaces  抽象，具体行动由类（class）去实现（implement）
    赋值时，变量的形状必须与接口的形状保持一致。
    可选属性?   age?: number;
    任意属性  [propName: string]: any;  *一旦定义了任意属性，确定属性和可选属性必须是任意属性的子集。
    只读属性readonly  readonly id: number;  *注意，只读的约束存在于第一次给对象赋值的时候，而不是第一次给只读属性赋值的时候
  - 数组类型   类型+方括号 number[]  或   数组泛型 Array<number>  用接口表示数组。

```js
      // 接口表示数组
      interface NumberArray {
          [index: number]: number;
      }
      let fibonacci: NumberArray = [1, 1, 2, 3, 5];

      //类数组
      function sum() {
          let args: {
              [index: number]: number;
              length: number;
              callee: Function;
          } = arguments;
      //常见做法 any 任意类型
      let list: any[] = ['xcatliu', 25, { website: 'http://xcatliu.com' }];    
      }
```
  事实上常用的类数组都有自己的接口定义，如 IArguments, NodeList, HTMLCollection 等。
  - 函数类型
```ts
      // 函数声明
      function decement(num: number, str: string):string {
        return num + str
      }
      // 函数表达式
      let incement = function(num: number, str: number):number { return num - str }
      // 等同于下面
      let incement: (x: number, y: number) => number = function (x: number, y: number): number {
          return x - y;
      };
      // 注意不要混淆了 TypeScript 中的 => 和 ES6 中的 =>。
      // 在 TypeScript 的类型定义中，=> 用来表示函数的定义，左边是输入类型，需要用括号括起来，右边是输出类型。
      // 在 ES6 中，=> 叫做箭头函数

      //接口定义函数形状
      interface SearchFun {
        (source: number, subString: string):boolean;
      }
      const mySearch: SearchFun = function(source: number, subString: string) {
        return subString.indexOf(source) > -1
      }

      // 可选参数 ？ 可选参数必须接在必需参数后面
      // TypeScript 会将添加了默认值的参数识别为可选参数 此时不受「可选参数必须接在必需参数后面」的限制
      // 剩余参数  ...rest
      function myPush(a:number[], ...items: any[]):void {
        items.forEach(item => {
          a.push(item)
        })
      }
      let a = [1]
      myPush(a, 2,4,3)

      // 重载
      function reverse(x: number): number;
      function reverse(x: string): string;
      function reverse(x: number | string): number | string | void {
          if (typeof x === 'number') {
              return Number(x.toString().split('').reverse().join(''));
          } else if (typeof x === 'string') {
              return x.split('').reverse().join('');
          }
      }
```
  - 类型断言  语法：值 as 类型  或  <类型>值
    在 tsx 语法（React 的 jsx 语法的 ts 版）中必须使用前者，即 值 as 类型。

  - 声明文件

  - 内置对象
  
  
  
    



  

  







  

