## 一、JS介绍

### 1.1 简介

JS是一种运行在客户端（浏览器）的编程语言，实现人机交互的效果

作用：

* 网页特效：监听用户的一些行为让网页做出对应的反馈
* 表单验证：针对表单数据的合法性进行判断
* 数据交互：获取后台的数据，渲染到前端
* 服务端编程：node.js

组成：

* JavaScript语言基础（ECMAScript）：变量、分支语句、循环语句、对象等等
* Web APIs（DOM（网页文档对象模型：对网页元素进行移动、大小等），BOM（浏览器对象模型：页面弹窗，检测窗口宽度））

### 1.2 书写位置

1. 内部JavaScript

   直接写在html文件里，用script标签围住

   规范：script标签写在</body>上面

   注：我们将 <script> 放在HTML文件的底部附近的原因是浏览器会按照代码在文件中的顺序加载 HTML。

   如果先加载的 JavaScript 期望修改其下方的 HTML，那么它可能由于 HTML 尚未被加载而失效。

   因此，将 JavaScript 代码放在 HTML页面的底部附近通常是最好的策略。

2. 外部javaScipt

   代码写在.js结尾的文件里，通过script标签，引入到html页面里

   注：. script标签中间无需写代码，否则会被忽略！

3. 内部JavaScript

   代码写在标签内部

注意：script标签如果用于引入外部js文件，中间最好不要有任何字符

### 1.3 javaScript注释和结束符

单行注释：//

块注释：/* */

结束符：

* 作用： 使用英文的 ; 代表语句结束 
* 实际情况： 实际开发中，可写可不写, 浏览器(JavaScript 引擎) 可以自动推断语句的结束位置

### 1.4 输入输出语法

输出和输入也可理解为人和计算机的交互，用户通过键盘、鼠标等向计算机输入信息，计算机处理后再展示结果给用户，这便是一次输入和输出的过程

3种输出语法：

```javascript
document.write("要输出的内容")

//想body输出内容
//如果输出的内容写的是标签，也会被解析成网页元素
```

```javascript
alert('要出的内容')
//页面弹出警告对话框
```

```javascript
cosole.log('控制台打印')
//控制台输出语法，程序员调试使用
```

输入语法：

```javascript
prompt("please enetr your age")
//显示一个对话框，对话框种包含一条文字信息，用来提示用输入文字
```

代码执行顺序：

* 按HTML文档流顺序执行JavaScript代码
* alert() 和 prompt() 它们会跳过页面渲染先被执行

### 1.5 字面量

字面量（literal）是在计算机中描述事/物

## 二、JS基础语法

### 2.1变量

变量是计算机存储数据的容器

注：变量不是数据本身，它们仅仅是一个用于存储数值的容器。可以理解为是一个个用来装东西的纸箱子。

#### 2.1.1 基本使用

1.声明变量：`let 变量名`

let是关键字，是系统提供的专门用来声明、定义变量的词语

2.变量赋值：`变量名=18`

定义了一个变量后，就能够初始化它（赋值）。在变量名之后跟上一个“=”，然后是数值。

`let age=18`

3.更新变量：变量赋值后，还可以通过简单地给它一个不同的值来更新它

4.声明多个变量：`   let age=123,uname="gaoxin`

#### 2.1.2 变量的本质

内存：计算机中存储数据的地方，相当于一个空间

变量本质：是程序在内存中申请的一块用来存放数据的小空间

#### 2.1.3 var与let区别

let 为了解决 var 的一些问题。

var 声明:

Ø 可以先使用 在声明 (不合理)

Ø var 声明过的变量可以重复声明(不合理)

Ø 比如变量提升、全局变量、没有块级作用域等等

#### 2.1.4 变量声明

变量声明有三个var、let和const

- var：老派写法，问题很多
- const：尽量使用const，很多变量声明的时候我们知道不会在更改，就可以用const
- let：如果后面是要修改的，将const改为let

注：

- 只要对象是引用类型，里面存储的是地址，只要地址不变，就不会报错，所以修改const声明的对象的属性不会报错
- 建议数组和对象使用const来声明

```js
/**
 * 1.js中的变量声明使用var
 * 2.JS是弱类型的语言，在声明的时候不指定类型，赋值时才确定类型，js不是没有类型
 */
```

*  `==` 如果两端的数据类型不一致，就会尝试将两端的数据转换成number在对比
* `===`  如果两端的数据类型不一致，直接返回false，相同则会继续对比

### 2.2 数组

数组是一种将一组数据存储在单个变量名下的优雅方法

> 创建数组的四种方式

- new Array()                                                   创建空数组
- new Array(5)                                                 创建数组时给定长度
- new Array(ele1,ele2,ele3,... ... ,elen);          创建数组时指定元素值
- [ele1,ele2,ele3,... ... ,elen];                           相当于第三种语法的简写

#### 2.2.1 声明语法

```javascript
let arr = [10, 2, 3, , 45, 3]
let name = ["刘德华", "张学友", "郭富城", "黎明"]
console.log(name[0])
```

数组按顺序保存，每个数据都有自己的编号，编号也叫做索引或下标

* 元素：数组中保存的每个数据都叫做数组元素
* 下标：数组中数据的编号
* 长度：数组中数据的个数，通过数组的length属性获得2.2常量

使用const声明的变量称为常量

当某个变量永远不会改变时，就可以使用const来声明，而不是let

注：常量不允许重新赋值，声明的时候必须赋值

#### 2.2.2 数组添加新的数据

* 数组.push():方法将一个或多个元素添加到数组的末尾，并返回该数组的新长度

  `arr.push(elememt1,element2,element3....)`

* 数组.unshift():方法将一个或多个元素添加到数组的开头，并返回该数组的新长度

  `arr.unshift(elememt1,element2,element3....)`

#### 2.2.3 删除数组中的数据

* 数组.pop():方法从数组中删除最后一个元素，并返回该元素的值

* 数组.shift():方法从数组中删除第一个元素，并返回该元素的值

* 数组.splice():方法删除指定的元素

  `arr.splice(start,deleteCount)`

  deleteCount如果不写，则默认从指定的起始位置删除到最后

#### 2.2.4 筛选数组filter方法

filter() 方法创建一个新的数组，新数组中的元素是通过检查指定数组中符合条件的所有元素

主要使用场景： 筛选数组符合条件的元素，并返回筛选之后元素的新数组

```javascript
被遍历的数组.filter(function(currenValue，index){
    return 筛选条件
})
```

返回值：返回数组，包含了符合条件的所有元素。如果没有符合条件的元素则返回空数组

 参数：currentValue 必须写， index 可选

因为返回新数组，所以不会影响原数组

#### 2.2.5 数组map和join方法

数组中的map方法：

map可以遍历数组处理数据，并且返回新的数组

map也称为映射，映射是一个术语，指两个元素的集之间元素相互对应的关系

map重点在于有返回值，forEach没有返回值

数组中的join方法：

join()方法可以把数组中的所有元素转换成一个字符串

数组元素是通过参数里面指定的分隔符进行分隔的，空字符串('')，则所有元素之间都没有任何字符

```html
<script>
  const arr = ['red', 'blue', 'pink']

  //1.数组map方法
  const newArr = arr.map(function (ele, index) {
    // console.log(ele)//数组元素
    // console.log(index);//索引号
    return ele + '颜色'
  })
  console.log(newArr);// ['red颜色', 'blue颜色', 'pink颜色']

  //2.数组join方法
  //小括号为空则逗号分隔
  console.log(newArr.join())// red颜色,blue颜色,pink颜色
  console.log(newArr.join(''))//red颜色blue颜色pink颜色
</script>
```

#### 2.2.6 遍历数组forEach方法

forEach()方法用于调用数组的每个元素，并将元素传递给回调函数，不返回值，适用于遍历对象数组

主要使用场景：遍历数组的每个元素

语法：

```javascript
被遍历的数组.forEach(function(当前数组对象，当前数组索引号){
    //函数体
})
```

实例：

```html
<script>
  const arr = ['red', 'blue', 'green', 'yellow']
  arr.forEach((item, index) => {
    console.log(item);//数组元素
    console.log(index);//索引号
  })
</script>
```

#### 2.2.7 常用方法

- 在JS中,数组属于Object类型,其长度是可以变化的,更像JAVA中的集合

| 方法                                                         | 描述                                                         |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| [concat()](https://www.runoob.com/jsref/jsref-concat-array.html) | 连接两个或更多的数组，并返回结果。                           |
| [copyWithin()](https://www.runoob.com/jsref/jsref-copywithin.html) | 从数组的指定位置拷贝元素到数组的另一个指定位置中。           |
| [entries()](https://www.runoob.com/jsref/jsref-entries.html) | 返回数组的可迭代对象。                                       |
| [every()](https://www.runoob.com/jsref/jsref-every.html)     | 检测数值元素的每个元素是否都符合条件。                       |
| [fill()](https://www.runoob.com/jsref/jsref-fill.html)       | 使用一个固定值来填充数组。                                   |
| [filter()](https://www.runoob.com/jsref/jsref-filter.html)   | 检测数值元素，并返回符合条件所有元素的数组。                 |
| [find()](https://www.runoob.com/jsref/jsref-find.html)       | 返回符合传入测试（函数）条件的数组元素。                     |
| [findIndex()](https://www.runoob.com/jsref/jsref-findindex.html) | 返回符合传入测试（函数）条件的数组元素索引。                 |
| [forEach()](https://www.runoob.com/jsref/jsref-foreach.html) | 数组每个元素都执行一次回调函数。                             |
| [from()](https://www.runoob.com/jsref/jsref-from.html)       | 通过给定的对象中创建一个数组。                               |
| [includes()](https://www.runoob.com/jsref/jsref-includes.html) | 判断一个数组是否包含一个指定的值。                           |
| [indexOf()](https://www.runoob.com/jsref/jsref-indexof-array.html) | 搜索数组中的元素，并返回它所在的位置。                       |
| [isArray()](https://www.runoob.com/jsref/jsref-isarray.html) | 判断对象是否为数组。                                         |
| [join()](https://www.runoob.com/jsref/jsref-join.html)       | 把数组的所有元素放入一个字符串。                             |
| [keys()](https://www.runoob.com/jsref/jsref-keys.html)       | 返回数组的可迭代对象，包含原始数组的键(key)。                |
| [lastIndexOf()](https://www.runoob.com/jsref/jsref-lastindexof-array.html) | 搜索数组中的元素，并返回它最后出现的位置。                   |
| [map()](https://www.runoob.com/jsref/jsref-map.html)         | 通过指定函数处理数组的每个元素，并返回处理后的数组。         |
| [pop()](https://www.runoob.com/jsref/jsref-pop.html)         | 删除数组的最后一个元素并返回删除的元素。                     |
| [push()](https://www.runoob.com/jsref/jsref-push.html)       | 向数组的末尾添加一个或更多元素，并返回新的长度。             |
| [reduce()](https://www.runoob.com/jsref/jsref-reduce.html)   | 将数组元素计算为一个值（从左到右）。                         |
| [reduceRight()](https://www.runoob.com/jsref/jsref-reduceright.html) | 将数组元素计算为一个值（从右到左）。                         |
| [reverse()](https://www.runoob.com/jsref/jsref-reverse.html) | 反转数组的元素顺序。                                         |
| [shift()](https://www.runoob.com/jsref/jsref-shift.html)     | 删除并返回数组的第一个元素。                                 |
| [slice()](https://www.runoob.com/jsref/jsref-slice-array.html) | 选取数组的一部分，并返回一个新数组。                         |
| [some()](https://www.runoob.com/jsref/jsref-some.html)       | 检测数组元素中是否有元素符合指定条件。                       |
| [sort()](https://www.runoob.com/jsref/jsref-sort.html)       | 对数组的元素进行排序。                                       |
| [splice()](https://www.runoob.com/jsref/jsref-splice.html)   | 从数组中添加或删除元素。                                     |
| [toString()](https://www.runoob.com/jsref/jsref-tostring-array.html) | 把数组转换为字符串，并返回结果。                             |
| [unshift()](https://www.runoob.com/jsref/jsref-unshift.html) | 向数组的开头添加一个或更多元素，并返回新的长度。             |
| [valueOf()](https://www.runoob.com/jsref/jsref-valueof-array.html) | 返回数组对象的原始值。                                       |
| [Array.of()](https://www.runoob.com/jsref/jsref-of-array.html) | 将一组值转换为数组。                                         |
| [Array.at()](https://www.runoob.com/jsref/jsref-at-array.html) | 用于接收一个整数值并返回该索引对应的元素，允许正数和负数。负整数从数组中的最后一个元素开始倒数。 |
| [Array.flat()](https://www.runoob.com/jsref/jsref-flat-array.html) | 创建一个新数组，这个新数组由原数组中的每个元素都调用一次提供的函数后的返回值组成。 |
| [Array.flatMap()](https://www.runoob.com/jsref/jsref-flatmap-array.html) | 使用映射函数映射每个元素，然后将结果压缩成一个新数组。       |

### 2.3数据类型

整体分为两大类：基本数据类型、应用类型

#### 2.3.1 数值类型

即我们数学中学习到的数字，可以是整数、小数、正数、负数

引用数据类型：object

* NaN代表一个计算错误，它是一个不正确的或者一个未定义的数学操作所得到的结果
* NaN是粘性的，任何对NaN的操作都会返回NaN

#### 2.3.2 字符串类型

通过单引号（ `''`） 、双引号（ `""`）或反引号包裹的数据都叫字符串，单引号和双引号没有本质上的区别，推荐使用单引号。

注意事项：

1. 无论单引号或是双引号必须成对使用
2. 单引号/双引号可以互相嵌套，但是不以自已嵌套自已
3. 必要时可以使用转义符 `\`，输出单引号或双引号

字符串拼接：可以用加号实现

模板字符串：

```javascript
let age = 11
document.write(`我今年${age}岁`)

//我今年11岁
```

#### 2.3.3 布尔类型，未定义类型，null

1. 表示肯定或否定时在计算机中对应的是布尔类型数据，它有两个固定的值 `true` 和 `false`，表示肯定的数据用 `true`，表示否定的数据用 `false`。
2. 未定义是比较特殊的类型，只有一个值 undefined，只声明变量，不赋值的情况下，变量的默认值为 undefined，一般很少【直接】为某个变量赋值为 undefined。
3. null仅仅是一个代表”无“、”空“或”值未知“的特殊值,使用typeof检测类型时，它的结果为object

#### 2.3.4 检测数据类型

typeof运算符可以返回被检测的数据类型，支持两种语法：

1. 作为运算符：typeof x(常用的写法)
2. 函数形式：typeof(x)

### 2.4 类型转换

JavaScript是弱数据类型：JavaScript也不知道变量到底属于那种数据类型，只有赋值了才清楚；

使用表单、prompt获取过来的数据默认是字符串类型，此时不能直接简单的进行加法运算

类型转换就是把数据类型的变量转换成我们需要的数据类型

#### 2.4.1 隐式转换

某些运算被执行时，系统内部自动将数据类型进行转换，这就是隐式转换

规则：

* +号只要两边有一个是字符串，都会把另外一个转换成字符串
* 除了+以外的算数运算符，比如-*/都会把数据转换成数字类型

缺点：

* 转换类型不明确

小技巧：

* +号作为正号解析可以转换成数字类型
* 任何数据和字符串相加结果都是字符串

#### 2.4.2 显示转换

告诉系统该转换成什么类型

转换为数字类型：

* Number（数据）

  转成数字类型

  如果字符串内容里有非数字，转换失败时结果为NaN（Not a Number）既不是一个数字

  NaN也是number类型的数据

* parseInt（数据）

  只保留整数

* parseFloat（数据）

  可以保留小数


转换为boolean型：

* Boolean(内容)

  `''、0、undefined、null、false、NaN`转换为布尔值后都是false，其余则为true

  1. 有字符串的加法" "+1，结果是"1"
  2. 减法"-"只能用于数字，它会使空字符串""转换为0
  3. null经过数字转换会变为0，undefined经过数字转换之后会变为NaN

### 2.5 简单数据类型与应用数据类型

#### 2.5.1 概述

- 基本数据类型(又叫做简单类型或者值类型)：number,string,boolean,undefined,null

  在存储时变量中存储的是值本身

- 应用类型(复杂类型)：通过new关键字创建的对象（系统对象、自定义对象），如Object、Array、Date等

  在存储变量时中存储的仅仅是地址（引用）

#### 2.5.2 堆栈空间分配区别

1. 栈（操作系统）：由操作系统自动分配释放存放函数的参数值、局部变量等。其操作方式类似于数据结构的栈；简单数据类型存放到栈里面

2. 堆（操作系统）：存储复杂类型（对象），一般由程序员分配释放，若程序员不释放，由垃圾回收机制回收；

   引用数据变量（栈里面）存放的是地址，真正的引用数据类型对象存放到堆里面




## 三、语句

- 表达式：表达式是可以被求值的代码，JavaScript引擎会将其计算出一个结果
- 语句：语句是可以执行的一段代码，语句不一定有值

### 3.1 运算符

===：左右两边是否类型和值都相等，开发中判断是否相等，推荐=================

！==：左右两边是否不全等

运算符优先级：

| 优先级 | 运算符     | 顺序            |
| ------ | ---------- | --------------- |
| 1      | 小括号     | `()`            |
| 2      | 一元运算符 | `++ -- !`       |
| 3      | 算术运算符 | `先* / %后+ -`  |
| 4      | 关系运算符 | `> >= < <=`     |
| 5      | 相等运算符 | `== != === !==` |
| 6      | 逻辑运算符 | `先&& 后||`     |
| 7      | 赋值运算符 | `=`             |
| 8      | 逗号运算符 | `,`             |

### 3.2 分支语句

分支语句可以让我们有选择性的执行想要的代码

#### 3.2.1 if分支语句

```javascript
if(条件){
    满足条件要执行的代码
}
```

#### 3.2.2 三元运算符

使用场景：比if双分支更简单的写法，可以使用三元运算符

```javascript
条件？满足条件执行的代码：不满足条件执行的代码
```

#### 3.2.3 switch分支语句

```javascript
switch(数据){
    case 值1:
        代码1
        break
    case 值1:
   	    代码1
   	    break
    default:
        代码n
        break
}
```

若没有全等===，则执行default里的代码

**if 多分支语句和 switch的区别：**

1. 共同点
   - 都能实现多分支选择， 多选1 
   - 大部分情况下可以互换
2. 区别：
   - switch…case语句通常处理case为比较确定值的情况，而if…else…语句更加灵活，通常用于范围判断(大于，等于某个范围)。
   - switch 语句进行判断后直接执行到程序的语句，效率更高，而if…else语句有几种判断条件，就得判断多少次
   - switch 一定要注意 必须是 ===  全等，一定注意 数据类型，同时注意break否则会有穿透效果
   - 结论：
     - 当分支比较少时，if…else语句执行效率高。
     - 当分支比较多时，switch语句执行效率高，而且结构更清晰。

### 3.3 循环语句

#### 3.3.1 while循环

```javascript
while(循环条件){
    循环体
}
```

#### 3.3.2 for循环

```javascript
for(变量起始值;终止条件;变量变化量){
    循环体
}
 for (var i in arr) {
           document.write("<li>" + arr[i] + "</li>")
 }
```

## 四、函数

和java相比有如下特点：

1. 没有访问修饰符
2. 没有返回值类型也没有void，如果有值要返回，则直接return即可
3. 没有异常列表
4. 调用方法时，实参和形参可以在数量上不一致，在方法内部可以通过arguments获得调用是的实参
5. 函数也可以作为参数传递给另一种方法

### 4.1 函数使用

函数：function，被设计为执行特定任务的代码块

说明：函数可以把具有相同或相似逻辑的代码“包裹”起来，通过函数调用执行这些别“包裹”的代码逻辑，有利于精简函数代码使用

函数声明语法：

```javascript
function 函数名(){
    函数体
}
```

 函数命名建议：

| 动词 | 含义                   |
| ---- | ---------------------- |
| can  | 判断是否可执行某个动作 |
| has  | 判断是否含某个值       |
| is   | 判断是否为某个值       |
| get  | 获取某个值             |
| set  | 设置某个值             |
| load | 加载某些值             |

函数调用语法：

```javascript
函数名()
```

* 声明的的函数必须调用才会被执行，使用()调用函数
* 两个相同的函数后面的会覆盖前面的函数

### 4.2 函数传参

```javascript
function 函数名(参数列表){
    函数体
}
```

形参：声明函数时写在函数名右边小括号里的叫形参

实参：调用函数时写在函数名右边小括号里的叫实参

* 如果用户不输入实参，就会出现undefined+undefined，结果为NaN

* 在Javascript中，实参的个数和形参的个数可以不一致

  如果形参过多，会自动填上undefined

  如果实参过多，那么多余的实参会被忽略（函数内部有一个arguments，里面装着所有实参）

### 4.3 函数返回值return

当函数需要返回数据出去时，用return关键字

```javascript
function 函数名(参数列表){
    函数体
    return 数据
}
```

细节：

* 在函数体中使用return关键字能将内部的执行结果交给函数外部使用
* return后面代码不会在被执行，会立即结束当前函数，所以return后面的数据不要拿换行写
* return函数可以没有return，这种情况函数默认返回值为undefined


### 4.4 匿名函数

没有名字的函数，无法直接使用

使用方式：

* 函数表达式

  将匿名函数赋值给一个变量，并且通过变量名称进行调用，这个称之为==函数表达式==

   函数表达式和具名函数的不同：具名函数的调用可以写在任何位置，函数表达式必须先写表达式后调用

* 立即执行函数

  使用场景：避免全局变量的污染

  ```javascript
  //第一种写法
  (function () {
    console.log(11)//函数体
  }
  )();

  //第二种写法
  (function () {
    console.log(11)//函数体
  }
  ());
  ```

  多个执行函数要用分号来分隔开，要不然会报错

### 4.5 逻辑中断

逻辑运算符里的短路

短路：只存在于&&和||中，当满足一定条件会让右边的代码不执行

| 符号 | 短路条件          |
| ---- | ----------------- |
| &&   | 左边为false就短路 |
| \|\| | 左边为true就短路  |

原因：通过左边就能得到整个式子的结果，因此没必要在判断右边

运算结果：无论是&&还是||，运算结果都是最后被执行的表达式求值，一般用在变量赋值

## 五、对象 

对象是一种数据类型，可以理解为一种无序的数据集合，而数组是有序的数据集合，可以详细的描述某个事物

### 5.1 对象基本使用

1.对象声明语法：

`let 对象名={}`

`let 对象名=new Object()`

2.对象由属性和方法组成

属性：信息或叫特征（名词）

* 属性都是成对出现的，包括属性名和值， 它们之间使用英文：分隔
* 多个属性之间使用英文”,“分隔
* 属性就是依附于对象上的变量

方法：功能或叫行为（动词）

```javascript
let 对象名={
	属性名：属性值,
    方法名：函数 
}

// 创建对象的语法
var person = new Object();
person.name = '高鑫';
person.age = 10;
person.eat = function(food) {
    console.log(this.age + "岁" + this.name + "正在吃" + food);
}
// 访问属性
console.log(person.name);
console.log(person.age);
// 访问方法
person.eat("炒饭")
```

### 5.2 对象的操作

#### 5.2.1 查询对象

声明对象，并添加了若干属性后，可以使用"."获得对象中属性对应的值，称之为属性访问

语法：`对象名.属性`

查的另外一种属性

语法：`对象名['属性']`    属性名必须加引号

```javascript
let obj = {
  'goods-name': '小米',
  num: '100',
  weight: '0.22kg',
  address: '中国大陆'
}
obj.address = 'Amercian'
console.log(obj.address)//Amercian

console.log(obj['goods-name']);//小米
console.log(obj['num']);//100
```

#### 5.2.2 修改对象

语法：`对象名.属性=新值`

#### 5.2.3  增加对象

语法：`对象名.新属性=新值`

#### 5.2.4 删除对象 

语法:`delete 对象名.属性`

### 5.3 遍历对象

```javascript
 let obj = {
   'goods-name': '小米',
   num: '100',
   weight: '0.22kg',
   address: '中国大陆'
 }
  for (let i in obj) {//i是带着引号的属性名
   console.log(i + ':')
   console.log(obj[i])
 }

//goods-name:
//小米
//num:
//100
//weight:
//0.22kg
//address:
//中国大陆
```

### 5.4 内置对象-Math

JavaScript内部提供的对象，包含各种属性和方法者供开发者调

| 方法名          | 说明                      |
| :-------------- | ------------------------- |
| abs(a)          | 获取参数的绝对值          |
| ceil(b)         | 向上取整                  |
| floor(a)        | 向下取整                  |
| min(a, b)       | 获取两个值的较小值        |
| max(a, b)       | 获取两个值的较大值        |
| pow( a, b)      | 返回a的b次幂              |
| double random() | 返回值为随机值，范围[0,1) |

### 5.5 术语解释

| 术语           | 解释                                       | 举例                                                    |
| -------------- | ------------------------------------------ | ------------------------------------------------------- |
| 关键字         | 在JavaScript中有特殊意义的词汇             | let,var,function,if,else,switch,for,case,break,continue |
| 保留字         | 在目前没有意义，但在未来可能具有特殊的意义 | int,short,long,char                                     |
| 标识（标识符） | 变量名、函数名的另一种叫法                 | 无                                                      |
| 表达式         | 能产生值的代码，一般配合运算符出现         | 10+3，age>=19                                           |
| 语句           | 一段可执行的代码                           | if(),for()                                              |

### 5.6 JSON

#### 5.6.1 概念

JSON（JavaScript Object Notation, JS对象简谱）是一种轻量级的数据交换格式。它基于ECMAScript（European Computer Manufacturers Association, 欧洲计算机协会的一个子集，采用完全独立于编程语言的文本格式来存储和表示数据。简洁和清晰的层次结构使得 JSON 成为理想的数据交换语言。 易于人阅读和编写，同时也易于机器解析和生成，并有效地提升网络传输效率 

简单来说,JSON 就是一种字符串格式,这种格式无论是在前端还是在后端,都可以很容易的转换成对象,所以常用于前后端数据传递

说明：

- JSON的语法

  ​		var obj="{'属性名':'属性值','属性名':{'属性名':'属性值'},'属性名':['值1','值1','值3']}"

- JSON字符串一般用于传递数据,所以字符串中的函数就显得没有意义,在此不做研究

- 通过JSON.parse()方法可以将一个JSON串转换成对象

- 通过JSON.stringify()方法可以将一个对象转换成一个JSON格式的字符串

#### 5.6.2 JSON在客户端的使用

```js
 /**
         * JSON格式的语法
         * var personStr = '{"属性名":"属性值"}'
         * 属性名必须用""包裹上，数字可以不处理
         */ 
        // 这是JSON格式的字符串
        var personStr = '{"name":"gaoxin","age":"10","dog":{"dname":"xiaohua"},"loveStars":["蔡徐坤","马嘉祺","范丞丞"],"friends":[{"fname":"宇智波赵四"},{"fanme":"新野王五"},{"fname":"张山小次郎"}]}'
        console.log(personStr);
        console.log(typeof personStr);
        console.log(personStr.name);
        // 通过JSON.parse()将字符串转换成对象
        var person = JSON.parse(personStr)
        console.log(person);
        console.log(typeof person);
        console.log(person.name)
        console.log(person.friends[0].fname);
        // 通过JSON.stringify()将一个对象转换为JSON串
        console.log(JSON.stringify(person));
```

## 六、js常见对象

### 6.1 Boolean对象

> boolean对象的方法比较简单

| 方法                                                         | 描述                               |
| :----------------------------------------------------------- | :--------------------------------- |
| [toString()](https://www.runoob.com/jsref/jsref-tostring-boolean.html) | 把布尔值转换为字符串，并返回结果。 |
| [valueOf()](https://www.runoob.com/jsref/jsref-valueof-boolean.html) | 返回 Boolean 对象的原始值。        |

### 6.2 Date对象

> 和JAVA中的Date类比较类似

| 方法                                                         | 描述                                                         |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| [getDate()](https://www.runoob.com/jsref/jsref-getdate.html) | 从 Date 对象返回一个月中的某一天 (1 ~ 31)。                  |
| [getDay()](https://www.runoob.com/jsref/jsref-getday.html)   | 从 Date 对象返回一周中的某一天 (0 ~ 6)。                     |
| [getFullYear()](https://www.runoob.com/jsref/jsref-getfullyear.html) | 从 Date 对象以四位数字返回年份。                             |
| [getHours()](https://www.runoob.com/jsref/jsref-gethours.html) | 返回 Date 对象的小时 (0 ~ 23)。                              |
| [getMilliseconds()](https://www.runoob.com/jsref/jsref-getmilliseconds.html) | 返回 Date 对象的毫秒(0 ~ 999)。                              |
| [getMinutes()](https://www.runoob.com/jsref/jsref-getminutes.html) | 返回 Date 对象的分钟 (0 ~ 59)。                              |
| [getMonth()](https://www.runoob.com/jsref/jsref-getmonth.html) | 从 Date 对象返回月份 (0 ~ 11)。                              |
| [getSeconds()](https://www.runoob.com/jsref/jsref-getseconds.html) | 返回 Date 对象的秒数 (0 ~ 59)。                              |
| [getTime()](https://www.runoob.com/jsref/jsref-gettime.html) | 返回 1970 年 1 月 1 日至今的毫秒数。                         |
| [getTimezoneOffset()](https://www.runoob.com/jsref/jsref-gettimezoneoffset.html) | 返回本地时间与格林威治标准时间 (GMT) 的分钟差。              |
| [getUTCDate()](https://www.runoob.com/jsref/jsref-getutcdate.html) | 根据世界时从 Date 对象返回月中的一天 (1 ~ 31)。              |
| [getUTCDay()](https://www.runoob.com/jsref/jsref-getutcday.html) | 根据世界时从 Date 对象返回周中的一天 (0 ~ 6)。               |
| [getUTCFullYear()](https://www.runoob.com/jsref/jsref-getutcfullyear.html) | 根据世界时从 Date 对象返回四位数的年份。                     |
| [getUTCHours()](https://www.runoob.com/jsref/jsref-getutchours.html) | 根据世界时返回 Date 对象的小时 (0 ~ 23)。                    |
| [getUTCMilliseconds()](https://www.runoob.com/jsref/jsref-getutcmilliseconds.html) | 根据世界时返回 Date 对象的毫秒(0 ~ 999)。                    |
| [getUTCMinutes()](https://www.runoob.com/jsref/jsref-getutcminutes.html) | 根据世界时返回 Date 对象的分钟 (0 ~ 59)。                    |
| [getUTCMonth()](https://www.runoob.com/jsref/jsref-getutcmonth.html) | 根据世界时从 Date 对象返回月份 (0 ~ 11)。                    |
| [getUTCSeconds()](https://www.runoob.com/jsref/jsref-getutcseconds.html) | 根据世界时返回 Date 对象的秒钟 (0 ~ 59)。                    |
| getYear()                                                    | 已废弃。 请使用 getFullYear() 方法代替。                     |
| [parse()](https://www.runoob.com/jsref/jsref-parse.html)     | 返回1970年1月1日午夜到指定日期（字符串）的毫秒数。           |
| [setDate()](https://www.runoob.com/jsref/jsref-setdate.html) | 设置 Date 对象中月的某一天 (1 ~ 31)。                        |
| [setFullYear()](https://www.runoob.com/jsref/jsref-setfullyear.html) | 设置 Date 对象中的年份（四位数字）。                         |
| [setHours()](https://www.runoob.com/jsref/jsref-sethours.html) | 设置 Date 对象中的小时 (0 ~ 23)。                            |
| [setMilliseconds()](https://www.runoob.com/jsref/jsref-setmilliseconds.html) | 设置 Date 对象中的毫秒 (0 ~ 999)。                           |
| [setMinutes()](https://www.runoob.com/jsref/jsref-setminutes.html) | 设置 Date 对象中的分钟 (0 ~ 59)。                            |
| [setMonth()](https://www.runoob.com/jsref/jsref-setmonth.html) | 设置 Date 对象中月份 (0 ~ 11)。                              |
| [setSeconds()](https://www.runoob.com/jsref/jsref-setseconds.html) | 设置 Date 对象中的秒钟 (0 ~ 59)。                            |
| [setTime()](https://www.runoob.com/jsref/jsref-settime.html) | setTime() 方法以毫秒设置 Date 对象。                         |
| [setUTCDate()](https://www.runoob.com/jsref/jsref-setutcdate.html) | 根据世界时设置 Date 对象中月份的一天 (1 ~ 31)。              |
| [setUTCFullYear()](https://www.runoob.com/jsref/jsref-setutcfullyear.html) | 根据世界时设置 Date 对象中的年份（四位数字）。               |
| [setUTCHours()](https://www.runoob.com/jsref/jsref-setutchours.html) | 根据世界时设置 Date 对象中的小时 (0 ~ 23)。                  |
| [setUTCMilliseconds()](https://www.runoob.com/jsref/jsref-setutcmilliseconds.html) | 根据世界时设置 Date 对象中的毫秒 (0 ~ 999)。                 |
| [setUTCMinutes()](https://www.runoob.com/jsref/jsref-setutcminutes.html) | 根据世界时设置 Date 对象中的分钟 (0 ~ 59)。                  |
| [setUTCMonth()](https://www.runoob.com/jsref/jsref-setutcmonth.html) | 根据世界时设置 Date 对象中的月份 (0 ~ 11)。                  |
| [setUTCSeconds()](https://www.runoob.com/jsref/jsref-setutcseconds.html) | setUTCSeconds() 方法用于根据世界时 (UTC) 设置指定时间的秒字段。 |
| setYear()                                                    | 已废弃。请使用 setFullYear() 方法代替。                      |
| [toDateString()](https://www.runoob.com/jsref/jsref-todatestring.html) | 把 Date 对象的日期部分转换为字符串。                         |
| toGMTString()                                                | 已废弃。请使用 toUTCString() 方法代替。                      |
| [toISOString()](https://www.runoob.com/jsref/jsref-toisostring.html) | 使用 ISO 标准返回字符串的日期格式。                          |
| [toJSON()](https://www.runoob.com/jsref/jsref-tojson.html)   | 以 JSON 数据格式返回日期字符串。                             |
| [toLocaleDateString()](https://www.runoob.com/jsref/jsref-tolocaledatestring.html) | 根据本地时间格式，把 Date 对象的日期部分转换为字符串。       |
| [toLocaleTimeString()](https://www.runoob.com/jsref/jsref-tolocaletimestring.html) | 根据本地时间格式，把 Date 对象的时间部分转换为字符串。       |
| [toLocaleString()](https://www.runoob.com/jsref/jsref-tolocalestring.html) | 根据本地时间格式，把 Date 对象转换为字符串。                 |
| [toString()](https://www.runoob.com/jsref/jsref-tostring-date.html) | 把 Date 对象转换为字符串。                                   |
| [toTimeString()](https://www.runoob.com/jsref/jsref-totimestring.html) | 把 Date 对象的时间部分转换为字符串。                         |
| [toUTCString()](https://www.runoob.com/jsref/jsref-toutcstring.html) | 根据世界时，把 Date 对象转换为字符串。实例：`var today = new Date(); var UTCstring = today.toUTCString();` |
| [UTC()](https://www.runoob.com/jsref/jsref-utc.html)         | 根据世界时返回 1970 年 1 月 1 日 到指定日期的毫秒数。        |
| [valueOf()](https://www.runoob.com/jsref/jsref-valueof-date.html) | 返回 Date 对象的原始值。                                     |

### 6.3 Math

> 和JAVA中的Math类比较类似

| 方法                                                         | 描述                                                         |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| [abs(x)](https://www.runoob.com/jsref/jsref-abs.html)        | 返回 x 的绝对值。                                            |
| [acos(x)](https://www.runoob.com/jsref/jsref-acos.html)      | 返回 x 的反余弦值。                                          |
| [asin(x)](https://www.runoob.com/jsref/jsref-asin.html)      | 返回 x 的反正弦值。                                          |
| [atan(x)](https://www.runoob.com/jsref/jsref-atan.html)      | 以介于 -PI/2 与 PI/2 弧度之间的数值来返回 x 的反正切值。     |
| [atan2(y,x)](https://www.runoob.com/jsref/jsref-atan2.html)  | 返回从 x 轴到点 (x,y) 的角度（介于 -PI/2 与 PI/2 弧度之间）。 |
| [ceil(x)](https://www.runoob.com/jsref/jsref-ceil.html)      | 对数进行上舍入。                                             |
| [cos(x)](https://www.runoob.com/jsref/jsref-cos.html)        | 返回数的余弦。                                               |
| [exp(x)](https://www.runoob.com/jsref/jsref-exp.html)        | 返回 Ex 的指数。                                             |
| [floor(x)](https://www.runoob.com/jsref/jsref-floor.html)    | 对 x 进行下舍入。                                            |
| [log(x)](https://www.runoob.com/jsref/jsref-log.html)        | 返回数的自然对数（底为e）。                                  |
| [max(x,y,z,...,n)](https://www.runoob.com/jsref/jsref-max.html) | 返回 x,y,z,...,n 中的最高值。                                |
| [min(x,y,z,...,n)](https://www.runoob.com/jsref/jsref-min.html) | 返回 x,y,z,...,n中的最低值。                                 |
| [pow(x,y)](https://www.runoob.com/jsref/jsref-pow.html)      | 返回 x 的 y 次幂。                                           |
| [random()](https://www.runoob.com/jsref/jsref-random.html)   | 返回 0 ~ 1 之间的随机数。                                    |
| [round(x)](https://www.runoob.com/jsref/jsref-round.html)    | 四舍五入。                                                   |
| [sin(x)](https://www.runoob.com/jsref/jsref-sin.html)        | 返回数的正弦。                                               |
| [sqrt(x)](https://www.runoob.com/jsref/jsref-sqrt.html)      | 返回数的平方根。                                             |
| [tan(x)](https://www.runoob.com/jsref/jsref-tan.html)        | 返回角的正切。                                               |
| [tanh(x)](https://www.runoob.com/jsref/jsref-tanh.html)      | 返回一个数的双曲正切函数值。                                 |
| [trunc(x)](https://www.runoob.com/jsref/jsref-trunc.html)    | 将数字的小数部分去掉，只保留整数部分。                       |

### 6.4 Number

> Number中准备了一些基础的数据处理函数

| 方法                                                         | 描述                                                 |
| :----------------------------------------------------------- | :--------------------------------------------------- |
| [isFinite](https://www.runoob.com/jsref/jsref-isfinite-number.html) | 检测指定参数是否为无穷大。                           |
| [isInteger](https://www.runoob.com/jsref/jsref-isinteger-number.html) | 检测指定参数是否为整数。                             |
| [isNaN](https://www.runoob.com/jsref/jsref-isnan-number.html) | 检测指定参数是否为 NaN。                             |
| [isSafeInteger](https://www.runoob.com/jsref/jsref-issafeInteger-number.html) | 检测指定参数是否为安全整数。                         |
| [toExponential(x)](https://www.runoob.com/jsref/jsref-toexponential.html) | 把对象的值转换为指数计数法。                         |
| [toFixed(x)](https://www.runoob.com/jsref/jsref-tofixed.html) | 把数字转换为字符串，结果的小数点后有指定位数的数字。 |
| [toLocaleString(locales, options)](https://www.runoob.com/jsref/jsref-tolocalestring-number.html) | 返回数字在特定语言环境下的表示字符串。               |
| [toPrecision(x)](https://www.runoob.com/jsref/jsref-toprecision.html) | 把数字格式化为指定的长度。                           |
| [toString()](https://www.runoob.com/jsref/jsref-tostring-number.html) | 把数字转换为字符串，使用指定的基数。                 |
| [valueOf()](https://www.runoob.com/jsref/jsref-valueof-number.html) | 返回一个 Number 对象的基本数字值。                   |

### 6.5 String

> 和JAVA中的String类似

| 方法                                                         | 描述                                                         |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| [charAt()](https://www.runoob.com/jsref/jsref-charat.html)   | 返回在指定位置的字符。                                       |
| [charCodeAt()](https://www.runoob.com/jsref/jsref-charcodeat.html) | 返回在指定的位置的字符的 Unicode 编码。                      |
| [concat()](https://www.runoob.com/jsref/jsref-concat-string.html) | 连接两个或更多字符串，并返回新的字符串。                     |
| [endsWith()](https://www.runoob.com/jsref/jsref-endswith.html) | 判断当前字符串是否是以指定的子字符串结尾的（区分大小写）。   |
| [fromCharCode()](https://www.runoob.com/jsref/jsref-fromcharcode.html) | 将 Unicode 编码转为字符。                                    |
| [indexOf()](https://www.runoob.com/jsref/jsref-indexof.html) | 返回某个指定的字符串值在字符串中首次出现的位置。             |
| [includes()](https://www.runoob.com/jsref/jsref-string-includes.html) | 查找字符串中是否包含指定的子字符串。                         |
| [lastIndexOf()](https://www.runoob.com/jsref/jsref-lastindexof.html) | 从后向前搜索字符串，并从起始位置（0）开始计算返回字符串最后出现的位置。 |
| [match()](https://www.runoob.com/jsref/jsref-match.html)     | 查找找到一个或多个正则表达式的匹配。                         |
| [repeat()](https://www.runoob.com/jsref/jsref-repeat.html)   | 复制字符串指定次数，并将它们连接在一起返回。                 |
| [replace()](https://www.runoob.com/jsref/jsref-replace.html) | 在字符串中查找匹配的子串，并替换与正则表达式匹配的子串。     |
| [replaceAll()](https://www.runoob.com/jsref/jsref-replaceall.html) | 在字符串中查找匹配的子串，并替换与正则表达式匹配的所有子串。 |
| [search()](https://www.runoob.com/jsref/jsref-search.html)   | 查找与正则表达式相匹配的值。                                 |
| [slice()](https://www.runoob.com/jsref/jsref-slice-string.html) | 提取字符串的片断，并在新的字符串中返回被提取的部分。         |
| [split()](https://www.runoob.com/jsref/jsref-split.html)     | 把字符串分割为字符串数组。                                   |
| [startsWith()](https://www.runoob.com/jsref/jsref-startswith.html) | 查看字符串是否以指定的子字符串开头。                         |
| [substr()](https://www.runoob.com/jsref/jsref-substr.html)   | 从起始索引号提取字符串中指定数目的字符。                     |
| [substring()](https://www.runoob.com/jsref/jsref-substring.html) | 提取字符串中两个指定的索引号之间的字符。                     |
| [toLowerCase()](https://www.runoob.com/jsref/jsref-tolowercase.html) | 把字符串转换为小写。                                         |
| [toUpperCase()](https://www.runoob.com/jsref/jsref-touppercase.html) | 把字符串转换为大写。                                         |
| [trim()](https://www.runoob.com/jsref/jsref-trim.html)       | 去除字符串两边的空白。                                       |
| [toLocaleLowerCase()](https://www.runoob.com/jsref/jsref-tolocalelowercase.html) | 根据本地主机的语言环境把字符串转换为小写。                   |
| [toLocaleUpperCase()](https://www.runoob.com/jsref/jsref-tolocaleuppercase.html) | 根据本地主机的语言环境把字符串转换为大写。                   |
| [valueOf()](https://www.runoob.com/jsref/jsref-valueof-string.html) | 返回某个字符串对象的原始值。                                 |
| [toString()](https://www.runoob.com/jsref/jsref-tostring.html) | 返回一个字符串。                                             |