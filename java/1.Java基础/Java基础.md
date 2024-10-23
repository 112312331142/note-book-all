# JAVA

### 常见dos命令：

```bash
c:  #盘符切换
dir #查看当前目录的所有文件
cd [/d] 位置参数 #切换目录
cd ..#返回上一级目录
cd>java.txt# 创建文件
cls #清除屏幕
exit #退出终端
ip config #查看电脑的ip
calc #打开计算器 
notepad #打开记事本
mspaint #打开画图工具
ping www.baidu.com #获取网站的ip地址
md test #创建文件夹
del #删除文件
rd #移除一个文件
```

### idea快捷键

```
ctrl alt m 给选中的代码封装成方法
ctrl alt v 自动生成左边
ctrl alt l 自动格式化
ctrl alt t surround with
ctrl alt 左键 回到上次的地方
ctrl alt shift j 选中同一个

ctrl +h 查看继承
ctrl +n 查找类
ctrl +p 查看参数形式
ctrl +o 选择重写的方法
ctrl +F12 查找方法

alt 部分选中

alt 回车 生成解决方案
alt 7 显示大纲
```



JDK Java Development Kit，

JRE Java Runtime Environment，

JVM JAVA Virtual Machine

psvm,sout

```java

```



graph LR
添加节点 --> 根
添加节点 --> 非根
根 -->直接变为黑色
非根 -->父黑色
非根-->父红色
父黑色-->则不需要任何操作P
父红色-->叔叔红色
父红色-->叔叔黑色,当前节点是父的右孩子
父红色-->叔叔黑色,当前节点是父的左孩子
叔叔红色-->将“父”和“叔叔”设为黑色
叔叔红色-->将“祖父”设为红色
叔叔红色-->如果祖父为根，再将根变为黑色
叔叔红色-->如果祖父非根，将祖父设置为当前结点在进行判断
叔叔黑色,当前节点是父的右孩子-->
叔叔黑色,当前节点是父的左孩子-->
叔叔黑色,当前节点是父的左孩子-->
叔叔黑色,当前节点是父的左孩子-->

## 一、基础

### 1.1标识符

Java所有的组成部分都需要名字。类名、变量名以及方法名都被称为标识符。

### 1.2数据类型

强类型语言：要求变量的使用要严格符合规定，所有变量都必须先定义后使用；

弱类型语言：

* 基本类型

整数(byte,short,int,long)、浮点数(float,double)、字符(char)、布尔(boolean)

* 应用类型

类，接口，数组



最好完全避免使用浮点数比较。

所有的字符本质还是数字。



强制转换  高-》低

自动转换  低-》高

* 不能对布尔值进行转换
* 不能把对象数据类型转换为不相干的类型
* 转换的时候可能存在内存溢出或精度问题



实例变量:从属于对象。如果不自行初始化，这个类型有默认值。基本类型默认是0，布尔值默认是false，除了基本类型，默认值都为空。

类变量

局部变量

```java
public class base {

    static double salary=5555;

    String name;
    int age;

    public static void main(String[] args) {
        //局部变量
        int i=10;
        System.out.println(i);
        base b=new base();
        System.out.println(b.age);
        System.out.println(b.name);

        System.out.println(salary);
    }
    
    public void add(){}
    
}
//10
//0
//null
//5555.0

```

修饰符 final 不区分先后顺序



位运算符：

```
A=0011 1100

B=0000 1101


A&B=0000 1100//位运算，一位一位比较，都为一时得1

A|B=0011 1101//位运算，一位一位比较，都为零时得0

A^B=0011 0001//异或运算

~A=1111 0010//取反运算

```

### 1.3包机制

为了更好地组织类，Java提供了包机制，用于区别类名的命名空间

包语句的语法格式为：

`package pkg1[.pkg2[.pkg3...]]`

一般公司利用域名导致作为包名；

为了能够利用某一个包的成员，我们需要在Java程序中明确导入该包。使用import语句可完成此功能

`import package[.package2...].(classmate|*);`

## 二、流程控制

### 2.1 Scanner对象

我们可以通过Scanner类来获取用户的输入

`Scanner s=new Scanner(System.in)`

通过Scanner类的next()与nextLine()方法来获取输入的字符串，在读取前我们一般需要使用hasNext()与hasNextLine()判断是否还有输入的数据

* next():
  * 一定要读取到有效字符后才可以结束输入
  * 对输入有效字符之前的空白，next()方法会自动将其去掉
  * 只有输入有效字符后才将其后面输入的空白作为分隔符或者结束符
  * next()不能得到带有空格的字符串

* nextLine()
  * 以Enter为结束符，也就是说nextLine()方法返回的是输入回车之前的所有字符
  * 可以获得空白

  ```java
  import java.util.Scanner;

  public class demo3 {
      public static void main(String[] args) {
          Scanner sc = new Scanner(System.in);
          int i=0;
          float f = 0.0f;

          System.out.println("请输入整数：");
          if(sc.hasNextInt()){
              i = sc.nextInt();
              System.out.println("整数数据："+i);
          }
          else{
              System.out.println("不是整数数据");
          }

          System.out.println("请输入小数：");
          if(sc.hasNextFloat()){
              f = sc.nextFloat();
              System.out.println("小数数据："+f);
          }
          else{
              System.out.println("不是小数数据");
          }
          sc.close();
    }  
  }
  ```


### 2.2 顺序结构

java的基本结构就是顺序结构，除非特别指明，否则就按照顺序一句一句的执行

### 2.3 if选择结构

```java
import java.util.Scanner;

public class demo03 {
    public static void main(String[] args) {
        Scanner scanner=new Scanner(System.in);

        System.out.println("请输入成绩：");
        int score=scanner.nextInt();

        if(score>90)
            System.out.println("优秀");
        else if(score>80){
            System.out.println("良好");
        }
        else if(score>60){
            System.out.println("及格");
        }
        else{
            System.out.println("不及格");
        }
    }
}

//请输入成绩：
//77
//及格
```

### 2.4 Switch语句结构

switch case语句判断一个变量与一系列值中某个值是否相等，每个值称为一个分支。

switch语句中的变量类型可以是：byte、short、int、char

case标签必须为字符串常量或字面量

```java
import java.util.Scanner;

public class demo04 {
    public static void main(String[] args) {
        Scanner sc=new Scanner(System.in);
        String grade=sc.next();
//        char grade='C';

        switch (grade) {
            case "A":
                System.out.println("优秀");
                break;
            case "B":
                System.out.println("良好");
                break;
            case "C":
                System.out.println("及格");
                break;
            case "D":
                System.out.println("不及格");
                break;
            default:
                System.out.println("找不到");
                break;
        }
    }
}
```

### 2.5 while循环

```java
while(布尔表达式){
	//循环内容
}
```

```java
do{
	//语句
}while(布尔表达式)
```

### 2.6 for循环

for循环语句是支持迭代的一种通用结构，是最有效，最灵活的循环结构

for循环执行的次数是在执行前就确定的，语法格式如下：

```java
for(初始化；布尔表达式；更新){
    代码语句
}
```

### 2.7 增强for循环

```java
for(声明语句:表达式){
    //代码
}
```

声明语句：声明新的局部变量，该变量的类型必须与数组的类型

### 2.8 break,continue

break在任何循环语句的主体部分，均可用break控制循环的流程。break用于强制退出循环，不执行循环语句中剩余的语句；

continue语句用在循环语句体中，用于终止某次循环，即跳过循环体中尚未执行的语句，接着进行下一次是否执行循环的判定。

## 三、方法

### 3.1 方法概念

Java方法是语句的集合，他们在一起执行一个功能：

* 方法是解决一类问题的步骤的有序组合
* 方法包含于类或对象中
* 方法在程序中被创建，在其他地方被引用

方法的本意是功能块，就是实现某个功能的语句块的集合。我们设计方法的时候，最好保持方法的原子性，就是一个方法只完成一个功能，利于后期的扩展。

方法包含一个一个方法头和一个方法体。下面是一个方法的所有部分：

```java
修饰符 返回值类型 方法名(参数类型 参数名){
    ...
    方法体
    ...
    return 返回值;
}
```

* 修饰符：可选的，告诉编译器如何调用该方法，定义了该方法的访问类型
* 返回值类型：方法可能会返回值
* 方法名：方法的实际名称，方法名和参数表共同构成方法签名
* 参数类型：参数像是一个占位符，当方法被调用时，传递值给参数。这个值被称为实参或变量。参数列表是指方法的参数类型、顺序和参数的个数。参数是可选的，方法不可以包含任何参数
  * 形式参数：在方法被调用时用于接受外界输入的数据
  * 实参：调用方法时实际传给方法的是数据
* 方法体：方法体包含具体的语句，定义该方法的功能

方法调用：对象名.方法名（实参列表）

### 3.2 方法的重载

重载就是在一个类中，由相同的函数名称，但形参不同的函数

方法的重载规则：

* 方法名称必须相同
* 参数列表必须不同（个数不同、类型不同、参数排列顺序不同等）
* 方法的返回类型可以相同，也可以不相同
* 仅仅返回值类型不同不足以成为方法的重载

方法的重载的实现理论：

方法名称相同时，编译器会根据调用方法的参数个数、参数类型等去逐个匹配，以选择对应的方法，如果匹配失败，则编译器报错

**可变参数**：在方法声明中，在指定参数类型后加一个省略号（...)

一个方法只能指定一个可变参数，他必须是方法的最后一个参数。任何普通的参数必须在它之前声明。

### 3.3 递归

递归就是自己调用自己。

递归结构包括两个部分：

递归头：什么时候不调用自身方法，如果没有头，将陷入死循环

递归体：什么时候调用自身方法

### 3.4方法的调用



## 四、数组

### 4.1数组声明与创建

数组是相同类型数据的有序集合

数组描述的是相同类型的若干个数据，按照一定的先后次序排列组合而成；其中，每一个数据称作一个数组元素，每个数组元素可以通过一个下标来访问它们

数组声明与创建：

```java
dataType[] arrayReVar = new dataType[arraySize];
```

数组的元素是通过索引访问的，数组索引从0开始

获取数组长度：array.length();

### 4.2 数组初始化

1. 静态初始化

   ```java
   int []a={1,2,3};
   ```

2. 动态初始化

   ```java
   int [] a=new int[2];
   a[0]=1;
   a[1]=2;
   ```

3. 数组的默认初始化

   数组是引用类型，它的元素相当于类的实例变量，因此数组一经分配空间，其中的每个元素也被按照实例变量同样的方式被隐式初始化

### 4.3 数组特点

* 其长度是确定的，数组一旦被创建，它的大小就是不可以改变的
* 其元素必须是相同类型，不允许出现混合类型
* 数组中的元素可以是任何数据类型，包括基本类型和引用类型
* 数组变量属于引用类型，数组也可以看成是对象，数组中的每个元素相当于该对象的成员变量。数组本身就是对象，java中对象是在堆中，因此数组无论保持原始类型还是其他对象类型，数组对象本身是在堆中
* 下标的合法区间：[0,length-1]，如果越界就会报错：ArrayIndexOutoBounds

### 4.4 多维数组

多维数组可以看成是数组中的数组，比如二维数组就是一个特殊的一维数组，其每一个元素都是一个一维数组

二维数组：

```java
int a[][]=new int[2][5];
```

### 4.5 Arrays类

数组的工具类java.util.Arrays

Arrays类中的方法都是static修饰的静态方法，在使用的时候可以直接使用类名进行调用，而不是使用对象来调用

常用功能：

* 对数组赋值：通过fill方法
* 对数组排序：通过sort方法，按升序
* 比较数组：通过equals方法比较数组中元素值是否相等
* 查找数组元素：通过binarySearch方法能对排序好的数组进行二分查找法比较

## 五、面向对象

### 5.1 初识面向对象

对于描述复杂的事物，为了从宏观上把控、从整体上合理分析，我们需要使用面向对象的思路来分析整个系统。但是具体到微观操作，仍然需要面向过程的思路去处理。

面向对象的本质：以类的方式组织代码，以对象的组织封装数据

### 5.2 创建与初始化对象

类是一种抽象的数据类型，它是对某一类事物整体描述/定义，但并不能代表某一个具体的事物

对象是抽象概念的具体实例

* 使用new关键字创建对象
* 使用new关键字创建的时候，除了分配内存空间之外，还会给创建好的对象进行默认的初始化以及对类中构造器的调用
* 类中构造器也称为构造方法，是在进行创建对象的时候必须要调用的。并且构造器有一下俩个特点”
  1. 必须和类的名字相同
  2. 必须没有返回类型，也不能写void

定义有参构造之后，如果想使用无参构造，显示的定义一个无参的构造

### 5.3 三大特性

#### 5.3.1 封装

该露的露，该藏的藏

程序设计要追求“高内聚，低耦合”。高内聚就是类的内部数据操作细节自己完成，不允许外部干涉；低耦合：仅暴露少量的方法给外部使用

通常，应禁止直接访问一个对象中数据的实际表示，而因通过操作接口来访问，这称为信息隐藏

==属性私有，get/set==

```java
public class Application {
    public static void main(String[] args) {
        Student student_1 = new Student();

        student_1.setName("高鑫");
        System.out.println(student_1.getName());
    }
}

public class Student {
    //名字、序号、性别、score、sleep、study
    private String name;
    private int id;
    private char sex;
    //属性私有

    //提供一些可以操作私有属性的方法
    public String getName() {
        return this.name;
    }

    public void setName(String  name) {
        this.name = name;
    }
}
```

#### 5.3.2 继承

extends的意思是扩展，子类是父类的扩展

java中类只有单继承，没有多继承

* 继承是类和类之间的一种关系，类和类之间的关系还有依赖、组合、聚合等
* 继承关系的两个类，一个为子类（派生类），一个为父类（基类）。子类继承父类，使用关键字extends来表示
* 子类和父类之间，从意义上讲应该具有”is a“的关系

object类：

super：

* 调用父类的构造方法，必须在构造方法的第一个
* super只能出现在子类的方法和构造方法中
* super和this不能同时调用构造方法

方法重写：需要有继承关系，子类重写父类的方法

* 方法名必须相同
* 参数列表必须相同
* 修饰符：范围可以扩大但不能缩小：public>protected>default>private
* 抛出的异常：范围可以被缩小，但不能扩大
* 子类的方法和父类的一致，方法体不同

#### 5.3.3 多态

即同一方法可以根据发送对象的不同而采用多种不同的行为方式

一个对象的实际类型是确定的，但可以指向对象的引用类型有很多

注：

* 多态是方法的多态，属性没有多态
* 存在条件：继承关系，方法才能重写，父类的引用指向子类对象

### 5.4 instanceof和类型转换

* 父类引用指向子类对象
* 把子类转换为父类，向上转型，直接转换；把父类转换为子类，向下转型，强制转换；
* 方便方法的调用，减少重复的代码

### 5.5 static关键字

### 5.6抽象类

abstract修饰符可以用来修饰方法也可以修饰类，如果修饰方法，那么该方法就是抽象方法；如果修饰类，那么该类是抽象类

抽象类中可以没有抽象方法，但是有抽象方法的类一定要声明为抽象类

* 抽象类，不能使用new关键字来创建对象，它是用来让子类继承的
* 抽象方法，只有方法的声明，没有方法的实现，它是用来让子类实现的

子类继承抽象类，那么就必须要实现抽象类没有实现的抽象方法，否则该子类也要声明为抽象类

注：

* 不能new抽象类，只能靠子类实现它* 
* 抽象类中只能写普通的方法
* 抽象方法必须在抽象类中

### 5.7 接口

接口就是规范，定义的是一组规则；接口的本质是契约。

声明类的关键字是class，声明接口的关键字是interface

接口的作用：

* 约束
* 定义一些方法，让不同的人实现
* 里面的方法都是public abstract，里面的属性都是final public static
* 接口不能被实例化，接口没有构造方法
* implements可以实现多个接口
* 必须重写接口中的方法

### 5.8 内部类

内部类就是在一个类中的内部再定义一个类

1. 成员内部类
2. 静态内部类
3. 局部静态类
4. 匿名内部类

## 六、异常

软件程序在运行过程中，非常可能会遇到异常问题

异常指程序在运行过程中出现的不期而至的各种状况，如：文件找不到、网络连接失败、非法参数等

异常分类:

1. 检查性异常：最具代表的检查型异常是用户错误或问题引起的异常，这是程序员无法预见的。
2. 运行时异常：运行时异常是可能被程序员避免的异常。与检查型异常相反，运行时异常可以在编译时被忽略。
3. 出错：错误不是异常，而是脱离程序员控制的问题。错误的代码中通常被忽略。

异常处理框架：Java把异常当作对象来处理，并定义一个基类java.lang.Throwable作为异常的超类

1. 抛出异常
2. 捕获异常

异常处理5个关键字：try,catch,finally,throw,throws

```java
try{//try监控区域
        if(b==0){
throw new Exception();
    }
            System.out.println(a/b);
}catch(Exception e){//catch捕获的异常
System.out.print("程序出现异常：");
    System.out.println(e);
}finally{//处理善后工作
        System.out.println("finally");
}
```













