# Java数据类型（八种基本数据类型 + 四种引用类型）、数据类型转换

## 1.总览

- Java

  的数据类型只有两大类：8大基本数据类型与引用数据类型

  。其中基本数据类型又被称为值类型

  - 基本数据类型：6种数字类型（byte/short/int/long/float/double）、1种字符型（char）、1种布尔型（boolean）
  - 引用数据类型：类（Class）、接口（Interface）、数组（Array）
  - 除了以上的基本数据类型和引用数据类型，还有一些其他相关的数据类型，例如字符串类型String、枚举类型Enum，它们都是基于引用数据类型来实现的

- **基本数据类型只能存自己类型的值，无其他额外功能**

- 引用类型：参数传递时，以拷贝引用地址的方式传递给接收变量，而非复制整个数据本体。**除八大基本数据类型之外的所有数据类型，都为引用数据类型**。所有引用数据类型的默认值都为null。

- 为了基本数据类型可以与引用数据类型互相转换、以利用彼此的特性，java为每一种基本数据类型提供了相应的包装类。包装类对基本数据类型进行了封装，提供了丰富的功能，**包装类是基本类型的拓展**

- 包装类是引用类型的一种，包装类与基本数据类型一一对应，也有8种，分别为：Byte、Short、Integer、Long、Float、Double、Character、Boolean

## 2.基本数据类型

### 2.1 类型概述

- 6种数字类型
  - 4种整数型：byte、short、int、long
  - 2种浮点型：float、double
- 1种字符类型：char
- 1种布尔型：boolean

### 2.2 基本数据类型详解

| 基本类型 | 存储大小                   | 初始化默认值                                                 | 取值范围                                                   | 包装类型  |
| -------- | -------------------------- | ------------------------------------------------------------ | ---------------------------------------------------------- | --------- |
| byte     | 1字节（8位）               | 0                                                            | -128~127                                                   | Byte      |
| short    | 2字节（16位）              | 0                                                            | -32768~32767                                               | Short     |
| int      | 4字节（32位）              | 0                                                            | -2^31 ~ 2^31 - 1                                           | Integer   |
| long     | 8字节（64位）              | 0L。"L"理论上不分大小写，但若写成"l"容易与数字"1"混淆，不容易分辨，所以最好大写。 | -2^63 ~ 2^63 - 1                                           | Long      |
| float    | 4字节（32位）              | 0.0f                                                         | 符合IEEE754标准的浮点数，1.4E-45 ~ 3.4028235E38            | Float     |
| double   | 8字节（64位）              | 0.0d                                                         | 符合IEEE754标准的浮点数，4.9E-324 ~ 1.7976931348623157E308 | Double    |
| char     | 2字节（16位）              | '\u0000'                                                     | \u0000 ~ \uffff（十进制等效值为 0~65535，本质也是数值）    | Character |
| boolean  | 1字节（8位）/4字节（32位） | false                                                        | true/false                                                 | Boolean   |

float、double不能用来表示精确的值，运算不精确——>解决方案：BigDecimal。

char 数据类型可以储存任何字符。

对于数值类型的基本类型的取值范围，我们无需强制去记忆，因为它们的值都已经以常量的形式定义在对应的包装类中

代码语言：java

复制

```
//long
System.out.println("基本类型：long 二进制位数：" + Long.SIZE);
System.out.println("包装类：java.lang.Long");
System.out.println("最小值：Long.MIN_VALUE=" + Long.MIN_VALUE);
System.out.println("最大值：Long.MAX_VALUE=" + Long.MAX_VALUE);
System.out.println();

//double
System.out.println("基本类型：double 二进制位数：" + Double.SIZE);
System.out.println("包装类：java.lang.Double");
System.out.println("最小值：Double.MIN_VALUE=" + Double.MIN_VALUE);
System.out.println("最大值：Double.MAX_VALUE=" + Double.MAX_VALUE);
System.out.println();

//其他类型方法同上类似
```

### 2.3 基本数据类型与引用数据类型区别

- 存储方式：基本数据类型直接存储值，而引用数据类型存储的是对象的引用（内存地址）
- 内存分配：基本数据类型在栈上分配内存，引用数据类型在堆上分配内存（具体内容存放在堆中，栈中存放的是其具体内容所在内存的地址）。栈上的分配速度较快，但是内存空间较小，而堆上的分配速度较慢，但可以分配更大的内存空间
- 默认值：基本数据类型会有默认值，例如int类型的默认值是0，boolean类型的默认值是false。而引用数据类型的默认值是null，表示没有引用指向任何对象
- 复制操作：基本数据类型进行复制时，会复制该变量的值。而引用数据类型进行复制时，只会复制对象的引用，两个变量指向同一个对象
- 参数传递：基本数据类型作为方法的参数传递时，传递的是值的副本，不会修改原始值。而引用数据类型作为方法的参数传递时，传递的是对象的引用，可以修改对象的属性或状态
- 比较操作：基本数据类型使用\==进行比较时，比较的是值是否相等。而引用数据类型使用\==进行比较时，比较的是引用是否指向同一个对象，如果要比较对象的内容是否相同，需要使用equals()方法

注意：Java中的包装类（Wrapper Classes）对基本数据类型进行了封装，使其也具有了对象的特性，可以调用方法和进行类型转换等操作。

![img](assets/d71964489a56c83d22b87b9f4a5ee8e5.png)

### 2.4 基本数据类型与包装类区别

- 存储方式：基本类型直接存储值，而包装类型存储的是对应基本类型值的对象。
- 空值处理：基本类型没有空值（null）的概念，而包装类型可以将null作为有效值来表示缺失或无效值。
- 默认值：基本类型有默认值，例如int类型的默认值是0，boolean类型的默认值是false。而包装类型的默认值是null。
- 对象操作：基本类型不能直接调用方法，而包装类型可以调用对应的方法，例如Integer类的intValue()方法可以获取保存在Integer对象中的值。
- 自动装箱/拆箱：基本类型和包装类型之间可以进行自动装箱和拆箱的转换。自动装箱是指将基本类型的值自动转换为对应的包装类型对象，如int 转Integer，Integer integer = 100，底层调用了Interger.valueOf(100)方法；而自动拆箱则是将包装类型对象自动转换为基本类型的值。
- 泛型支持：泛型只能使用引用类型，不能直接使用基本类型。因此，当需要在泛型中使用基本类型时，需要使用对应的包装类型。
- 比较方式：基本类型使用\==进行比较时，比较的是值是否相等。而包装类型使用\==进行比较时，比较的是引用是否指向同一个对象，而不是比较值是否相等。若要比较包装类型的值是否相等，需要使用equals()方法。

注意：在Java 5及其之后的版本中，基本类型和包装类型之间的转换会通过自动装箱、拆箱来自动进行，使得基本类型和包装类型之间的使用更加方便

## 3.数据类型转换

Java中的数据转换主要分为两种：自动类型转换（也称为隐式转换）、强制类型转换（也称为显式转换）。

转换从低级到高级：byte、short、char（三者同级）——> int ——> long ——> float ——> double

- 自动类型转换：代码无需任何处理，在代码编译时 编译器会自动进行处理。特点——**低级转换高级**。
- 强制类型转换：需要在待转换数据类型前 使用 (type)value， type是要强制类型转换后的数据类型，可能会导致溢出或损失精度 。特点——**高级转换低级**。

**数据类型转换必须满足如下规则**：

- 1. 不能对boolean类型进行类型转换。
- 1. 不能把对象类型转换成不相关类的对象。
- 1. 在把容量大的类型转换为容量小的类型时必须使用强制类型转换。
- 1. 转换过程中可能导致溢出或损失精度，例如：int i = 128; byte b = (byte)i;因为 byte 类型是 8 位，最大值为127，所以当 int 强制转换为 byte 类型时，值 128 时候就会导致溢出。
- 1. 浮点数到整数的转换是通过舍弃小数得到，而不是四舍五入，例如：(int)23.7 == 23;(int)-45.89f == -45;

### 3.1 具体示例

int 和 long 互转、int和double互转、int和byte互转、int和char互转、int和String互转

代码语言：java

复制

```java
public static void main(String[] args) {
    int aInt = 20;
    long bLong = 50L;
    double cDouble = 4.8;

    //低优先级类型数据 + 高优先级类型数据 ——> 结果会自动转换为高优先级数据
    long sum = aInt + bLong;

    //long -> int 需要强制类型转换
    int d = (int) bLong;
    //double -> int 需要强制类型转换
    int e = (int) cDouble;

    System.out.println("自动类型转换 int—>long:  " + sum);
    System.out.println("强制类型转换 long—>int:  " + d);
    System.out.println("强制类型转换 double—>int:  " + e);
    System.out.println();


    //int 和 byte 转换
    byte fByte = (byte) aInt;  //高转低，强转
    int gInt = fByte;          //低转高，自动
    System.out.println("高转低-强转，int->byte:  " + fByte);
    System.out.println("低转高-自动，byte->int:  " + gInt);
    System.out.println();

    //int 和 char 转换
    char hChar = 'a';
    int iInt = hChar;
    char j = (char) iInt;
    System.out.println("低转高-自动，char->int:  " + iInt);
    System.out.println("高转低-强转，int->char:  " + j);
    System.out.println();

    //int 和 String 转换
    //int转String： 1）使用String的ValueOf方法   2）直接使用 String类+ (即字符串拼接)，任意字符串和其他类型"+" 都会把其他类型转为字符串
    String str1 = String.valueOf(aInt);
    String str2 = "" + aInt;
    System.out.println("int转String： " + str1 + ",  " + str2);
    //String转int：调用包装类的Integer.parseInt方法，当字符串中包含非数字时会出错
    String str3 = "18";
    int k = Integer.parseInt(str3);
    System.out.println("String转int： " + k);
    System.out.println();

    //byte 和 char 互转
    byte m = (byte) hChar;
    char n = (char) m;
    System.out.println("char->byte，强转：  " + m);
    System.out.println("byte->char，强转：  " + n);
}
```

输出：

代码语言：txt

复制

```
自动类型转换 int—>long:  70
强制类型转换 long—>int:  50
强制类型转换 double—>int:  4

高转低-强转，int->byte:  20
低转高-自动，byte->int:  20

低转高-自动，char->int:  97
高转低-强转，int->char:  a

int转String： 20,  20
String转int： 18

char->byte，强转：  97
byte->char，强转：  a
```