## 一、初识Vue

Vue是一个用于构建用户界面的渐进式框架，Vue的两种使用方式：

* Vue核心包开发：局部模块改造
* Vue核心包&Vue插件工程化开发：整站开发

### 1.1 Vue快速上手

#### 1.1.1 Vue实例

创建用户实例，初始化渲染：

1. 构建用户界面

2. 创建Vue实例，初始化渲染：

   * 准备容器，引包（官网）-开发版本/生产版本

   * 创建Vue实例 new Vue()

   * 指定配置项 → 渲染数据：el指定挂载点，data提供数据

```html
<body>
  <!-- 创建Vue实例，初始化渲染 -->
  <div class="box">
    {{msg}}
  </div>
  <div class="box2">
    {{count}}
  </div>
  <div id="app">
    <!-- 这里将来会编写一些代码 -->
    <h1>{{ msg }}</h1>
    <a href="#"> {{count}}</a>

  </div>

  <!-- 引入的是开发版本的包 -->
  <script src="https://cdn.jsdelivr.net/npm/vue@2.7.16/dist/vue.js"></script>
  <!-- <script src="vue.js"></script> -->

  <script>
    //一旦引入了VueJs核心包，在全局环境就有了Vue的构造函数
    const app = new Vue({
      // 通过el配置选择器，指定Vue管理的是哪个盒子
      el: "#app",
      //通过data提供渲染的数据
      data: {
        msg: 'hello Vue',
        count: 666
      }
    }
    )
  </script>

</body>
```

#### 1.1.2 插值表达式

插值表达式是一种Vue的模板语法

作用：利用表达式进行插值，渲染到页面上

表达式：是可以被求值的代码，JS引擎会将其计算出一个结果

语法：`{{表达式}}`

注：

* 使用的数据必须存在
* 支持的是表达式，而非语句，比如：if for
* 不能在标签属性中使用`{{ }}`插值

#### 1.1.3 响应式特征

数据的响应式处理 → 响应式：数据变化，视图自动更新

```mermaid
graph LR;
数据--修改数据-->Vue监听到数据监听
Vue监听到数据监听--自动更新视图Dom操作-->视图界面
```

data中的数据，最终回添加到实例上

#### 1.1.4 开发者工具

### 1.2 Vue指令

Vue会根据不同的指令，针对标签实现不同的功能（指令：带有v-前缀的特殊标签属性）

为啥要学：提高程序员操作 DOM 的效率。

vue 中的指令按照不同的用途可以分为如下 6 大类：

- 内容渲染指令（v-html、v-text）内容渲染指令用来辅助开发者渲染 DOM 元素的文本内容
- 条件渲染指令（v-show、v-if、v-else、v-else-if）用来辅助开发者按需控制 DOM 的显示与隐藏
- 事件绑定指令（v-on）
- 属性绑定指令 （v-bind）
- 双向绑定指令（v-model）
- 列表渲染指令（v-for）

#### 1.2.1 内容渲染指令

内容渲染指令用来辅助开发者渲染 DOM 元素的文本内容。常用的内容渲染指令有如下2 个：

- v-text（类似innerText）


- - 使用语法：`<p v-text="uname">hello</p>`，意思是将 uame 值渲染到 p 标签中
  - 类似 innerText，使用该语法，会覆盖 p 标签原有内容


- v-html（类似 innerHTML）


- - 使用语法：`<p v-html="intro">hello</p>`，意思是将 intro 值渲染到 p 标签中
  - 类似 innerHTML，使用该语法，会覆盖 p 标签原有内容
  - 类似 innerHTML，使用该语法，能够将HTML标签的样式呈现出来。

#### 1.2.2 条件渲染指令

*  v-show
  * 作用：控制元素的显示隐藏
  * 语法：`v-show="表达式"`  表达式值true显示，false隐藏
  * 原理：  切换 display:none 控制显示隐藏
  * 适合场景：频繁切换显示隐藏的的场景
*  v-if
  * 作用：控制元素的显示隐藏（条件渲染）
  * 语法：`v-if="表达式"`  表达式值true显示，false隐藏
  * 原理：根据判断条件控制元素的创建和移除
  * 适合场景：要么显示，要么隐藏，不频繁切换的场景
*  v-else 和 v-else-if
   * 作用：辅助v-if进行判断渲染
   * 语法：v-else  v-else-if="表达式"
   * 需要紧接着v-if使用

#### 1.2.3 事件绑定指令

1. 作用：注册事件=添加监听+提供逻辑范围
2. 语法：
   1. v-on：事件名=“内联语句”
   2. v-on：事件名=“methods中的函数名”
   3. v-on：事件名="处理函数(实参)
3. 简称：@事件名
4. 注意：methods中的this指向Vue实例

#### 1.2.4 属性绑定指令

1. 作用：动态设置html的标签属性 比如：src、url、title
2. 语法**：**v-bind:属性名=“表达式”
3. v-bind:可以简写成 =>   ：

#### 1.2.5 列表渲染指令

Vue 提供了 v-for 列表渲染指令，用来辅助开发者基于一个数组来循环渲染一个列表结构。

v-for 指令需要使用 `(item, index) in arr` 形式的特殊语法，其中：

- item 是数组中的每一项
- index 是每一项的索引，不需要可以省略
- arr 是被遍历的数组

此语法也可以遍历**对象和数字**

```js
//遍历对象
<div v-for="(value, key, index) in object">{{value}}</div>
//value:对象中的值
//key:对象中的键
//index:遍历索引从0开始

//遍历数字
<p v-for="item in 10">{{item}}</p>
item从1 开始
```

语法： key="唯一值"

作用：给列表项添加的唯一标识。便于Vue进行列表项的正确排序复用。

为什么加key：Vue 的默认行为会尝试原地修改元素（就地复用）

实例代码：

```html
<ul>
  <li v-for="(item, index) in booksList" :key="item.id">
    <span>{{ item.name }}</span>
    <span>{{ item.author }}</span>
    <button @click="del(item.id)">删除</button>
  </li>
</ul>
```

注意：

1. key 的值只能是字符串 或 数字类型
2. key 的值必须具有唯一性
3. 推荐使用  id 作为 key（唯一），不推荐使用 index 作为 key（会变化，不对应）

#### 1.2.6 双向绑定指令

1. 作用：给表单元素使用，双向数据绑定→可以快速获取或设置表单元素内容

   ①数据变化：视图自动更新

   ②视图变化：数据自动更新

2. 语法：v-model="变量"

### 1.3 指令修饰符

通过“.”指明一些指令后缀，不同后缀封装了不同的处理操作→简化代码

1. 按键修饰符

   @keyup.enter	键盘回车监听

2. v-model修饰符

   v-model.trim		去除首尾空格

   v-model.number 	转数字

3. 事件修饰符

   @事件名.stop	阻止冒泡

   @事件名.prevent 	阻止默认行为

### 1.4 v-bind对于样式控制的增强

#### 1.4.1 操作class

语法：:class="对象/数组"

1. 对象→键就是类名，值是布尔值，如果值为true，有这个类，否则没有这个类

   ```html
   <div class="box" :class="{ 类名1: 布尔值, 类名2: 布尔值 }"></div>
   ```

   ​    适用场景：一个类名，来回切换

2. 数组→数组中的所有的类，都会添加到盒子上，本质上就是一个class类表

   ```html
   <div class="box" :class="[ '类名1', '类名2', '类名3' ]"></div>
   ```

      使用场景:批量添加或删除类

#### 1.4.2 操作style

语法：:style="样式对象"

```html
<div class="box" :style="{ CSS属性名1: CSS属性值, CSS属性名2: CSS属性值 }"></div>
```

使用场景：具体属性的动态配置

### 1.5 v-model应用于其他表单元素

常见的表单元素都可以用 v-model 绑定关联  →  快速获取或设置表单元素的值

它会根据控件类自动选取正确的方法来更新元素

```js
输入框  input:text   ——> value
文本域  textarea	 ——> value
复选框  input:checkbox  ——> checked
单选框  input:radio   ——> checked
下拉菜单 select    ——> value
...
```

### 1.6 计算属性

#### 1.6.1 计算属性

概念：基于现有的数据，计算出来的新属性。依赖的数据变化，自动重新计算

语法：

1. 生命在computed配置项中，一个计算属性对应一个函数
2. 使用起来和普通属性一样使用{{计算属性名}}

代码：

```javascript
computed:{
  计算属性名 () {
     计算代码
     return 结果
	}
}
```

注意：

1. computed配置项和data配置项是**同级**的
2. computed中的计算属性**虽然是函数的写法**，但他**依然是个属性**
3. computed中的计算属性**不能**和data中的属性**同名**
4. 使用computed中的计算属性和使用data中的属性是一样的用法
5. computed中计算属性内部的**this**依然**指向的是Vue实例**

特性：

* 缓存特性（提升性能），计算属性会对计算出来的结果缓存，再次使用直接读取缓存，依赖项变了，会自动重新计算→并在次缓存

#### 1.6.2 计算属性完整写法

计算属性默认的简写，只能读取访问，不能修改。要修改，需要写计算属性的完整写法

```js
computed: {
  //没有配置设置的逻辑
  // fullName() {
  //   return this.firstName + this.lastName
  // }
  //完整写法
  fullName: {
    //获取求值时,执行get,有缓存优先都缓存
    get() {
      return this.firstName + this.lastName
    },
    //被修改赋值时,执行set
    set(value) {
      console.log(value);
      this.firstName = value.slice(0, 1)
      this.lastName = value.slice(1)
    }
  }
}
```

### 1.7 watch监视器

#### 1.7.1 watch监视器

作用：监视数据变化，执行一些业务逻辑或异步操作

语法：

1. watch同样声明在跟data同级的配置项中
2. 简单写法： 简单类型数据直接监视
3. 完整写法：添加额外配置项

```js
watch: {
  // 该方法会在数据变化时，触发执行
  数据属性名 (newValue, oldValue) {
    一些业务逻辑 或 异步操作。 
  },
  '对象.属性名' (newValue, oldValue) {
    一些业务逻辑 或 异步操作。 
  }
}
```

#### 1.7.2 watch监视器完整写法

完整写法 —>添加额外的配置项

1. deep:true 对复杂类型进行深度监听
2. immdiate:true 初始化 立刻执行一次

```js
data: {
  obj: {
    words: '苹果',
    lang: 'italy'
  },
},
watch: {// watch 完整写法
  对象: {
    deep: true, // 深度监视
    immdiate:true,//立即执行handler函数
    handler (newValue) {
      console.log(newValue)
    }
  }
}
```

