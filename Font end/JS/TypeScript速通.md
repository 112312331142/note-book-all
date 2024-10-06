## 一、TypeScript简介

### 1.1 Type特点

1. TypeScript有微软开发，是基于javaScript的一个扩展语言
2. TypeScript包含了JavaScript的所有内容，即TypeScript是JavaScript的超集
3. TypeScript增加了：静态类型检查、接口、泛型等很多现代开发特性，因此更适合大型项目的开发
4. TypeScript需要编译为JavaScript，然后交给浏览器或其他JavaScript运行环境执行

### 1.2 javaScript中的缺点

* 不清不楚的数据类型

  ```js
  let welcome = 'hello'
  welcome()//此行报错：TypeError：welcome is not a function
  ```

* 有漏洞的逻辑

* 访问不存在的属性

* 低级的拼写错误

静态类型检查：在代码运行之前进行检查，发现代码的错误和不合理之处，减小运行时异常的出现几率，此检查叫静态类型检查

### 1.3 编译TyepScript

浏览器不能直接运行TypeScript代码，需要编译为JavaScript交由浏览器解析执行

#### 1.3.1 命令行编译

要把 .ts ⽂件编译为 .js ⽂件，需要配置 TypeScript 的编译环境，步骤如下：
第⼀步：创建⼀个 demo.ts ⽂件

第⼆步：全局安装 TypeScript：`npm i typescript -g`

第三步：使⽤命令编译 .ts ⽂件： `tsc demo.ts`

#### 1.3.2 自动化编译

第⼀步：创建 TypeScript 编译控制⽂件

`tsc --init`

1. ⼯程中会⽣成⼀个 tsconfig.json 配置⽂件，其中包含着很多编译时的配置。
2. 观察发现，默认编译的 JS 版本是 ES7 ，我们可以⼿动调整为其他版本。

第⼆步：监视⽬录中的 .t

`tsc --watch 或 tsc -w`

## 二、类型

### 2.1 类型声明

使用`: `来对变量或函数形参，进行类型声明：

```typescript
let a: string
let b: number
let c: boolean

a = '9'//只能赋值字符串类型
b = -123
c = true
console.log(a, b, c);


function count(x: number, y: number) {
    return x + y;
}
：

let result = count(31, 31);
console.log(result);
```

```typescript
let b: "hello"//只能存hello，常量
```

### 2.2 类型推断

TS 会根据我们的代码，进⾏类型推导，例如下⾯代码中的变量 d ，只能存储数字

```typescript
let d = -99 //TypeScript会推断出变量d的类型是数字
d = false //警告：不能将类型“boolean”分配给类型“number
```

但要注意，类型推断不是万能的，⾯对复杂类型时推断容易出问题，所以尽量还是明确的编写类
型声明

### 2.3 类型总览

#### 2.3.1 数据类型

JavaScript 中的数据类型：

① string② number③ boolean④ null⑤ undefined⑥ bigint⑦ symbol⑧ object
备注：其中 object 包含： Array 、 Function 、 Date 、 Error 等......

TypeScript中新增的类型

六个新类型：
① any② unknown③ never④ void⑤ tuple⑥ enum

两个用于自定义类型的⽅式：① type② interface

```typescript
let str1:string//ts官方推荐的写法
str1='hello'
// str1=new String('hello');不能存储字符串的包装对象

let str2:String
str2 = 'hello'
str2=new String('hello')//能写字符串的包装对象，对内存不友好
```

1. 原始类型 VS 包装对象

  * 原始类型：如 number 、 string 、 boolean ，在 JavaScript 中是简单数据类型，它们在内存中占⽤空间少，处理速度快。
  * 包装对象：如 Number 对象、 String 对象、 Boolean 对象，是复杂类型，在

  内存中占⽤更多空间，在⽇常开发时很少由开发⼈员⾃⼰创建包装对象。
2. ⾃动装箱：JavaScript 在必要时会⾃动将原始类型包装成对象，以便调⽤⽅法或访
问属性

#### 2.3.1 字符串的装箱

```javascript
// 原始类型字符串
let str = 'hello';
// 当访问str.length时，JavaScript引擎做了以下⼯作：
let size = (function () {
    // 1. ⾃动装箱：创建⼀个临时的String对象包装原始字符串
    let tempStringObject = new String(str);
    // 2. 访问String对象的length属性
    let lengthValue = tempStringObject.length;
    // 3. 销毁临时对象，返回⻓度值
    // （JavaScript引擎⾃动处理对象销毁，开发者⽆感知）
    return lengthValue;
})();
console.log(size); // 输出: 5
```

### 2.4 常用类型

#### 2.4.1 any

any 的含义是：任意类型，⼀旦将变量类型限制为 any ，那就意味着放弃了对该变量的类型检查。

```typescript
let a: any// 明确的表示a的类型是 any —— 显式的any
// 以下对a的赋值，均⽆警告
a = 100
a = '你好'
a = false
let b // 没有明确的表示b的类型是any，但TS主动推断出来b是any —— 隐式的any
//以下对b的赋值，均⽆警告
b = 100
b = '你好'
b = false
```

注意点： any 类型的变量，可以赋值给任意类型的变量

```typescript
/* 注意点：any类型的变量，可以赋值给任意类型的变量 */
let c:any
c = 9
let x: string
x = c // ⽆警告
```

#### 2.4.2 unknown

unknown的含义是未知类型，适⽤于：起初不确定数据的具体类型，要后期才能确定

1. unknown 可以理解为⼀个类型安全的 any 

   ```typescript
   // 设置a的类型为unknown
   let a: unknown
   //以下对a的赋值，均符合规范
   a = 100
   a = false
   a = '你好'
   // 设置x的数据类型为string
   let x: string
   x = a //警告：不能将类型“unknown”分配给类型“string”
   ```

2. unknown 会强制开发者在使⽤之前进行类型检查，从⽽提供更强的类型安全性。

   ```typescript
   let a: unknown
   a = 'hello'
   //第⼀种⽅式：加类型判断
   if(typeof a === 'string'){
    x = a
    console.log(x)
   }
   //第⼆种⽅式：加断⾔
   x = a as string
   //第三种⽅式：加断⾔
   x = <string>a
   ```

3. 读取 any 类型数据的任何属性都不会报错，⽽ unknown 正好与之相反。

   ```typescript
   let str1: string
   str1 = 'hello'
   str1.toUpperCase() //⽆警告
   let str2: any
   str2 = 'hello'
   str2.toUpperCase() //⽆警告
   let str3: unknown
   str3 = 'hello';
   str3.toUpperCase() //警告：“str3”的类型为“未知”
   // 使⽤断⾔强制指定str3的类型为string
   (str3 as string).toUpperCase() //⽆警告
   ```

#### 2.4.3 never

never 的含义是：任何值都不是，即：不能有值，例如 undefined 、 null 、 '' 、 0 都不行！

1. ⼏乎不⽤ never 去直接限制变量，因为没有意义，例如：

   ```typescript
   /* 指定a的类型为never，那就意味着a以后不能存任何的数据了 */
   let a: never
   // 以下对a的所有赋值都会有警告
   a = 1
   a = true
   a = undefined
   a = null
   ```

2. never ⼀般是 TypeScript 主动推断出来的，例如：