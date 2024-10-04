## 一、作用域&解构&箭头函数

### 1.1 作用域

作用域规定了变量能够访问的“范围”，离开了这个“范围”变量便不能被访问

作用域分为：

* 局部作用域
* 全局作用域

#### 1.1.1 局部作用域

局部作用域分为函数作用域和块作用域

* 函数作用域

  在函数内部声明的变量只能在函数内部被访问，外部无法直接访问

  1. 函数内部声明的变量，在函数外部无法直接被访问
  2. 函数的参数也是函数内部的局部变量
  3. 不同函数内部声明的变量无法互相访问
  4. 函数执行完毕后，函数内部的变量实际被清空

* 块作用域

  在JavaScript中使用{}包裹的代码称为代码块，代码块内部声明的变量外部==有可能==无法被访问

  1. let声明的变量会产生块作用域，var不会产生块作用域
  2. const声明的常量也会产生块作用域
  3. 不同的代码块之间的常量无法相互访问

#### 1.1.2 全局作用域

<script>标签和.js文件的最外层就是所谓的全局作用域，在此声明的变量在函数内部也可以被访问。全局作用域中的声明的变量，任何其他作用域都可以被访问

注：

1. 为window对象动态添加的属性默认也是全局的
2. 函数中未使用任何关键字声明的变量为全局变量
3. 尽可能少的声明全局变量，防止全局变量被污染

### 1.2 作用域链

作用域链本质上是底层的变量查找机制

* 当函数被执行时，会优先查找当前函数作用域中查找变量
* 如果当前作用域查找不到则会依次逐级查找父级作用域直到全局作用域

总结：

1.嵌套关系传递作用域串联起来形成了作用域链

2.相同作用域链中按着从小到大的规则查找变量

3.子作用域能够访问父作用域，父级作用域无法访问子级作用域

### 1.3 垃圾回收机制

#### 1.3.1垃圾回收机制介绍

JS中内存中分配的内存，一般有如下的生命周期：

1. 内存分配：当我们声明变量、函数、对象的时候，系统会自动为它们分配内存
2. 内存使用：即读写内存，也就是使用变量、函数等
3. 内存回收：使用完毕，由垃圾回收器自动回收不在使用的内存

注：

* 全局变量一般不会回收（关闭页面回收）
* 一般情况下情况变量的值，不用了会自动回收掉

内存泄露：程序中分配的内存由于某种原因程序未释放或无法释放叫做内存泄露

垃圾回收机制算法：

#### 1.3.2 引用计数法

IE采用的引用计数算法，定义“内存不在使用”，就是看一个对象是否有指向它的引用，没有引用了就回收对象

算法：

* 跟踪记录被引用的次数
* 如果被引用了一次，那么就纪录次数1，多次引用会累加
* 如果减少一个引用就减一
* 如果引用次数是0，则释放内存

注：它存在一个致命的问题：嵌套使用

如果两个对象相互引用，尽管它们已不再使用，垃圾回收器不会在进行回收，导致内存泄露

```javascript
function fn(){
    let o1={}
    let o2={}
    o1.a=o2
    o2.a=o1
    return '引用计数无法回收'
}
fn()
```

因为它们的引用次数永远不会是0，这样的相互引用如果说很大量的存在就会导致大量的内存泄露

#### 1.3.3 标记清除法

现代浏览器通用的大多是基于标记清除算法的某些改进算法，总体思想是一致的

核心：

1. 标记清除算法将"不在使用的对象"定义为"无法到达的对象"
2. 就是从根部（在JS中就是全局对象）出发定时扫描内存中的对象，凡是能从根部到达的对象，都是还需要使用的
3. 那些无法由根部出发触及到的对象标记为不在使用，稍后进行回收垃圾回收机制

### 1.4 闭包

概念：一个函数对周围状态的引用捆绑在一起，内层函数中访问到其外层函数的作用域

闭包=内存函数+外层函数的变量

闭包作用：封闭数据，提供操作，外部也可以访问函数内部的变量

```html
<script>
  // 常见的闭包形式 外部可以访问使用函数内部的变量
  function outer() {
    let a = 10
    function fn() {
      console.log(a);
    }
    return f
  }
  const fun = outer()
  fun()
</script>
```

闭包应用：实现数据的私有

闭包引起的问题：内存泄露

### 1.5 变量提升

变量提升是JavaScript中比较“奇怪”的现象，它允许在变量声明之前即将访问（仅存在于var声明变量）

变量提升的流程：

1. 先把var变量提升到当前作用域最前面
2. 只提升变量声明，不提升变量赋值
3. 然后依次执行代码

注：

1. 变量在未声明即被访问时会报语法错误
2. 变量在va声明之前即被访问，变量的值为undefined
3. let/const声明的变量不存在变量提升
4. 变量提升出现在相同作用域当中
5. 实际开发中推荐先声明在访问变量

### 1.6 函数进阶

#### 1.6.1 函数提升

函数提升与变量提升类似，在函数声明之前就可被调用；

但函数表达式必须先声明和赋值，后调用，否则报错

总结：

+ 函数提升能够使函数的声明和调用更灵活
+ 函数表达式不存在提升的现象
+ 函数提升出现在相同的作用域当中

#### 1.6.2 函数参数

* 动态参数

  arguments是函数内部内置的伪数组变量，它包含了调用函数时传入的所有实参,它只存在函数之中

  ```html
  <script>
    function getSum() {
      //arguments动态参数只存在于函数里面
      //1.伪数组
      // console.log(arguments);[2,3,4]
      let sum = 0
      for (let i = 0; i < arguments.length; i++) {
        sum += arguments[i]
      }
      console.log(sum);
    }
      
    getSum(2, 3, 4)
    getSum(1, 2, 4, 5, 6, 9, 7)
  </script>
  ```

* 剩余参数

  剩余参数允许我们将一个不定数量的参数表示为一个数组

  ```html
  <script>
    function getSum(...args) {
      // console.log(args);
      let sum = 0
      for (let i = 0; i < args.length; i++) {
        sum += args[i]
      }
      console.log(sum);

    }
    getSum(2, 3, 5)
  </script>
  ```

  1. ...是语法符号，置于最末函数形参之前，用于获取多余的实参
  2. 借助...获取剩余实参，是一个真数组

#### 1.6.3 展开运算符（...）

展开运算符（...）可以将一个数组进行展开

```javascript
const arr=[1, 2, 3, 4, 5]
console.log(...arr)//1 2 3 4 5
```

说明：不会修改原数组

#### 1.6.4 箭头函数（重要）

1.基本语法

引入箭头函数的目的是更简短的函数写法并且不绑定this，箭头函数的语法比函数表达式更简洁

```html
 <script>
    const fn=function(){
      console.log(123);
    }

   //1.箭头函数 基本语法
    const fn = (x) => {
      console.log(x);
    }
    fn(1)//2

    //2.只有一个参数时可以省略小括号
    const fn = x => {
      console.log(x);
    }
    fn(2)//2

    //3.只有一行代码时可以省略大括号
    const fn = x => console.log(x);
    fn(2)//2

    //4.只有一行代码时可以还省略return
    const fn = x => x + x
    console.log(fn(1));//2
     
    //4.箭头函数可以直接返回一个对象
    const fn=(uname) => ({uname: uname })
    console.log(fn('刘德华'))//{uname:'刘德华'}

  </script>
```

总结：

* 箭头函数属于表达式函数，因此不存在函数提升
* 箭头函数只有一个参数时可以省略圆括号()，
* 箭头函数函数体只有一行代码时可以省略花括号{}，并自动作为返回值被返回
* 加括号的函数体返回对象字面量表达式

2.箭头函数参数

箭头函数没有arguments动态参数，但是有剩余参数...args

3.箭头函数this

在箭头函数出现之前，每一个新函数根据它是被如何调用的来定义这个函数的this值，

而箭头函数不会创建自己的this，它只会从自己的作用域链的上一层沿用this

```html
<script>
   // 1.箭头函数的this 是上一层作用域的this指向
   // const fn = () => {
   //   console.log(this);
   // }
   // fn()//window
   // 2.对象方法箭头函数this
   // const obj = {
   //   uname: 'pink',
   //   sayHi: () => {
   //     console.log(this);
   //     function fn() {
   //       console.log(this);
   //     }
   //     fn()
   //   }
   // }
   // obj.sayHi()//window //window
   const obj = {
     uname: 'pink',
     sayHi: function () {
       console.log(this);
       let i = 10
       const count = () => {
         console.log(this);
       }
       count()
     }
   }
   obj.sayHi()//{uname: 'pink', sayHi: ƒ}//{uname: 'pink', sayHi: ƒ}
 </script>
```

在开发中【使用箭头函数前需要考虑函数中this的值】，事件回调函数使用箭头函数时，this为全局的window，因此DOM回调函数为了方便，还是不太推荐使用箭头函数

### 1.7 解构赋值

#### 1.7.1 数组解构

数组解构是将数组的单元值快速批量赋值给一系列变量的简洁语法

基本语法：

1. 赋值运算符=左侧的[]用于批量声明变量，右侧数组的单元值将被赋值给左侧的变量
2. 变量的顺序对应数组单元值的位置依次进行赋值操作

```html
<script>
  const arr = [100, 60, 80]
  //数组解构 批量的赋值操作
  const [max, min, avg] = arr
  console.log(max);//100
  console.log(min);//60
  console.log(avg);//80
</script>
```

数组开头的，特别是前面有语句的一定注意加分号

```html
;[b,a]=[a,b]
```
数组解构的不同情况：
1. 变量多， 单元值少 ， undefined

```javascript
const [a, b, c, d] = [1, 2, 3]
console.log(a) // 1
console.log(b) // 2
console.log(c) // 3
console.log(d) // undefined
```

2. 变量少， 单元值多

```javascript
const [a, b] = [1, 2, 3]
console.log(a) // 1
console.log(b) // 2
```

3. 剩余参数 变量少， 单元值多

```javascript
const [a, b, ...c] = [1, 2, 3, 4]
console.log(a) // 1
console.log(b) // 2
console.log(c) // [3, 4]  真数组
```

4. 防止 undefined 传递

```javascript
const [a = 0, b = 0] = [1, 2]
//const [a = 0, b = 0] = []
console.log(a) // 1
console.log(b) // 2
```

5. 按需导入赋值

```javascript
const [a, b, , d] = [1, 2, 3, 4]
console.log(a) // 1
console.log(b) // 2
console.log(d) // 4
```

6. 多维数组解构

```javascript
const arr = [1, 2, [3, 4]]
const [a, b, c] = [1, 2, [3, 4]]
console.log(a) // 1
console.log(b) // 2
console.log(c) // [3,4]
```

#### 1.7.2 对象解构

基本语法：

1. 赋值运算符=左侧的{}用于批量声明变量，右侧数组的属性值将被赋值给左侧的变量
2. 对象属性的值将赋值和给与属性名相同的变量
3. 注意解构的变量名不要和外面的变量名冲突否则报错
4. 对象中找不到与变量名一致的属性时变量值为undefined
5. 对象解构的变量名，可以重新改名，语法：`旧变量名:新变量名`

```html
<script>
  uname = 'red老师'
  const obj = {
    uname: 'pink老师',
    age: 18
  }
  const { age, uname: username } = obj
  //等价于const uname = obj.uname
  console.log(uname);//red老师
  console.log(age);//18
  console.log(username);//pink老师
</script>
```

数组对象解构：

```javascript
//数组对象解构
const pig = [{
  uname: '佩奇',
  age: 13
}]
const [{ uname, age }] = pig
console.log(uname);//佩奇
```

多级对象解构：

```javascript
const pig = [{
   uname: '佩奇',
   family: {
     mother: '猪妈妈',
     father: '猪爸爸',
     sister: '乔治'
   },
   age: 13
 }]
 const [{ uname, family: { mother, father, sister }, age }] = pig
 console.log(uname);//佩奇
 console.log(father);//猪爸爸
 console.log(age);//13
```



## 二、构造函数&数据常用函数

### 2.1 深入对象

#### 2.1.1 创建对象的三种方式

1. 利用对象字面量创建对象
2. 利用new Object创建对象
3. 利用构造函数创建对象

#### 2.1.2 构造函数

构造函数：是一个特殊的函数，主要用来初始化对象

构造函数在技术上是常规函数

约定：它们的命名以大写字母开头，且只能由“new”操作符来执行

创建构造函数：

```html
<script>
  function Pig(name, age) {
    this.name = name
    this.age = age
  }
  // console.log(new Pig('佩奇', 6));
  // console.log(new Pig('乔治', 6));
  const peppa = new Pig('佩奇', 6)
  console.log(peppa);
</script>
```

说明：

1. 使用 new 关键字调用函数的行为被称为实例化
2. 实例化构造函数时没有参数时可以省略 ()
3. 构造函数内部无需写return，返回值即为新创建的对象
4. 构造函数内部的 return 返回的值无效，所以不要写return
5. new Object（） new Date（） 也是实例化构造函数

构造函数执行过程：

1. 创建新的空对象
2. 构造函数this指向新对象
3. 执行构造函数代码，修改this，添加新的属性
4. 返回新对象

#### 2.1.3 实例成员&静态成员

**通过构造函数创建的对象称为实例对象，实例对象中的属性和方法称为实例成员**

说明：

1. 实例对象的属性和方法即为实例成员
2. 为构造函数传入参数，动态创建结构相同但值不同的对象
3. 构造函数创建的实例对象彼此独立互不影响。

**构造函数的属性和方法被称为静态成员**

说明：

1. 构造函数的属性和方法被称为静态成员
2. 一般公共特征的属性或方法静态成员设置为静态成员
3. 静态成员方法中的 this 指向构造函数本身

### 2.2 内置构造函数

引用类型：Object、Array、RegExp、Date等

包装类型：String、Number、Boolean等

#### 2.2.1 Object

Object是内置的构造函数，用于创建普通对象

* Object.keys静态方法获取对象的所有属性（键）

  Object.values静态方法获取对象的所有属性值

  返回的都是一个数组

  ```html
  <script>
    const o = { uname: "red", age: 18 }
    //获取所有的属性名
    console.log(Object.keys(o));//['uname', 'age']
    //获得所有的属性值
    console.log(Object.values(o));//['red', 18]
  </script>
  ```

* Object.assign静态方法常用于对象拷贝

  经常使用的属性是给对象添加属性

  ```html
  <script>
    const o = { uname: "red", age: 18 }
   
    const oo = {}
    Object.assign(oo, o)
    console.log(oo);//{uname: 'red', age: 18}
    console.log(Object.assign(oo, { gender: '女' }));  //{uname: 'red', age: 18, gender: '女'}
  </script>
  ```

#### 2.2.2 Array

Array是内置的构造函数，用于创建函数

创建数组建议使用字面量创建，不用Array构造函数创建

##### 2.2.2.1 数组常见实例方法-核心方法

| 方法    | 作用     | 说明                                                         |
| ------- | -------- | ------------------------------------------------------------ |
| forEach | 遍历数组 | 不返回数组，经常用于查找遍历数组元素                         |
| filter  | 过滤数组 | 返回新数组，返回的是筛选满足条件的数组元素                   |
| map     | 迭代数组 | 返回新数组，返回的是处理之后的数组元素，想要使用返回的新数组 |
| reduce  | 累计器   | 返回累计处理的结果，经常用于求和                             |

reduce基本语法：

```javascript
arr.reduce(function(){},起始值)
arr.reduce(function(上一次值,当前值){},起始值)
```

代码示例：

```html
<script>
  const arr = [1, 4, 6]
  //1. 没有初始值
  const total1 = arr.reduce(function (prev, current) {
    return prev + current
  })
  console.log(total1);  //11
  
  //2. 有初始值 初始值为10
  const total2 = arr.reduce(function (prev, current) {
    return prev + current
  }, 10)
  console.log(total2); //21
    
  //3. 箭头函数
  const total3 = arr.reduce((prev, current) => prev + current, 10)
  console.log(total3);//21
</script>
```

##### 2.2.2.2 数组常见方法-其他方法

| 方法      | 说明                                                         |
| --------- | ------------------------------------------------------------ |
| join      | 数组元素拼接为字符串，返回字符串                             |
| find      | 查找元素，返回符合测试条件的第一个数组元素值，如果没有符合条件的则返回undefined |
| every     | 检测数组所有元素是否都符合指定条件，若所有元素都通过检测返回true，反之返回false |
| some      | 检测数组中的元素是否符合指定条件，如果有元素满足条件返回true，反之返回false |
| concat    | 合并两个数组，返回生成新数组                                 |
| sort      | 对原数组单元值排序                                           |
| splice    | 删除或替换原数组单元                                         |
| reverse   | 反转数组                                                     |
| findIndex | 查找元素的索引值                                             |

##### 2.2.2.3 伪数组转换为数组

静态方法：Array.from()

```html
<body>
  <ul>
    <li>1</li>
    <li>2</li>
    <li>3</li>
  </ul>
  <script>
    //Array.from(lis) 把伪数组转换为真数组
    const lis = document.querySelectorAll('ul li')
    console.log(lis);//NodeList(3) [li, li, li]
    const liss = Array.from(lis)
    console.log(liss);//[li, li, li]
    liss.pop()
    console.log(liss);//[li, li]
  </script>
</body>
```

#### 2.2.3 String

| 方法                                                 | 说明                                                         |
| ---------------------------------------------------- | ------------------------------------------------------------ |
| length                                               | 获取字符串的长度                                             |
| split('分隔符')                                      | 将字符串拆分成数组                                           |
| substring(需要截取的第一个字符的索引[,结束的索引号]) | 用于字符串截取                                               |
| startsWith(检测字符串[,检测位置索引号])              | 检测是否以某字符开头                                         |
| includes(搜索的字符串[,搜索位置索引号])              | 判断一个字符串是否包含在另一个字符串中，根据情况返回false或true |
| toUpperCase                                          | 将字母转化为大写                                             |
| toLowerCase                                          | 将字母转化为小写                                             |
| indexOf                                              | 检测是否包含某字符                                           |
| endsWith                                             | 检测是否以某字符结尾                                         |
| replace                                              | 用于替换字符串，支持正则表达式                               |
| match                                                | 用于查找字符串，支持正则表达式                               |

#### 2.2.4 Number

常用方法：

`toFixed(保留小数的位数)` ：设置保留小数位的长度

```javascript
const price = 12.345
console.log(price.toFixed(2))//12.35
```

## 三、深入面向对象

### 3.1 编程思想

#### 3.1.1 面向过程编程

面向过程就是分析出解决问题所需要的步骤，然后用函数把这些步骤一步一步实现，使用的时候再一个一个的依次调用就可以了。

**面向过程编程** ：

优点：性能比面向对象高，适合跟硬件联系很紧密的东西，例如单片机就采用的面向过程编程。

缺点：没有面向对象易维护、易复用、易扩展

#### 3.1.2 面向对象编程

面向对象是把事务分解成为一个个对象，然后由对象之间分工与合作。

面向对象是以对象功能来划分问题，而不是步骤。

 在面向对象程序开发思想中，每一个对象都是功能中心，具有明确分工。

 面向对象编程具有灵活、代码可复用、容易维护和开发的优点，更适合多人合作的大型软件项目。

**面向对象编程**：

优点：易维护、易复用、易扩展，由于面向对象有封装、继承、多态性的特性，可以设计出低耦合的系统，使系统 更加灵活、更加易于维护

缺点：性能比面向过程低

### 3.2 构造函数

* 封装是面向对象思想中比较重要的一部分，js面向对象可以通过构造函数实现的封装。
* 同样的将变量和函数组合到了一起并能通过 this 实现数据的共享，所不同的是借助构造函数创建出来的实例对象之间是彼此不影响的

```javascript
function Star(uname, age) {
	this.uname = uname
	this.age = age
	this.sing = function() {
		console.log('我会唱歌')
	}
} 

const ldh = new Star('刘德华', 18)
const zxy = new Star('张学友', 19)
console.log(ldh.sing===zxy.sing)//false
//构造函数方法很好用，但是 存在浪费内存的问题
```

### 3.3 原型

#### 3.3.1 原型概念

* 构造函数通过原型分配的函数是所有对象所共享的
* JavaScript规定，**每一个构造函数都有一个prototype属性，指向另一个对象**，所以可以称为原型对象
* 这个对象可以挂载函数，对象实例化不会多次创建原型上函数，节约内存
* 可以把那些不变的方法，直接定义在prototype对象中，这样所有对象的示例就可以共享这些方法
* 构造函数和原型函数中的this都执行实例化的对象
* 自定义的方法可以定义到原型里面

#### 3.3.2 construcor属性

每个原型对象里面有个constructor属性，该属性指向该原型对象的构造函数

使用场景：

* 如果有多个对象的方法，我们可以给原型对象采取对象形式赋值
* 但是这样就会覆盖构造函数原型对象原来的内容，这样修改后的原型对象constructor就不会在指向当前构造函数，但是我们可以再修改后的原型对象中添加一个constructor指向原来的构造函数：`constructor:构造函数`

#### 3.3.3 对象原型

对象都会有一个对象`__proto__`指向构造函数的prototype原型对象，之所以我们对象可以使用构造函数prototype原型对象的属性和方法，就是因为对象有`__proto__`原型的存在

注：

* `__proto__`是JS非标准属性
* `[[prototype]]`和`__proto__`意义相同
* 对象原型`__proto__`用来表明当前实例对象指向哪个**原型对象prototype**
* **对象原型`__proto__`**里面也有一个constructor属性，指向创建该实例对象的构造函数

```html
<script>
  function Star() {

  }
  const ldh = new Star()
  console.log(ldh.__proto__ === Star.prototype);//true
  console.log(ldh.__proto__.constructor === Star);//true
</script>
```

#### 3.3.4 原型继承

继承是面向对象编程的另一个特征。通过继承进一步提升代码封装的程度，JavaScript中大多是借助原型对象实现继承的特性

```html
<script>
    // 继续抽取   公共的部分放到原型上
    // const Person1 = {
    //   eyes: 2,
    //   head: 1
    // }
    // const Person2 = {
    //   eyes: 2,
    //   head: 1
    // }
    // 构造函数  new 出来的对象 结构一样，但是对象不一样
    function Person() {
      this.eyes = 2
      this.head = 1
    }
    // console.log(new Person)
    // 女人  构造函数   继承  想要 继承 Person
    function Woman() {

    }
    // Woman 通过原型来继承 Person
    // 父构造函数（父类）   子构造函数（子类）
    // 子类的原型 =  new 父类  
    Woman.prototype = new Person()   // {eyes: 2, head: 1} 
    // 指回原来的构造函数
    Woman.prototype.constructor = Woman

    // 给女人添加一个方法  生孩子
    Woman.prototype.baby = function () {
      console.log('宝贝')
    }
    const red = new Woman()
    console.log(red)
    // console.log(Woman.prototype)
    // 男人 构造函数  继承  想要 继承 Person
    function Man() {

    }
    // 通过 原型继承 Person
    Man.prototype = new Person()
    Man.prototype.constructor = Man
    const pink = new Man()
    console.log(pink)
  </script>
```

#### 3.3.5 原型链

基于原型对象的继承使得不同构造函数的原型对象关联在一起，并且这种关联的关系是一种链状的结构，我们将原型对象的链状结构关系称为原型链

![](images\JS\原型链.png)

* 只要是对象就有`__proto__`指向原型对象
* 只要是原型对象就要constructor，只会指向创造自己的构造函数

原型链-查找规则

​	① 当访问一个对象的属性（包括方法）时，首先查找这个对象自身有没有该属性。

​	② 如果没有就查找它的原型（也就是   `__proto__` 指向的 prototype 原型对象）

​	③ 如果还没有就查找原型对象的原型（Object的原型对象）

​	④ 依此类推一直找到 Object 为止（null）

​	⑤   `__proto__` 对象原型的意义就在于为对象成员查找机制提供一个方向，或者说一条路线

​	⑥ 可以使用 instanceof 运算符用于检测构造函数的 prototype 属性是否出现在某个实例对象的原型链上

```html
<body>
  <script>
    // function Objetc() {}
    console.log(Object.prototype)
    console.log(Object.prototype.__proto__)

    function Person() {

    }
    const ldh = new Person()
    // console.log(ldh.__proto__ === Person.prototype)
    // console.log(Person.prototype.__proto__ === Object.prototype)
    console.log(ldh instanceof Person)//true
    console.log(ldh instanceof Object)//true
    console.log(ldh instanceof Array)//false
    console.log([1, 2, 3] instanceof Array)//true
    console.log(Array instanceof Object)//true
  </script>
</body>
```

## 四、高级技巧

### 4.1 深浅拷贝

浅拷贝与深拷贝只针对引用类型

#### 4.1.1 浅拷贝

拷贝的是地址

常见方法：

1. 拷贝对象：Object.assign()/展开运算符{...obj}拷贝对象
2. 拷贝数组：Array.prototype.concat()或者[...arr]

```html
<script>
    const obj = {
      uname: 'pink',
      age: 18,
      family: {
        baby: '小pink'
      }
    }
    // 浅拷贝
    // const o = { ...obj }
    // console.log(o)
    // o.age = 20
    // console.log(o)
    // console.log(obj)
    const o = {}
    Object.assign(o, obj)
    o.age = 20
    o.family.baby = '老pink'
    console.log(o)
    console.log(obj)//修改o时obj中的family也被修改
  </script>
```

1. 直接赋值和浅拷贝的区别

*  直接赋值的方法，只要是对象，都会相互影响，因为是直接拷贝对象栈里面的地址

* 浅拷贝如果是一层对象，不相互影响，如果出现多层对象拷贝还会相互影响

2. 浅拷贝的理解

* 拷贝对象之后，里面的属性值是简单数据类型直接拷贝值
* 如果属性值是引用数据类型则拷贝的是地址

#### 4.1.2 深拷贝

深拷贝：拷贝的是对象，不是地址

常见方法：

1. 通过递归函数实现深拷贝

   ```html
   <script>
       const obj = {
         uname: 'pink',
         age: 18,
         hobby: ['pingpong', 'football']
       }
       const o = {}
       //深拷贝函数
       function deepCopy(newObj, oldObj) {
         for (let k in oldObj) {
           //k   属性名    oldObj[k]   属性值
           //处理数组的数据
           if (oldObj[k] instanceof Array) {
             newObj[k] = []
             deepCopy(newObj[k], oldObj[k])
           } else {
             // newObj[k]===o.uname
             newObj[k] = oldObj[k]
           }
         }
       }
       deepCopy(o, obj)//函数调用

       console.log(o);
       o.age = 20
       o.hobby[0] = 'basketball'
       console.log(obj);//对象不会因为o的修改而被修改
   </script>
   ```

2. js库lodash里面cloneDeep内部实现了深拷贝

   ```html
   <body>
     <!-- 先引用 -->
     <script src="./lodash.min.js"></script>
     <script>
       const obj = {
         uname: 'pink',
         age: 18,
         hobby: ['乒乓球', '足球'],
         family: {
           baby: '小pink'
         }
       }
       const o = _.cloneDeep(obj)
       console.log(o)
       o.family.baby = '老pink'
       console.log(obj)
     </script>
   </body>
   ```

3. json系列化

   ```html
   <body>
     <script>
       const obj = {
         uname: 'pink',
         age: 18,
         hobby: ['乒乓球', '足球'],
         family: {
           baby: '小pink'
         }
       }
       // 把对象转换为 JSON 字符串
       // console.log(JSON.stringify(obj))
       const o = JSON.parse(JSON.stringify(obj))
       console.log(o)
       o.family.baby = '123'
       console.log(obj)
     </script>
   </body>
   ```

### 4.2 异常处理

异常处理是指预估代码执行过程中可能发生的错误，然后最大程度的避免错误发生导致整个程序无法继续运行

#### 4.2.1 throw抛异常

```html
<script>
    function fn(x, y) {
      if (!x || !y) {
        // throw '用户没有传入参数'
        throw new Error('用户没有传入参数')
      }
      return x + y
    }
    console.log(fn());
  </script>
```

1. throw抛出异常信息，程序也会终止执行
2. throw后面跟的是错误提示信息
3. Error对象配合throw使用，能够设置跟详细的错误信息

#### 4.2.2 try/catch捕获异常

try/catch捕获错误信息（浏览器提供的错误信息）

```html
<body>    
    <p>123</p>
   <script>
      function foo() {
         try {
           // 查找 DOM 节点
           const p = document.querySelector('.p')//错误的代码
           p.style.color = 'red'
         } catch (error) {
           // try 代码段中执行有错误时，会执行 catch 代码段
           // 查看错误信息
           console.log(error.message)
           // 终止代码继续执行
           return
         }
         finally {
             alert('执行')
         }
         console.log('如果出现错误，我的语句不会执行')
       }
       foo()
   </script>
</body>
```

1. try...catch用于捕获错误信息
2. 将预估可能发生错误的代码写在try代码段中
3. 如果try代码段中出现错误后，会执行catch代码段，并截取到错误信息
4. finally不管是否有错误，都会被执行

#### 4.2.3 debugger

相当于在写 `dubugger` 的哪一行打一个断点

断点调试

### 4.3 处理this

#### 4.3.1 this指向

普通函数没有明确调用者时this值为window，严格模式下没有调用者时this的值undefined

箭头函数中的 this 与普通函数完全不同，也不受调用方式的影响，事实上箭头函数中并不存在 this 

1. 箭头函数会默认帮我们绑定外层 this 的值，所以在箭头函数中 this 的值和外层的 this 是一样的
2. 箭头函数中的this引用的就是最近作用域中的this
3. 向外层作用域中，一层一层查找this，直到有this的定义

注意：

* 不适用构造函数，原型函数，dom事件函数等等
* 使用需要使用上层this的地方

#### 4.3.2 改变this

##### 4.3.2.1 call()改变this指向

使用call方法调用函数，同时指定被调用函数中this的值

语法：`fun.call(thisArg,arg1,arg2,...)`

* thisArg：在fun函数运行是指定的this值
* arg1，arg2：传递的其他参数
* 返回值就是函数的返回值，因为他就是调用函数 

```html
<script>
  // 普通函数
  function sayHi() {console.log(this);}

  let user = {
    name: '小明',
    age: 18
  }

  let student = {
    name: '小红',
    age: 16
  }

  // 调用函数并指定 this 的值
  sayHi.call(user); // this 值为 user
  sayHi.call(student); // this 值为 student
</script>>
```

##### 4.3.2.2 apply()改变this指向

使用apply方法调用函数，同时指定被调用函数中this的值

语法：`fun.apply(thisArg,[argsArray])`

* thisArg：在fun函数运行是指定的this值
* argsArray：传递的值，必须包含在数组里面，数组的单元值依次自动传入函数做为函数的参数
* 返回值就是函数的返回值，因为他就是调用函数
* 因为apply主要是跟数组有关系，比如使用Math.max()求数组的最大值

```html
<script>
  // 求和函数
  function counter(x, y) {
    return x + y
  }
  // 调用 counter 函数，并传入参数
  let result = counter.apply(null, [5, 10])
  console.log(result)//15
</script>
```

使用场景：求数组的最值

```javascript
// 使用场景： 求数组最大值
// const max = Math.max(1, 2, 3)
// console.log(max)
const arr = [100, 44, 77]
const max = Math.max.apply(Math, arr)
const min = Math.min.apply(null, arr)
console.log(max, min)//100, 44
// 使用场景： 求数组最大值
console.log(Math.max(...arr)//100
```

##### 4.3.2.3 bind()改变this指向

使用bind方法不会调用函数，但能指定被调用函数中this的值

语法：`fun.bind(thisArg, arg1, arg2, ...)`

- thisArg：在fun函数运行是指定的this值
- arg1，arg2：传递的其他参数
- 返回指定的this值和初始化参数改造的原函数拷贝（新函数）
- 当我们只想要改变this指向，并且不想调用这个函数的时候，可以使用bind，比如改变定时器内部的this指向

```html
<body>
  <button>发送短信</button>
  <script>
    const obj = {
      name: 'gaoxin'
    }
    function fn() {
      console.log(this);
    }
    //1.不会调用函数
    //2.能改变this指向
    //3，返回值是函数，但是这个函数的this是修改过的
    const fun = fn.bind(obj)
    console.log(fun);
    fun()

    //需求：点击按钮，这个按钮本身就会被禁用2s
    const btn = document.querySelector('button')
    btn.addEventListener('click', function () {
      this.disabled = true
      setTimeout(function () {
        this.disabled = false//普通函数里面指向的是调用者window
        //利用bind（）将this由指向window改为指向 btn,this指向btn
      }.bind(this), 1000)
    })
  </script>
</body>
```

##### 4.3.2.4 call apply bind 总结

* 相同点:  
  1. 都可以改变函数内部的this指向.
* 区别点: 
  1. call 和 apply 会调用函数, 并且改变函数内部this指向.
  2. call 和 apply 传递的参数不一样, call 传递参数 aru1, aru2..形式，apply 必须数组形式[arg]
  3. bind 不会调用函数, 可以改变函数内部this指向.


* 主要应用场景: 
* 1. call 调用函数并且可以传递参数
  2.  apply 经常跟数组有关系. 比如借助于数学对象实现数组最大值最小值
  3.  bind 不调用函数,但是还想改变this指向. 比如改变定时器内部的this指向.

### 4.4 性能优化

#### 4.4.1 防抖

防抖：单位时间内，频繁触发事件，只执行最后一次

**需求案例**：鼠标在盒子上移动，鼠标停止之后，500ms后里面的数字就会变化+1

实现方式：

* lodash提供的防抖来处理

  ```html
  <body>
    <div class="box"></div>
    <script src="lodash.min.js"></script>
    <script>
      //利用防抖实现性能优化
      //需求：鼠标在盒子上移动，里面的数字就会变化加一
      const box = document.querySelector('.box')
      let i = 1
      function mouseMove() {
        box.innerHTML = i++
      }
      // 添加事件
      // box.addEventListener('mousemove', mouseMove)

      // 利用lodash实现防抖，500毫秒后采取加一
      // 语法：_.debounce(fun,时间)
      box.addEventListener('mousemove', _.debounce(mouseMove, 500))
    </script>
  </body>
  ```

* 手写一个防抖函数来处理

  核心是利用计数器（setTimeout）来实现：

  1. 声明一个定时器变量
  2. 当鼠标每次滑动都先判断是否有定时器，如果有定时器先清除以前的定时器
  3. 如果没有定时器则开启定时器，记得存到变量里面
  4. 在定时器里面调用执行的函数

  ```html
  <body>
    <div class="box"></div>
    <script>
      //利用防抖实现性能优化
      //需求：鼠标在盒子上移动，里面的数字就会变化加一
      const box = document.querySelector('.box')
      let i = 1
      function mouseMove() {
        box.innerHTML = i++
      }

      function debounce(fn, t) {
        let timer
        // return 返回一个匿名函数
        return function () {
          if (timer) clearTimeout(timer)
          timer = setTimeout(function () {
            fn()
          }, t)
        }
      }
      box.addEventListener('mousemove', debounce(mouseMove, 500))
    </script>
  </body>
  ```

#### 4.4.2 节流

节流：单位时间内，频繁触发事件，只执行一次

**需求案例**：鼠标在盒子上移动，不管移动多少次，每隔500ms才+1

实现方式：

- lodash提供的节流来处理

  ```html
  <body>
    <div class="box"></div>
    <script src="lodash.min.js"></script>
    <script>
      //利用防抖实现性能优化
      //需求：鼠标在盒子上移动，里面的数字就会变化加一
      const box = document.querySelector('.box')
      let i = 1
      function mouseMove() {
        box.innerHTML = i++
      }

      box.addEventListener('mousemove', _.throttle(mouseMove, 500))
    </script>
  </body>
  ```

- 手写一个节流函数来处理

  核心是利用计数器（setTimeout）来实现

  1. 声明一个定时器变量
  2. 当鼠标每次滑动都先判断是否有定时器，如果有定时器则不开启新定时器
  3. 如果没有定时器则开启定时器，记得存到变量里面
     - 定时器里面调用执行的函数
     - 定时器里面要把定时器清空

  ```html
  <body>
    <div class="box"></div>
    <script>
      //利用防抖实现性能优化
      //需求：鼠标在盒子上移动，里面的数字就会变化加一
      const box = document.querySelector('.box')
      let i = 1
      function mouseMove() {
        box.innerHTML = i++
      }

      function throttle(fn, t) {
        let timer
        return function () {
          if (!timer) {
            timer = setTimeout(function () {
              fn()
              //清空定时器
              timer = null
              //不能使用clearTimeout()
              //原因：在setTimeout中是无法删除定时器的，因为定时器还在运作，所以使用timer = null，而				不是clearTimeout()
            }, t)
          }
        }
      }
      box.addEventListener('mousemove', throttle(mouseMove, 2000))
    </script>
  </body>
  ```

  ​