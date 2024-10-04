

# python基本语法

##一、你好python

###  初始编程语言

编程语言是人类和计算机交流的一种专有领域语言

```python
print("hello World")
```

将想法转换为编程语言，通过解释器翻译成二进制提交计算机执行



## 二、Python基本语法

### 2.1字面量

在代码中，被写下来的固定的值，称之为字面量。

Python常用的有6种值的类型：

| 类型               | 描述                     |
| ------------------ | ------------------------ |
| 数字（Number）     | 整数、浮点数、复数、布尔 |
| 字符串（String）   | 描述文本的一种数据类型   |
| 列表（List）       | 有序的可变序列           |
| 元组（Tuple）      | 有序的不可变序列         |
| 集合（Set）        | 无序不重复集合           |
| 字典（Dictionary） | 无序Key-Value集合        |

字符串：又称文本，只有任意数量的字符如中文、英文等各类符号组成。

### 2.2注释

注释：在程序代码中对程序代码解释说明的文字

作用：注释不会被执行，让别人可以看懂代码的作用，能够大大增强程序的可读性

```python
#整数字面量
555
#浮点数字面量
34.23
#字符串字面量
"高鑫"

"""
我是一个多行注释
555
"""
```

### 2.3变量

在程序执行中，能存储计算结果或能表示值的抽象概念。

变量的值可以改变。

```python
a=5
#存储的目的是为了重复使用
print("内容1"，"内容2","内容3","内容4")
#print语句输出多份内容
```

### 2.4数据类型

数据是有类型的<u>，type(被查看的数据)可以得到数据的数据类型</u>。

```python
name="高鑫"
name_type=type(name)
print(name_type)
#会输出<class 'str'>
```

字符串类型的的不同定义方法：

```python
#双引号定义法
text1="字符串文本"
#单引号定义法
text2='字符串文本2'
#三引号定义法
text2="""字符串文本3（既能做注释，又是字符串）"""
```

### 2.4数据类型转换

数据之间在一些特定的场景之下，是需要相互转换的：

* 从文件中读取的数字，默认是字符串，需要我们手动转换成数字类型
* input()语句，默认也是字符串，也需要手动转换
* 将数字转换成字符串，写出到外部系统

| 语句（函数） | 说明            |
| ------------ | --------------- |
| int(x)       | 将x转换成整数   |
| float(x)     | 将x转换成浮点数 |
| str(x)       | 将x转换成字符串 |

注：

* 任何类型，都可以通过str（）转换成字符串
* 字符串内必须是真数字，才可以将字符串转换为数字
* 浮点数转换成整数，会丢失精度，也就是小数部分

### 2.5字符串扩展

#### 2.5.1引号的嵌套

* 可以使用\来进行转义
* 单引号内可以写双引号，双引号内可以写单引号

#### 2.5.2 字符串拼接

可以使用“+”号连接字符串变量或字符串字面量，无法和非字符串类型进行拼接

```python
name="高鑫"
year=2003
print("我是："+name+"，今年大三了")# 可以
print("我是："+name+"，今年大三了,出生于："+year)# 会报错
```

#### 2.5.3 字符串格式化

可以通过%s进行占位，实现字符串与变量的快速拼接

| 格式符号 | 转化                                 |
| -------- | ------------------------------------ |
| %s       | 将内容转换成字符串类型，放入占位位置 |
| %d       | 将内容转换成整型，放入占位位置       |
| %f       | 将内容转换成浮点型，放入占位位置     |

注：多个变量占位，变量要用括号括起来，并按照占位的顺序填入。

```python
name="高鑫"
year=2003
school="清华大学" # doge
score=98.88
print("我是%s，出生于：%d，毕业于%s，我的毕业成绩是%f，"%(name,year,school,score))
# 我是高鑫，出生于：2003，毕业于清华大学，我的毕业成绩是98.880000，
```

换可以通过f"{变量}{变量}"的方式快速格式化，这种方式不需要理会类型，也不需要精度处理。

```python
name="高鑫"
year=2003
school="清华大学" # doge
score=98.88
print(f"我是{name}，出生于：{year}，毕业于{school}，我的毕业成绩是{score}")
# 我是高鑫，出生于：2003，毕业于清华大学，我的毕业成绩是98.88
```

#### 2.5.4 格式化的精准控制

m.n的形式控制，如%d，%5.2f，%.2f,m和n均可以省略。

当m比数字本身的宽度还小时，m不生效

.n会对小数部分做精度控制，同时在做精度控制的同时，还会对小数部分四舍五入。

#### 2.5.5  表达式

表达式就是一个具有明确结果的代码语句，如 1 + 1、type(“字符串”)、3 * 5等在变量定义的时候，如 age = 11 + 11，等号右侧的就是表达式，也就是有具体的结果，将结果赋值给了等号左侧的变量。

### 2.6 数据输入

数据输入：input；数据输出：print。

可以使用input()语句从键盘获取输入，使用一个变量接收input语句获取键盘输入数据即可

* input()语句的功能是，获取键盘输入的数据
*  可以使用：input(提示信息)，用以在使用者输入内容之前显示提示信息。*
*  要注意，无论键盘输入什么类型的数据，获取到的数据永远都是字符串类型

```python
name=input("你的名字:")
print(f"我的名字是{name},这么巧")
#你的名字:韦一敏
#我的名字是韦一敏,这么巧
```



## 三、Python判断语法

### 3.1布尔类型

True：真

False：假

布尔类型不仅可以自行定义，还可以通过使用比较运算符得到布尔类型的结果。

### 3.2 if语句

```python
if 条件表达式1:
    语句序列1
elif 条件表达式2:
    语句序列2
else:
    语句序列3
```

* 判断互斥是有顺序的，按1，2，3的顺序判断
* elif可以写多个
* else后，不需要判断条件
* if，elif，else代码块后需要加4个代码块作为缩进

## 四、Python循环语法

### 4.1while循环语法

```python
while 条件:
    语句序列1
    语句序列2 
    语句序列1
    ····
```

条件提供布尔类型结果，True继续,False停止。

### 4.2for循环的基础语法

while与for是循环的区别

* while循环的循环条件是自定义的，自行控制判断条件
* for循环是一种轮询机制，是对一批内容进行“逐个处理”
* for循环的临时变量其作用域限定为循环内（实际上可以访问，但在编程规范上不允许）

```python
for 临时变量 in 待处理数据集:
    执行代码
```

遍历字符串：

```python
name = "gapxin"
for x in name:
    print(x)
    print('高鑫666')
#g    
#高鑫666
#a
#高鑫666
#p
#高鑫666
#x
#高鑫666
#i
#高鑫666
#n
#高鑫666
```

### 4.3range语句

语法中的待处理数据集，可称之为可迭代类型，可迭代类型指其内容可以一个个依次取出的一种类型，包括，字符串、列表、元组等。

1. `range(num)`

   获取一个从0开始，到num结束的数字序列（不含num本身）

   如range(5)取得的数据是：[0, 1, 2, 3, 4]

2. `range(num1, num2)`

   获得一个从num1开始，到num2结束的数字序列（不含num2本身）

   如，range(5, 10)取得的数据是：[5, 6, 7, 8, 9]

3. range(num1,num2,step)

   获得一个从num1开始，到num2结束的数字序列（不含num2本身）

   数字之间的步长，以step为准（step默认为1）如，range(5, 10, 2)取得的数据是：[5, 7, 9]

for循环遍历range序列

```python
i = 0
for i in range(5):
    print(i, end=" ")
#0 1 2 3 4 
```

### 4.4循环中断

#### 4.4.1continue语句

中断本次循环直接进入下一次循环

```python
for i in "gaoxin":
    print (i,end=" ")
    continue
    print("高鑫")
# g a o x i n 
#continue后的语句并没有执行
```

#### 4.4.2 break语句

直接结束所在的循环

```python
for i in "gaoxin":
    print (i,end=" ")
    break
    print("高鑫")
# g
#直接跳出了所在的循环
```

注：

* continue和break在for和while循环中作用一致
* 在循环嵌套中，只能作用在所在的循环，无法对上层循环起作用

## 五、Python函数

### 4.1函数介绍

函数是组织好，可重复使用，用来实现特定功能的代码片段

好处：将功能封装在函数内，可供随时随地的重复使用；提高代码的复用性，减少重复代码，提高开发效率。

```python
def 函数名（传入参数）:
    函数体
    return 返回值
```

1. 参数或返回值如果不需要，可以省略
2. 函数必须先定义，后使用

### 4.2函数的传入参数与返回值

#### 4.2.1函数的参数

传入参数的功能：在函数进行计算的时候，接受外部提供的数据。

```python
def add(x,y):
    result =x+y
    print(f"结果是{result}")
print(add(5,6))
#结果是11
```

* 参数之间使用逗号隔开
* 传入的时候按照顺序传入数据，也使用逗号隔开

注：

1. 函数中定义的参数，称之为形式参数，函数中调用的参数，称之为实际参数
2. 传入的时候要与形参一一对应，使用逗号隔开

#### 4.2.2 函数的返回值

返回值就是指程序中函数完成特定的功能后，最后给调用者的结果。

```python
def add(x,y):
    result =x+y
    return result
    print("gaoxin")
print(add(5,6))
# 11
```

* 函数体在遇到return后就结束了，不会执行后面的语句
* 不使用return语句时，函数的返回值为None。

### 4.3函数的嵌套使用

在一个函数中，调用另外一个函数

```python
def func_b():
    print("--2--")
    func_c()

def func_a():
    print("--1--")
    func_b()
    print("--end--")

def func_c():
    print("--3--")

func_a()
#--1--
#--2--
#--3--
#--end--
```

函数A中执行到调用函数B的语句，B,会调用函数C里的语句，执行完函数C后的语句，在执行函数B的语句，会将函数B全部执行完成后，继续执行函数A的剩余内容。

### 4.4变量的作用域

变量作用域指的是变量的作用范围（变量在哪里可用，在哪里不可用），主要分为两类：局部变量和全局变量。

局部变量是定义==在函数体内部的变量，即只在函数体内部生效==

局部变量的作用：在函数体内部，**临时保存数据**，即当函数调用完成后，则销毁局部变量

全局变量，指的是在函数体内、外都能生效的变量

**global关键字**

使用global可以在函数内部声明变量为全局变量

```python
num=100

def testA():
    print(num)

def testB():
    global num
    num=200
    print(num)

testA()
testB()
print(f"全局变量num={num}")
#100
#200
#全局变量num=200（不加globa时，全局变量不会变，还为100)
```



## 六、Python数据容器

python中的数据容器是：一种可以容纳多个数据的数据类型。

### 6.1 list容器

#### 6.1.1列表介绍

列表的定义语法：[元素一,元素二,元素三,元素四,元素五]

且元素的类型没有限制，甚至元素也可以是列表，这样就定义了嵌套列表。

列表中的每个位置，都有其位置下标索引，从前向后，从0开始，依次递增。

特点：

- 可以容纳多个数据和不同种类的数据
- 数据是有序存储的，有下标序号
- 允许重复数字存在，不可修改

```python
name_list = ['gaoxin','gaosang','xinxin']

print(name_list[0])
print(name_list[1])
print(name_list[2])

#gaoxin
#gaosang
#xinxin

```

从前向后，编号从-1开始递增：

```python
name_list = ['gaoxin','gaosang','xinxin']
print(name_list[-1])
print(name_list[-2])
print(name_list[-3])

#xinxin
#gaosang
#gaoxin
```

list容器的多层嵌套

```python
my_list=[[1,2,3],[4,5,6]]
print(my_list[0])
print(my_list[1][2])

#[1, 2, 3]
#6
```

注：注意下标索引的取值范围，超出范围无法取出元素，并且会报错。

#### 6.1.2 列表的常用操作

##### 6.1.2.1查询

查找某元素的下标，查不到，报错ValueError

==列表.index(元素==

```python
name_list = ['gaoxin','gaosang','xinxin']
print(name_list.index("gaoxin"))
# 0
```

##### 6.1.2.2修改

1.修改特定位置（索引）的元素值

==列表[小标]=值==

```python
name_list = ['gaoxin','gaosang','xinxin']

name_list[2]='gaogao'
print(name_list)
#['gaoxin', 'gaosang', 'gaogao' ]
```

2.在指定的位置插入元素

==列表.insert(下标，元素)==

```python
name_list = ['gaoxin','gaosang','xinxin']

name_list.insert(1,"gaogao")
print(name_list)
# ['gaoxin', 'gaogao', 'gaosang', 'xinxin']
```

3.将指定元素追加到列表的尾部

==列表.append(元素）==

```python
name_list = ['gaoxin','gaosang','xinxin']

name_list.append("gaogao")
print(name_list)
# ['gaoxin', 'gaosang', 'xinxin', 'gaogao']
```

4.将其他数据容器的内容取出，依次追加到列表尾部

==列表.extend(其他数据容器)==

```python
name_list = ['gaoxin','gaosang','xinxin']

name_list.extend(["gaogao","高鑫"])
print(name_list)
# ['gaoxin', 'gaosang', 'xinxin', 'gaogao', '高鑫']
```

5.删除元素

==del 列表[下表]==或==列表.pop(下标)==\

```python
name_list = ['gaoxin','gaosang','xinxin']

del name_list[0]
print(name_list)
# ['gaosang', 'xinxin']
name_list.pop(0)
print(name_list)
# ['xinxin']

```

6.删除元素在列表中的第一个匹配项

==列表.remove(元素)==

```python
name_list = ['gaoxin','gaosang','xinxin']

name_list.remove("xinxin")
print(name_list)

['gaoxin', 'gaosang']
```

7.清空列表内容

==列表.clear()==

```python
name_list = ['gaoxin','gaosang','xinxin']

name_list.clear()
print(name_list)

# []

```

##### 6.1.2.3统计

1.统计某元素在列表中的数量

==列表.count(元素)==

```python
name_list = ['gaoxin','gaosang','xinxin',"gaoxin"]

print(name_list.count("gaoxin"))

# 2 
```

2.统计列表内的元素总数

==len(列表)==

```python
name_list = ['gaoxin','gaosang','xinxin',"gaoxin"]

print(len(name_list))

# 4
```

#### 6.1.3 列表的遍历

将数据内的元素依次取出，并处理，称之为遍历

while循环处理：

```
index=0
while index < len(list):
    element=list[index]
    execute statement
    index+=1
```

for循环处理：

```
for i in list:
    execute:
    # print(i)
```

区别：while循环可以自定义循环条件，并自行控制，可以通过条件控制做到无限循环；for循环不可以自定义循环条件，理论上不可以做到无限循环，被遍历的容器不是无限的。

for用于从容器内依次取出元素并处理，while用以任何需要循环的场所。



### 6.2 tuple容器

元组一旦定义就不可修改

定义元组字面量语法：变量名称=（元素1，元素2，元素3……）

元组支持方法：index(),count(),len(元组)

```python
t1=(1,2,3,4,4,5)

print(t1[2])
print(t1.index(1))
print(t1.count(4))
print(len(t1))

# 3
# 0
# 2
# 6
```



注：

* 不可以修改元组的内容，否则直接报错
* 可以修改元组内list的内容
* 不可以替换list为其他list或其他类型
* 支持for循环和while循环
* 允许重复数据存在



### 6.3 str容器

和列表、元组一样，字符串也支持下标访问，但它也是一个无法修改的数据容器

定义元组语法：变量名称=" "



1. 查找特定字符的下标索引值

   ==字符串.index(字符串)==

   ```python
   my_str="heima and cast"
   print(my_str.index("and"))

   #6
   ```

2. 字符串的替换

   ==字符串.replace(字符串1，字符串2)==

   将字符串的全部字符串1，替换为字符串2；这不是修改字符串1，而是得到一个新的字符串

   ```python
   name="gaogao gaoxin gaosang"
   new_name=name.replace("gao","xin")
   print(name)
   print(new_name)

   # gaogao gaoxin gaosang
   # xinxin xinxin xinsang
   ```

3. 字符串的分割

   ==字符串.split(分隔符字符串)==

   将字符串按照指定的分隔符字符串划分为多个字符串，并存入列表对象，字符串本身不变，而是得到了一个列表序列。

   ```python
   my_str="gaogao gaoxin gaosang"
   my_str_list=my_str.split(" ")
   print(my_str_list)
   print(type(my_str_list))

   # ['gaogao', 'gaoxin', 'gaosang']
   # <class 'list'>
   ```

4. 取出前后空格

   ==字符串.strip()==

   ```python
   my_str="    itheima and itcast   "
   print(my_str.strip())

   #itheima and itcast
   ```

5. 取出前后指定字符串

   ==字符串.strip(字符串)==

   ```python
   my_str="gaogao gaoxin gaosang"
   print(my_str.strip("g"))

   # aogao gaoxin gaosan
   ```

6. 统计字符串出现次数

   ==字符串.count(字符串)==

7. 统计字符串长度

   ==len(字符串)==

   ```python
   my_str="gaogao gaoxin gaosang"
   print(my_str.count('g'))
   print(len(my_str))

   # 5
   # 21
   ```

字符串容器可以容纳的类型是单一的，只能是字符串类型。

### 6.4序列

序列是内容连续、有序、支持下标索引的一类数据类型（列表、元组、字符串）

**序列切片：**从序列的指定位置开始，依次取出元素，到指定位置结束，得到一个新序列。

==序列[起始:结束:步长]==

* 起始可以省略，表示从头开始
* 结束可以省略，表示到尾结束
* 步长可以省略，表示步长为1，可以为负数，表示倒序执行

```python
my_list = [0, 1, 2, 3, 4, 5, 6]
result1=my_list[1:4:1]
print(result1)
# [1, 2, 3]

my_tuple=(0,1,2,3,4,5,6)
result2=my_tuple[:]
print(result2)
# (0, 1, 2, 3, 4, 5, 6)

my_str="01234567"
result3=my_str[::2]
print(result3)# 起始和结束省略，表示从头到尾
# 0246

```

### 6.5 set容器

集合定义语法：{元素，元素，元素，元素}

集合组主要特点：不主持元素的重复，自带去重功能，并且内容无序（因为要对元素做去重处理，无法保证顺序和创建的时候一致）

```python
my_set = {"传智教育","黑马程序员","itheima","传智教育","黑马程序员"
        ,"itheima","传智教育","黑马程序员","itheima",}
print(f"内容是{my_set},类型是{type(my_set)}")

# 内容是{'itheima', '传智教育', '黑马程序员'},类型是<class 'set'>
```

1.添加新元素

==集合.add(元素)==

```python
my_set={"小明","小航","小红","小亮"}
my_set.add("高鑫")
print(f"添加元素后：{my_set}")

# 添加元素后：{'小明', '小红', '小亮', '高鑫', '小航'}
```

2.移除元素

==集合.remove(元素)==

```python
my_set={"小明","小航","小红","小亮"}
my_set.remove("小红")
print(my_set)

# {'小航', '小亮', '小明'}
```

3.随机取出元素

==集合.pop()==

会得到一个元素的结果，同时集合本身被修改，元素被移除。

```python
my_set={"小明","小航","小红","小亮"}
element=my_set.pop()
print(f"取出的元素为{element},取出元素后：{my_set}")

# 取出的元素为小红,取出元素后：{'小明', '小亮', '小航'}
```

4.清空集合

==集合.clear()==

```python
my_set={"小明","小航","小红","小亮"}
my_set.clear()
print(my_set)

# set()
```

5.取出两个集合的差集

==集合1.difference(集合2)==

得到一个新集合，集合1和集合2不变

```python
set1={1,2,3,4}
set2={2,3,4,5}
set3=set1.difference(set2)
# set3中的内容是1里有，2里没有的
print(f"set1:{set1},set2:{set2},set3:{set3}")

# set1:{1, 2, 3, 4},set2:{2, 3, 4, 5},set3:{1}
```

6.消除两个集合的差集

==集合1.difference_update(集合2)==

对比集合1和集合2，在集合1内，删除集合2相同的元素，集合1被修改，集合保持不变

```python
set1={1,2,3,4}
set2={2,3,4,5}
set1.difference_update(set2)
print(f"set1:{set1},set2:{set2}")

# set1:{1},set2:{2, 3, 4, 5}
```

7.两个集合合并

==集合1.union(集合2)==

将集合1和集合2组成新集合，集合1和集合2保持不变

```python
set1={1,2,3,4}
set2={2,3,4,5}

set3=set1.union(set2)
print(f"set3:{set3}")

# set3:{1, 2, 3, 4, 5}
```

八、集合长度

==len(集合)==

统计集合内有多少元素，得到一个整数结果

```python
set1={1,2,3,4}
set2={1,2,3,4,5,4}

print(f"set1长度：{len(set1)},set2长度{len(set2)}")

# set1长度：4,set2长度5
```

集合不支持下标索引，所以也就不支持使用while循环，但支持for循环

### 6.6 dict容器

#### 6.6.1字典的定义

使用字典，可以实现用Key取出Value的操作。

字典的定义，同样使用{}，不过存储的元素是一个个的键值对：

{key:value,key:value,key:value,key:value}

注：1.Key和Value可以是任意类型的元素，Key不可为字典

​        2.Key不可重复，重复会对原有数据进行覆盖

​        3.字典同集合一样，不可以使用下标索引，但字典可以通过Key值来取得对应的Value

#### 6.6.2字典的基本操作

1. 更新元素和新增元素

   ==字典[Key]=Value==

   当是对已存在的Key执行上述操作时，就是更新Value值，如果是不存在的Key值，则是新增元素

   ```python
   my_dict = {"王力宏":99,"周杰伦":98,"林俊杰":53,"陶喆":33}
   print(my_dict)
   # {'王力宏': 99, '周杰伦': 98, '林俊杰': 53, '陶喆': 33}

   my_dict["王力宏"]=0
   print("修改后：",my_dict)
   # 修改后： {'王力宏': 0, '周杰伦': 98, '林俊杰': 53, '陶喆': 33}

   my_dict["邓紫棋"]=77
   print("新增后：",my_dict)
   新增后： {'王力宏': 0, '周杰伦': 98, '林俊杰': 53, '陶喆': 33, '邓紫棋': 77}
   ```

2. 删除元素

   ==字典.pop(key)==

   获取指定Key的Value，同时字典被修改，指定Key的数据被删除

   ```python
   my_dict = {"王力宏":99,"周杰伦":98,"林俊杰":53,"陶喆":33}
   print(my_dict)
   print(my_dict.pop("陶喆"))
   print("移除后：",my_dict)

   # {'王力宏': 99, '周杰伦': 98, '林俊杰': 53, '陶喆': 33}
   # 33
   # 移除后： {'王力宏': 99, '周杰伦': 98, '林俊杰': 53}
   ```

3. 清空字典

   ==字典.clear()==

   字典被修改，元素被清空

   ```python
   my_dict = {"王力宏":99,"周杰伦":98,"林俊杰":53,"陶喆":33}
   print(my_dict)

   my_dict.clear()
   print(my_dict)

   # {'王力宏': 99, '周杰伦': 98, '林俊杰': 53, '陶喆': 33}
   # {}
   ```

4. 获取全部的key

   ==字典.keys()==

   ```python
   my_dict = {"王力宏":99,"周杰伦":98,"林俊杰":53,"陶喆":33}
   print(my_dict)

   print(my_dict.keys())

   # {'王力宏': 99, '周杰伦': 98, '林俊杰': 53, '陶喆': 33}
   # dict_keys(['王力宏', '周杰伦', '林俊杰', '陶喆'])
   ```

5. 遍历字典

   字典不支持下标索引，所以同样不可以用while循环遍历

   ```python
   my_dict = {"王力宏":99,"周杰伦":98,"林俊杰":53,"陶喆":33}
   print(my_dict)

   keys=my_dict.keys()
   for key in keys:
       print(f"学生：{key}，成绩：{my_dict[key]}")
       
   # {'王力宏': 99, '周杰伦': 98, '林俊杰': 53, '陶喆': 33}
   # 学生：王力宏，成绩：99
   # 学生：周杰伦，成绩：98
   # 学生：林俊杰，成绩：53
   # 学生：陶喆，成绩：33
   ```

6. 字典全部元素的数量

   ==len(字典)==

   ```py
   my_dict = {"王力宏":99,"周杰伦":98,"林俊杰":53,"陶喆":33}
   print(len(my_dict))

   # 4
   ```

### 6.7总结

|          | 列表                         | 元组                           | 字符串             | 集合               | 字典                       |
| -------- | ---------------------------- | ------------------------------ | ------------------ | ------------------ | -------------------------- |
| 元素数量 | 支持多个                     | 支持多个                       | 支持多个           | 支持多个           | 支持多个                   |
| 元素类型 | 任意                         | 任意                           | 仅字符             | 任意               | 任意类型，但key不能是字典  |
| 下标索引 | 支持                         | 支持                           | 支持               | 不支持             | 不支持                     |
| 重复元素 | 支持                         | 支持                           | 支持               | 不支持             | 不支持                     |
| 可修改性 | 支持                         | 不支持                         | 不支持             | 不支持             | 不支持                     |
| 数据有序 | 是                           | 是                             | 是                 | 否                 | 否                         |
| 使用场景 | 可修改，可重复的一批数据场景 | 不可修改，可重复的一批数据场景 | 一批字符的记录场景 | 不可重复的数据场景 | 以Key检索Value数据记录场景 |

#### 6.7.1 通用统计功能

len(容器)：统计容器的元素的个数

max(容器)：统计容器的最大元素

min(容器)：统计容器的最小元素

#### 6.7.3容器的通用转换功能

list(容器):将指定容器转换为列表

str(容器):将指定容器转换为字符串

tuple(容器):将指定容器转换为元组

set(容器)：将指定容器转换为集合

#### 6.7.4容器的通用排序功能

sorted(容器,[reverse=Ture])  

将给定容器进行排序，排序后都会得到列表list对象

```python
tup=(32,33,12,45,56,17,67,86)
tup1=sorted(tup)
print(f"tup1正向排序的类型：{type(tup1)}，tup的值：{tup1}")

tup2=sorted(tup,reverse=True)
print(f"tup1反向排序的类型：{type(tup2)}，tup的值：{tup2}")

# tup1正向排序的类型：<class 'list'>，tup的值：[12, 17, 32, 33, 45, 56, 67, 86]
# tup1反向排序的类型：<class 'list'>，tup的值：[86, 67, 56, 45, 33, 32, 17, 12]
```



## 七、函数进阶

### 7.1函数多返回值

* 按照返回值的顺序，写对应顺序的多个变量接受即可
* 变量之间用逗号隔开
* 支持不同类型的return

```python
def test_return():
    return "高鑫",100

name,score=test_return()
print(name,score)

# 高鑫 100
```

### 7.2函数多种传参方式

#### 7.2.1位置参数

调用函数时根据函数定义的参数位置来传递参数

注：传递的参数和定义的参数的顺序及个数必须一致

```python
def test_return(name,score):
    print(f"我的名字是{name},期末考了{score}")

test_return("高鑫",100)

# 我的名字是高鑫,期末考了100
```

#### 7.2.2 关键字参数

函数调用时通过“键-值”形式传递参数

作用：可以让函数更加清晰、容易使用，同时也清除了参数的顺序需求

注：函数调用时，如果有位置参数时，位置参数必须在关键字参数的前面，但关键字参数之间不存在先后顺序

```python
def test_return(name,age,score):
    print(f"我的名字是{name},今年{age},期末考了{score}.")

test_return("高鑫",score=100,age=20)

# 我的名字是高鑫,今年20,期末考了100.
```

#### 7.2.3 缺省参数

也叫默认参数，用于定义函数，为参数提供默认值，调用函数时可不传该默认函数的值。

所有的位置参数必须出现在默认参数前面，包括函数定义和调用。

```python
def test_return(age,score,name="高鑫"):
    print(f"我的名字是{name},今年{age},期末考了{score}.")

test_return(score=100,age=20)

# 我的名字是高鑫,今年20,期末考了100.
```

#### 7.2.4 不定长参数

也叫做可变参数，用于不确定调用的时候会传递多少个参数（不传参也可以）的场景

1. 位置传递

   ```python
   def func(*name):
       print(type(name))
       print(name)

   func("高鑫")
   func("高鑫","鑫鑫","高高")

   # <class 'tuple'>
   # ('高鑫',)
   # <class 'tuple'>
   # ('高鑫', '鑫鑫', '高高')
   ```

   传进的所有参数都会name变量收集，它会根据传进参数的位置合并为一个元组，name为元组类型。

2. 关键字传递

   ```python
   def func(**student):
       print(type(student))
       print(student)

   func(name="高鑫",score=90,grade="Sophomore")

   # <class 'dict'>
   # {'name': '高鑫', 'score': 90, 'grade': 'Sophomore'}
   ```

   参数是”键=值“形式的情况下，所有的”键=值“都会被student接受，会根据”键=值“组成字典

### 7.3 匿名函数

#### 7.3.1函数作为参数传递

函数本身也可以作为参数传入另一个函数内

```python
def test(compute):
    result=compute(1,2)
    print(result)

def compute(x,y):
    return x+y

test(compute)

# 3
```

这是一种计算逻辑的传递，而非数据的传递，任何逻辑运算都可以自定于并作为函数传入

#### 7.3.2 lambda函数

def可以定义带有名称的函数，lamabda可以定义匿名函数

有名称的函数可以基于名称多次使用，匿名函数只能临时使用一次

==lambda 传入参数:函数体:(一行代码)==

* 传入参数表示匿名函数的形式参数，如，x，y表示接受两个形式参数
* 函数体，就是函数的执行逻辑，注：只能写一行，无法写多行代码

```python
def test(compute):
    result=compute(1,2)
    print(result)

test(lambda x,y:x+y)

# 3
```

## 八、Python文件操作

### 8.1文件编码

编码是一种规则集合，记录了内容和二进制进行相互转换的逻辑

编码有许多种，我们常用的是UTF-8编码

计算机只认识0和1，所以需要将内容翻译成0和1才能保存在计算机中，同时也需要编码，将计算机保存的0和1反向翻译回识别的内容

### 8.2文件的读取

对文件的基本操作可以分为3个基本操作：

打开文件、读写文件，关闭文件

注：可以只打开文件，不进行任何读写

#### 8.2.1 open打开函数

==open(name,mode,encoding)==

使用open函数打开一个已经存在的文件，或者创建一个新文件

* name：打开的目标文件名的字符串（可以包含文件所在的具体路径）
* mode：设置打开文件的模式：只读，只写，追加
* encoding：编码格式，推荐UTF-8

```python
f=open('python.txt','r',encoding="UTF-8")
print(type(f))

# <class '_io.TextIOWrapper'>
#encoding的顺序不是第三位，所以需要用关键字传参指定
```

此时的"f"是open函数的文件对象，对象是python的一种特殊的数据格式，拥有属性和方法，可以使用对象.属性或对象.方法对其进行访问。

#### 8.2.2 mode常用的三种基本访问模式

| 模式 | 描述                                                         |
| ---- | ------------------------------------------------------------ |
| r    | 以只读方式打开文件，文件的指针将放在文件的开头               |
| w    | 打开一个文件只用于写入，如果文件已存在则打开文件，并从头开始编辑，原有内容会被删除。如果文件不存在，创建新文件 |
| a    | 打开一个文件只用于追加，如果文件已存在则打开文件，新的内容会被写入已有内容之后。如果文件不存在，创建新文件已写入 |

#### 8.2.3 读操作相关方法

1.read() 方法：

==文件对象.read(num)==

num表示要从文件中读取的数据长度（单位是字节），如果没有传入num，那么就表示读取文件中的所有数据

2.readlines()方法

==文件对象.readlines()==

按照行的方式把整个文件中的内容一次性读取，并且返回的是一个列表，其中每一行的数据为一个元素

3.readline()方法

==文件对象.readline()==

一次读取一行数据

可以通过for循环读取文件行：

```python
for line in open("python.txt","r"):
    print(line)
    
# 每一个line临时变量，就记录了文件的一行数据
```

4.close方法

==文件对象.close()==

如果不调用close方法，同时程序没有停止运行，那么这个文件将一直被python程序占用

5.with open方法

==with open ("name","mode") as f==

在操作完成后自动关闭文件，避免遗忘掉close方法

### 8.3 文件的写入

```python
# 1.打开文件
f=open('python.txt','w')
# 2.文件写入
f.write('hello world')
# 3.内容刷新
f.flush()
```

* 直接调用write，内容并未真正写入文件，而是会积攒在程序的内存中，称之为缓冲区
* 当调用flush时，内容回真正写入文件中
* 可以避免频繁的操作硬盘，导致效率下降（攒一堆，一次性写入硬盘）

注意事项：

1. w模式，文件不存在，会创建新文件；文件存在，会清空原有内容
2. close方法，回到有flush方法的功能

### 8.4文件的追加

```python
# 1.打开文件，通过a模式打开即可
f=open('python.txt','a')
# 2.文件写入
f.write('hello world')
# 3.内容刷新
f.flush()
```

注意事项：

1. a模式，文件不存在，会创建新文件；文件存在，会在原有内容后继续写入
2. 可以使用"\n"来写出换行符



## 九、Python异常模块与包

### 9.1捕获异常

异常就是程序运行过程中出现了错误，bug就是指异常的意思

世界上没有完美的程序，任何程序在运行过程中，都有可能出现异常，我们可以在力所能及的范围内，对可能出现的bug进行提前准备和处理

这称之为：异常处理（捕获异常）

捕获异常的作用在于：提前假设某处会出现异常，做好提前准备，当真的出现异常的时候，可以有后续手段。

#### 9.1.1捕获常规异常

基本语法：

```python
try:
    可能出现错误的代码
except:
    异常处理
```

快速入门:

```python
# 尝试以r模式打开文件，如果文件不存在，则以w方式打开
try:
    f=open("linux.txt",'r')
except:
    f=open("linux.txt",'w')
```

#### 9.1.2 捕获指定异常

```python
try:
    print(name)
except NameError as e:
    print("name未定义")
```

注：

* 如果尝试执行的代码的异常类型和要捕获的异常类型不一致，则无法捕获异常
* 一般try下方只放一行尝试执行的代码

#### 9.1.3 捕获多个异常

当捕获多个异常时，可以把捕获的异常类型的名字，放到except后，并使用元组的方式进行书写

```python
try:
    print(1/0)
except (NameError,ZeroDivisionError):
    print("ZeroDivisionError错误……")
    
```

#### 9.1.4捕获所有异常并输出描述信息

```python
try:
    print(1/0)
except Exception as e:
    print(e)
    
# division by zero
```

#### 9.1.5 异常else和finally

else表示的是如果没有异常要执行的代码；finally表示的是最后无论如何都要执行的代码

```python
try:
    print("高鑫")
except Exception as e:
    print(e)
else:
    print("高鑫没有异常")
finally:
    print("我是必须执行的代码")
    
# 高鑫
# 高鑫没有异常
# 我是必须执行的代码

```

### 9.2 异常的传递

异常是具有传递性的

当fun1中发生异常的，并且没有捕获异常的时候，异常会传递给fun2，当fun2也没有捕获这个异常的时候，main函数会捕获这个异常，这就是异常的传递性。

```python
def fun1():
    print("fun1 start")
    print(name)
    print("fun1 end")

def fun2():
    print("fun2 start")
    fun1()
    print("fun2 end")

def main():
    try:
        fun2()
    except Exception as e:
        print(e)

main()

# fun2 start
# fun1 start
# name 'name' is not defined
```

注：当所有函数都没有捕获异常时，程序会报错

### 9.3 Python模块

#### 9.3.1 模块的导入

模块是一个python文件，以.py结尾，模块能定义函数、类和变量，模块里也能包含可执行的代码

模块的作用:  python中有很多各种不同的模块, 每一个模块都可以帮助我们快速的实现一些功能, 比如实现和时间相关的功能就可以使用time模块，我们可以认为一个模块就是一个工具包, 每一个工具包中都有各种不同的工具供我们使用进而实现各种不同的功能

模块导入方式：`[from 模块名]import [模块|类|变量|函数|*][as 别名]`

常用的组合形式

1.impot 模块名

```python
import time

print("start")
time.sleep(1)
print("end")

# start
# end
```

2.from 模块名 import 功能名

```python
from time import sleep

print("start")
sleep(1)
print("end")
```

3.from 模块名 import *

导入模块中的所有方法

```python
from time import *

print("start")
sleep(1)
print("end")
```

4.import 模块名 as 定义别名 和 from 模块名 import  功能 as 定义别名 

```python
from time import sleep as sl

print("start")
sl(1)
print("end")
```

定义别名后就不能用它原来的名字

注：

1. from和as 可以省略，直接import即可
2. 可以通过"."来确定层级关系
3. 模块的导入一般写在代码文件的开头位置

#### 9.3.2 自定义模块

如何自定义模块并导入：在Python代码文件中正常写代码即可，通过import、from关键字和导入python内置模块一样使用即可。

每一个python文件都可以作为一个模块，模块的名字就是文件的名字，也就是说自定义模块名必须符合标识符命名规则。

`if __name__=='__main__`表示只有当程序是直接执行的才会进入if内部，如果是被导入的，则无法执行if下的语句

`__all__=['testA']`表示使用[from xxx] import* 时只导入testA功能

注：不同模块，同名的功能，如果都被导入，那么后导入的会覆盖先导入的

### 9.4 python包

#### 9.4.1 自定义包

包就是一个文件夹，里面存放许多python的模块，通过包，在逻辑上将一批模块归为一类，方面使用

`__init__.py`文件的作用:创建包会默认自动创建的文件，通过这个文件来表示一个文件夹是Python的包，而非普通的文件夹

导入包方式一：

> import 包名.模块名

> 包名.模块名.目标

导入包方式二：

> from 包名 import 模块名

> 模块名.目标

必须在`__init__.py`文件中添加`__all__=[]`，控制允许导入的模块列表

注： `__all__`针对的是 ’ from ... import * ‘ 这种方式对 ‘ import xxx ’ 这种方式无效

#### 9.4.2 安装第三方包

在Python程序的生态中，有许多非常多的第三方包（非Python官方），可以极大的帮助我们提高开发效率，如：

* 科学计算中常用的：numpy包
* 数据分析中常用的：pandas包
* 大数据计算中常用的：pyspark、apache-flink包
* 图形可视化常用的：matplotlib、pyecharts
* 人工智能常用的：tensorflow

安装第三方包：

在命令提示符程序中输入：`pip install 包名称`获取第三方包

pip网络优化（镜像下载）：

`pip install -i https://pypi.tuna.tsinghua.edu.cn/simple 包名称`