# C++提高编程

## vs快捷键

```
ctrl f5：启动项目

ctrl k，
	ctrl+d：格式化代码
	ctrl+u：取消注释
	
ctrl alt l：显示Solution Explorer
ctrl shift a：添加新类

ctrl shift b：快速生成项目
ctrl shift l：删除当前行
Ctrl Shift Enter：在当前行下方插入空行
```

C++：向函数传递对象(对象、对象指针、对象引用)

1. 使用对象作为函数参数，形参和实参是不同的对象，它们所占地址空间不同，因此，形参的改变，并不影响实参的值。
2. 使用对象指针作为函数参数中，所谓"传址调用",就是在函数调用时使实参对象和形参对象的地址传递给函数，形参和实参都指向同一个地址值，此时在函数中对形参对象的修改将影响调用该函数的实参对象本身。
3. 使用对象引用作为函数的参数，所谓"对象引用"，就是对实参对象换了个别名，实际上它们仍是同一个对象，所以，所谓的形参(别名对象)值的的改变，直接就是实参对象值的改变。

## 1 模板

### 1.1 模板的概念

模板的特点：

* 模板不可以直接使用，他只是一个框架
* 模板的通用并不是万能的

### 1.2 函数模板

* C++另一种编程思想称为泛型编程，主要利用的技术就是模板
* C++提供两种模板机制：函数模板和类模板

#### 1.2.1 函数模板语法

```C++
template<typename T>
函数声明或定义

//template --声明创建模板
//typename --表明其后面的符号是一种数据类型，可以用class代替
//T --T是一个通用的数据类型
```

代码实例：

```C++
template<typename T>
void mySwap(T &a, T &b) {
	T temp = a;
	a = b;
	b = temp;
}

int main() {
	int a = 10;
	int b = 20;

	double c = 1.4;
	double d = 31.1;

	//使用模板方式交换，两种方式使用函数模板
	//1.自动类型推导
	mySwap(a, b);
	//2.显示指定类型
	mySwap<double>(c, d);

	cout << "a=" << a << endl;//a=20
	cout << "b=" << b << endl;//b=10

	cout << "c=" << c << endl;//c=31.1
	cout << "d=" << d << endl;//d=1.4
}
```

注意：

* 自动类型推导，必须推导出数据类型T，才可以使用
* 模板必须要确定出T的数据类型，才可以使用

#### 1.2.2 函数模板案例

实现通用对数组进行排序的函数，规则：从大到小，算法：选择

```c++
template<typename T>
void mySort(T arr[],int len) {
	
	for (int i = 0; i < len; i++) {
		int max = i;//认定的最大值下标
		for (int j = i + 1; j < len; j++) {
			//认定的最大值比遍历的数值要小，说明j下标的元素才是真正的最大值
			if (arr[max] < arr[j]) {
				max = j;
			}
		}
		if (max != i) {
			//交换max和i元素
			swap(arr[max], arr[i]);
		}
	}
}

//提供打印数组的模板
template<class T>
void printArray(T arr[], int len) {
	for (int i = 0; i < len; i++) {
		cout << arr[i] << " ";
	}
	cout << endl;
}

int main() {
	int arrayInt[] = { 43,435,7,713,24,5 };
	char arrayChar[] = "1m3bifa";
	
	int lenInt = sizeof(arrayInt) / sizeof(int);
	int lenChar = sizeof(arrayChar) / sizeof(char);
	mySort(arrayInt,lenInt);
	mySort(arrayChar,lenChar);

	printArray(arrayInt, lenInt);//713 435 43 24 7 5
	printArray(arrayChar, lenChar);//m i f b a 3 1
}
```

#### 1.2.3 普通函数与函数模板

 普通函数与函数模板区别：

* 普通函数调用可以发生隐式类型转换
* 函数模板用自动类型推导，不可以发生隐式类型转换
* 普通函数用显示指定类型，可以发生隐式类型转换

 普通函数与函数模板调用规则：

* 如果函数模板和普通函数都可以实现，优先调用普通函数
* 可以通过空模板参数列表来强制调用函数模板
* 函数模板也可以发生重载
* 如果函数模板可以产生更好的匹配，优先调用函数模板

#### 1.2.4 模板的局限性

模板并不是万能的，有些特定数据类型，需要具体化方式做特殊实现

提供模板的重载，可以为这些特定的类型提供具体化的模板

```C++
//利用具体化的person的版本实现代码，具体化优先调用
template<> bool myCompare(Person& p1, Person& p2) {
	if (p1.name == p2.name && p1.age == p2.age) {
		return true;
	}
	else
	{
		return false;
	}
}
```

* 利用具体化模板，可以解决自定义类型的通用化
* 学习模板并不是为了写模板，而是在STL能够运用系统提供的模板

### 1.3 类模板

#### 1.3.1 类模板语法

类模板和函数模板语法类似，在声明模板template后面加类，此类称为类模板

类模板和函数模板区别主要有两点：

* 类模板没有自动类型推导的使用方式
* 类模板在模板参数列表中可以有默认参数

#### 1.3.2 类模板成员函数创建时机

* 普通类中的成员函数一开始就可以创建
* 类模板中的成员函数在调用时才创建

```C++
#include<iostream>
using namespace std;

class Person1 {
public:
	void showPerson1() {
		cout << "person1 show" << endl;
	}
};

class Person2 {
public:
	void showPerson1() {
		cout << "person2 show" << endl;
	}
};

template<class T>
class MyClass {
public:
	T obj;

	//类模板中的成员函数
	void fun1() {
		obj.showPerson1();
	}

	void fun2() {
		obj.showPerson2();
	}
};

void test01() {
	MyClass<Person1> m;
	m.fun1();
	//m.fun2();//编译会出错
}

int main() {
	test01();
}
```

#### 1.3.3 类模板对象做函数参数

三种传入方式：

* 指定传入的类型 -- 直接显示对象的数据类型
* 参数模型化        -- 将对象中的参数变为模板进行传递
* 整个类模板化    -- 将这个对象类型，模板进行传递

```C++
#include<iostream>
#include<string>
using namespace std;


template<class T1,class T2>
class Person {
public:
	Person(T1 name, T2 age) {
		this->m_Name = name;
		this->m_age = age;
	}

	void show() {
		cout << "姓名：" << this->m_Name << " 年龄：" << this->m_age << endl;
	}

	T1 m_Name;
	T2 m_age;
};

//-指定传入的类型 -- 直接显示对象的数据类型
void printPerson1(Person<string,int>&p) {
	p.show();
}
void test01() {
	Person<string, int>p{ "高鑫",111 };
	printPerson1(p);
}

//- 参数模型化        -- 将对象中的参数变为模板进行传递
template<class T1,class T2>
void printPerson2(Person<T1,T2>& p) {
	p.show();
	cout << typeid(T1).name() << " " << typeid(T2).name() << endl;

}
void test02() {
	Person<string, int>p{ "gaoxin",11 };
	printPerson2(p);
}
//- 整个类模板化    -- 将这个对象类型，模板进行传递
template <typename T>
void printPerson3(T& p) {
	p.show();
	cout << "T的数据类型：" << typeid(T).name() << endl;

}
void test03() {
	Person<string, int>p{ "高大王",1 };
	printPerson3(p);
}


int main() {
	//test01();
	//test02();
	test03();
}
```

#### 1.3.4 类模板与继承

当类模板碰到继承时，需要注意以下几点：

* 当子类继承的父类是一个类模板时，子类在声明的时候，需指定出父类中T的类型
* 如果不指定，编译器无法给出子类分配内存
* 如果想灵活指出父类中的T的类型，子类也需变为类模板

如果父类是类模板，子类需要指定出父类中T的数据类型 

#### 1.3.5 类模板成员函数类外实现

```C++
#include<iostream>
#include<string>
using namespace std;

template<class T1,class T2>
class Person {
public:
	Person(T1 name, T2 age);
	void showPerson();

	T1 name;
	T2 age;
};

//构造函数的类外实现
template<class T1, class T2>
Person<T1,T2>::Person(T1 name, T2 age) {
	this->name = name;
	this->age = age;
}

//成员函数的类外实现
template<class T1, class T2>
void Person<T1,T2>::showPerson() {
	cout << "姓名：" << this->name << endl;
	cout << "年龄：" << this->age << endl;
}

int main() {
	Person<string, int> person("叶凡", 1100000);
	person.showPerson();
}
```

类模板中的成员函数类外实现时，需要加上模板参数列表

#### 1.3.6. 类模板分文件编写

问题：类模板中成员函数创建时机是在调用阶段，导致分文件编写时链接不到

解决：

* 直接包含.cpp源文件
* 将声明和实现写在同一个文件中，并将后缀名改为.hpp，hpp是约定的名称，并不是强制（主流方法）

#### 1.3.7 类模板与友元

全局函数类内实现 - 直接在类内声明友元即可

全局函数类外实现 - 需要提前让编译器知道全局函数的存在

```C++
#include<iostream>
#include<string>
using namespace std;

//提前让编译器知道Person类存在
template<class T1, class T2>
class Person;

//类外实现
template<class T1, class T2>
void printPerson2(Person<T1, T2> p) {
	cout << "类外实现	姓名：" << p.m_Name << " 年龄：" << p.m_Age << endl;
}

template<class T1, class T2>
class Person {
	//全局函数 类内实现
	friend void printPerson(Person<T1, T2> p) {
		cout << "类内实现	姓名：" << p.m_Name << " 年龄：" << p.m_Age << endl;
	}

	//全局函数类外实现
	friend void printPerson2<>(Person<T1, T2> p);

public:
	Person(T1 name, T2 age) {
		this->m_Name = name;
		this->m_Age = age;
	}

	void show() {
		cout << "姓名：" << this->m_Name << " 年龄：" << this->m_Age << endl;
	}
private:
	T1 m_Name;
	T2 m_Age;
};

void test01() {
	Person<string, int> p("tom", 20);
	printPerson(p);
}

void test02() {
	Person<string, int> p("jerry", 20);
	printPerson2(p);
}

int main() {
	test01();
	test02();
}
```

注：建议全局函数做类内实现，用法简单，而且编译器可以直接识别

## 2 STL常用容器

### 2.1 STL简介

STL（标准模板库），从广义上分为容器、算法、迭代器，容器和算法之间通过迭代器进行无缝连接

STL几乎所有的代码都采用了模板库或者模板函数

STL六大组件：

* 容器：各种数据结构，如vector、list、deque、set、map用来存放数据
* 算法：各种常见的算法，如sort、find、copy、for_each等
* 迭代器：扮演了容器与算法之间的胶合剂
* 仿函数：行为类似函数，可用作算法的某种策略
* 适配器：一种用来修饰容器或者仿函数或迭代器接口的函数
* 空间配置器：负责空间的配置与管理

容器总共分为两大类，一类是序列式容器，包括vector(连续存储),deque(双向队列),list(双向链表)，stack(栈)，queue(队列)，另一类是关联式容器，包括set,multiset,map,multimap.

算法分为两类，质变算法是指运算过程中会更改区间内的元素的内容，例如拷贝、替换、删除等，非质变算法是指运算过程中不会更改区间的元素内容，例如查找、计数、遍历、寻找极值等

迭代器提供一种方法，使之能够寻访某个容器所含的各个元素，而又无需暴露该容器的内部表示方式，每个容器都有专属的迭代器

### 2.2 string容器

string与char* 的区别：

* `char*`是一个指针
* string是一个类，列内部封装到`char*`，管理这个字符串，是一个`char*`型的容器

```C++
#include<iostream>
#include<string>
using namespace std;

//string构造函数
void test1() {
	const char* str = "hello world";

	string s1;//默认构造
	string s2(str);
	string s3(s2);
	string s4(10, 'g');

	cout << "s2= " << s2 << endl;
	cout << "s3= " << s3 << endl;
	cout << "s4= " << s4 << endl;
}

//string赋值操作
void test2() {
	string str1 = "hello";//char*字符串直接赋值
	string str2 = str1;//拷贝赋值
	string str3;
	str3 = 'a';//字符赋值
	string str4;
	str4.assign("hello C++");//将括号里面的字符串赋值给str4
	string str5;
	str5.assign("java python C/C++", 5);
	string str6;
	str6.assign(str5);
	string str7;
	str7.assign(7, 'a');

	cout << "str1=" << str1 << endl;
	cout << "str2=" << str2 << endl;
	cout << "str3=" << str3 << endl;
	cout << "str4=" << str4 << endl;
	cout << "str5=" << str5 << endl;
	cout << "str6=" << str6 << endl;
	cout << "str7=" << str7 << endl;
}

//字符串拼接
void test3() {
	string str1 = "我";
	str1 += "是高鑫";
	cout << "str1=" << str1 << endl;

	string str2 = "I";
	str2.append(" love ");
	cout << "str2=" << str2 << endl;
	str2.append("game abdfa", 4);
	cout << "str2=" << str2 << endl;
	str2.append(str1);
	cout << "str2=" << str2 << endl;

	str2.append(str1, 4, 4);//一个中文占两个位置
	cout << "str2=" << str2 << endl;
}

//字符串查找和替代
void test4() {
	//1.查找
	string str1 = "abcdefg";
	//find  :从左往左找
	cout<<"de在str1中的位置：" << str1.find("de") << endl;
	cout<<"df在str1中的位置：" << int(str1.find("df")) << endl;//没有返回-1(用int接受时)
	//rfind	:从右往左查找
	str1.append(str1);
	cout << "de在str1中的位置：" << str1.rfind("de") << endl;
	cout << "df在str1中的位置：" << int(str1.rfind("df")) << endl;//没有返回-1(用int接受时)

	//2.替换
	string str2 = str1;
	str2.replace(1, 3, "214314");//从一号位置其起，3个字符被替换
	cout << "str2= " << str2 << endl;
}

//字符串比较
void test5() {
	//按照ASCⅡ码进行比较
	string str1 = "gaoxin";
	string str2 = "hanli";
	if (str1 > str2) {
		cout << str1 << "比" << str2 << "大" << endl;
	}
	else {
		cout << str1 << "比" << str2 << "小" << endl;
	}
	cout << str1.compare(str1) << endl;//相等时返回0
	cout << str1.compare(str2) << endl;//小于时返回-1
	cout << str2.compare(str1) << endl;//大于时返回1
}

//字符存取
void test6() {
	string str = "hello";
	cout << "str=" << str << endl;
	//1.通过中括号访问单个字符
	for (int i = 0; i < str.size(); i++) {
		cout << str[i] << " ";
	}
	cout << endl;
	//2.通过at方法访问单个字符
	for (int i = 0; i < str.size(); i++) {
		cout << str.at(i) << " ";
	}
	cout << endl;
	//3.增强for
	for (char c : str) {
		cout << c << " ";
	}
	cout << endl;
	
	//修改单个字符
	str[0] = 'x';
	cout << "str=" << str << endl;
}

//字符串插入和删除
void test7() {
	string str = "C/C++ go";
	//插入
	str.insert(5, " java");
	cout << "str = " << str << endl;
	//删除
	str.erase(11);
	cout << "str = " << str;
}

//字串获取
void test8() {
	string str = "yefan";
	string subStr = str.substr(1, 3);//从1位置起，截三个
	cout << "substr = " << subStr << endl;
}

int main() {
	test1();
	cout << "========================" << endl;
	test2();
	cout << "========================" << endl;
	test3();
	cout << "========================" << endl;
	test4();
	cout << "========================" << endl;
	test5();
	cout << "========================" << endl;
	test6();
	cout << "========================" << endl;
	test7();
	cout << "========================" << endl;
	test8();
}
```

### 2.3 vector容器

vector与数组类似，称为单端数组，区别：

数组是静态空间，而vector可以动态扩展

动态扩展：并不是在原空间之后续借新空间，而是找更大的内存空间，然后将原数组拷贝新空间，释放原空间

vector的迭代器是支持随机访问的迭代器

```C++
#include<iostream>
#include<vector>
using namespace std;

//迭代器遍历
void printVector(vector<int>& v) {
	for (vector<int>::iterator it = v.begin(); it != v.end(); it++) {
		cout << *it << " ";
	}
	cout << endl;
}

//vector容器构造
void test1() {
	vector<int>v1;//默认构造，无参构造
	for (int i = 0; i < 10; i++) {
		v1.push_back(int(rand() % 100));
	}
	printVector(v1);

	//通过区间构造
	vector<int> v2(v1.begin(), v1.end() - v1.size() / 2);
	printVector(v2);

	//n个elem构造
	vector<int> v3(10, int(rand() % 100));
	printVector(v3);

	//拷贝构造
	vector<int> v4(v3);
	printVector(v4);
}

//vector赋值
void test2() {
	vector<int>v1;//默认构造，无参构造
	for (int i = 0; i < 10; i++) {
		v1.push_back(int(rand() % 100));
	}
	printVector(v1);
	//operator =
	vector<int> v2 = v1;
	printVector(v2);
	//assign
	vector<int> v3;
	v3.assign(v1.begin(), v1.end());
	printVector(v3);
	//assign n elem
	vector<int> v4;
	v4.assign(10, 100);
	printVector(v4);
}

//vector容量和大小操作
void test3() {
	vector<int> v1;
	for (int i = 0; i < 10; i++) {
		v1.push_back(i);
	}
	printVector(v1);

	if (v1.empty()) {
		cout << "v1为空" << endl;
	}
	else {
		cout << "v1不为空" << endl;
		cout << "大小:" << v1.size() << " 容量：" << v1.capacity() << endl;
	}

	//重新指定大小
	v1.resize(15,100);
	printVector(v1);//如果重新指定的过长，默认0填充,第二个参数指定默认的填充值
}

//vector的插入和删除
void test4() {
	vector<int> v1;
	//尾插法
	v1.push_back(10);
	v1.push_back(20);
	v1.push_back(30);
	v1.push_back(40);

	printVector(v1);

	v1.pop_back();
	printVector(v1);

	v1.insert(v1.begin()+2, 1000);//第一个参数必须是迭代器
	v1.insert(v1.begin(), 100);
	printVector(v1);

	v1.insert(v1.begin(), 3, 10000);
	printVector(v1);

	v1.erase(v1.begin() + 1, v1.end() - 2);//删除两个迭代器之间的数
	printVector(v1);

	v1.clear();
	printVector(v1);
}

//vector数据存取
void test5() {
	vector<int> v1;
	for (int i = 0; i < 10; i++) {
		v1.push_back(i);
	}
	int length = sizeof(v1) / sizeof(v1[1]);
	cout << length << endl;
	for (int i = 0; i < length; i++) {
		cout << v1.at(i) << " ";
	}
	cout << endl;
	for (vector<int>::iterator it = v1.begin(); it != v1.end(); it++) {
		cout << *it << " ";
	}
	cout << endl;
	for (int i : v1) {
		cout << i << " ";
	}
	cout << endl;

	cout << v1.front() << v1.back() << endl;
}

//vector容器互换
void test6() {
	//1,基础使用
	vector<int> v1;
	for (int i = 0; i < 10; i++) {
		v1.push_back(i);
	}
	vector<int> v2;
	for (int i = 0; i < 10; i++) {
		v2.push_back(i*10);
	}
	v1.swap(v2);
	cout << "vector1:";
	printVector(v1);
	cout << "vector2:";
	printVector(v2);

	//2.实际用途，巧用swap可以收缩内存空间
	cout << "----------------------------" << endl;
	vector<int> v3;
	for (int i = 0; i < 100000; i++) {
		v3.push_back(i);
	}
	cout << "v3的容量：" << v3.capacity() << endl;
	cout << "v3的大小：" << v3.size() << endl;
	v3.resize(3);
	cout << "v3的容量：" << v3.capacity() << endl;//138225
	cout << "v3的大小：" << v3.size() << endl;//3
	vector<int>(v3).swap(v3);//匿名对象使用完就回收，可以收缩内存空间
	cout << "v3的容量：" << v3.capacity() << endl;//3
	cout << "v3的大小：" << v3.size() << endl;//3

	cout << vector<int>(3).size() << endl;;
	cout << vector<int>(3).capacity() << endl;;
}

//vector预留空间
void test7() {
	vector<int> v;

	//利用reserve预留空间
	v.reserve(100000);

	int num = 0;//统计开辟的次数
	int* p = NULL;
	for (int i = 0; i < 100000; i++) {
		v.push_back(i);
		if (p != &v[0]) {
			p = &v[0];
			num++;
		}
	}
	cout << "指针更改位置的次数:" << num << endl;//1

}


int main() {
	test1();
	cout << "=============================" << endl;
	test2();
	cout << "=============================" << endl;
	test3();
	cout << "=============================" << endl;
	test4();
	cout << "=============================" << endl;
	test5();
	cout << "=============================" << endl;
	test6();
	cout << "=============================" << endl;
	test7();
}
```

### 2.4 deque容器

双端数组，可以对头端进行插入删除操作

deque与vector的比较：

* vector对于头部的插入删除效率低，数据量较大，效率越低
* deque相对而言，对头部的插入删除速度会比vector快
* vector访问元素时的速度会比deque快，这和两者内部实现有关

deque内部工作原理：

deque内部有个中控区，维护每段缓冲区中的内容，缓冲区中存放真实数据。中控器维护的是每个缓冲区的地址，使得使用deque时像一片连续的内存空间

deque容器的迭代器也是支持随机访问

```C++
#include<iostream>
#include<deque>
#include<algorithm>
using namespace std;

void printDeque(const deque<int> &d) {//容器里的数据不能修改
	for (deque<int>::const_iterator it = d.begin(); it != d.end(); it++) {
		cout << *it << "\t";//only_read iterator
	}
	cout << endl;
}

//deque构造函数
void test1() {
	deque<int> d1;
	for (int i = 0; i < 10; i++) {
		d1.push_back(i);
	}
	printDeque(d1);

	deque<int> d2(d1.begin(),d1.end());
	printDeque(d1);

	deque<int> d3(10, 100);
	printDeque(d3);

	deque<int> d4(d3);
	printDeque(d4);

}

//deque赋值操作
void test2() {
	deque<int> d1;
	for (int i = 0; i < 10; i++) {
		d1.push_back(i);
	}
	printDeque(d1);

	//operator=赋值
	deque<int> d2 = d1;
	printDeque(d2);

	d1.assign(10,10);
	printDeque(d1);
}

//deque大小操作,deque没有容量capacity的概念
void test3() {
	deque<int> d1;
	for (int i = 0; i < 10; i++) {
		d1.push_back(i);
	}
	printDeque(d1);

	if (d1.empty()) {
		cout << "d1为空" << endl;
	}
	else {
		cout << "d1不为空" << endl;
	}

	//重新指定大小
	d1.resize(15, 100);
	printDeque(d1);
}

//deque的插入和删除,插入和删除提供的位置都是迭代器
void test4() {
	//1.两端插入操作
	deque<int> d1;
	d1.push_back(10);
	d1.push_back(10);
	d1.push_back(10);
	d1.push_front(1);
	d1.push_front(1);
	d1.push_front(1);
	printDeque(d1);
	//2.指定位置操作
	deque<int> d2;
	d2.push_back(10);
	d2.push_back(20);
	d2.push_back(100);
	d2.push_back(200);
	d2.push_back(500);
	printDeque(d2);

	d2.insert(d2.begin(),1000);//insert插入
	d2.insert(d2.end(), 2, 1000);
	printDeque(d2);

	//3.按照区间进行插入
	deque<int> d3;
	d3.push_back(1);
	d3.push_back(1);
	d3.push_back(1);
	d3.insert(d3.begin(), d2.begin(), d2.end());//将d2区间的数据填充d3的初始位置
	printDeque(d3);

	//4.删除
	deque<int> d4;
	for (int i = 0; i < 10; i++) {
		d4.push_back(i);
	}
	deque<int>::iterator it = d4.begin();
	it++;
	d4.erase(it,d4.end()-5);
	printDeque(d4);
}

//deque数据存取
void test5() {
	deque<int> d1;
	for (int i = 0; i < 10; i++) {
		d1.push_back(i);
	}
	for (int i = 0; i < d1.size(); i++) {
		cout << d1[i] << "\t";
	}
	cout << endl;
}

//deque排序操作
void test6() {
	deque<int> d1;
	for (int i = 0; i < 10; i++) {
		d1.push_back(rand()%100);
	}
	cout << "排序前：";
	printDeque(d1);
	//对于支持随机访问的迭代器的容器，都可以利用sort算法直接对其进行排序，vector容器也支持
	sort(d1.begin(),d1.end());
	cout << "排序后：";
	printDeque(d1);
	
}

int main() {
	test1();
	cout << "================================" << endl;
	test2();
	cout << "================================" << endl;
	test3();
	cout << "================================" << endl;
	test4();
	cout << "================================" << endl;
	test5();
	cout << "================================" << endl;
	test6();
}
```

### 2.5 stack容器

概念：stack是一种先进后出的数据结构，它只有一个出口

栈不允许有遍历行为

常用接口：

```C++
#include<iostream>
#include<stack>
using namespace std;

int main() {
	//符合先进后出的数据结构
	stack<int> stk;
	stk.push(10);//入栈
	stk.push(20);
	stk.push(30);
	stk.push(40);
	cout << "栈的大小为：" << stk.size() << endl;//栈的大小为：4
	//只要栈不为空，查看栈顶，并且执行出栈操作
	while (!stk.empty()) {
		//查看栈顶元素
		cout<<"栈顶元素："<<stk.top()<< " ";//栈顶元素：40 栈顶元素：30 栈顶元素：20 栈顶元素：10
		//出栈
		stk.pop();
	}
	cout << endl;
	cout << "栈的大小为：" << stk.size() << endl;//栈的大小为：0
}

```

### 2.6 queue容器

Queue是一种先进先出的数据结构，它有两个出口（队头出数据，队尾进数据）

队列中只有队头和队尾可以让外界访问

常用接口：

```C++
#include<iostream>
#include<queue>
#include<string>
using namespace std;

class Person {
public:
	string name;
	int age;

	Person(string name, int age) {
		this->name = name;
		this->age = age;
	}
};


int main() {
	Person p1("Tom", 12);
	Person p2("Jack", 12);
	Person p3("John", 12);
	Person p4("Rog", 12);
	queue<Person> q;
	q.push(p1);
	q.push(p2);
	q.push(p3);
	q.push(p4);

	//判断只要队列不为空，查看队头，查看队尾，出队
	while (!q.empty()) {
		//查看队头和队尾
		cout << "队头元素--姓名:" << q.front().name << "\t年龄:" << q.front().age << endl;
		cout << "队尾元素--姓名:" << q.back().name << "\t年龄:" << q.back().age << endl;
		//出队
		q.pop();
		//查看队列大小
		cout << "当前队列大小：" << q.size() << endl;
	}
}
```

### 2.7 list容器

#### 2.7.1 list基本概念

组成：链表有一系列节点组成；节点由一个存储数据的元素的数据域，另一个是存储下一个节点地址的指针域

功能：将数据进行链式存储

优点：可以对任意的位置进行快速的插入或删除元素

缺点：

* 对于的容器的遍历速度，没有数组快
* 占用的空间比数组大

STL中的链表是一个双向循环链表

由于链表的存储形式并不是连续的内存空间，因此链表ist中的迭代器只支持前移和后移，属于双向迭代器

List还有一个重要性质：插入和删除并不会使原有的迭代器的失效

所有不支持随机访问迭代器的容器，不可以用标准算法

#### 2.7.2 list常用接口

```C++
#include<iostream>
#include<list>
using namespace std;

void printList(const list<int> &l) {
	for (list<int>::const_iterator it = l.begin(); it != l.end(); it++) {
		cout << *it << "\t";
	}
	cout << endl;
}

//list构造函数
void test1() {
	list<int> l1;
	l1.push_back(10);
	l1.push_back(20);
	l1.push_back(30);
	l1.push_back(40);
	printList(l1);
	//区间方式构造
	list<int> l2(++l1.begin(), l1.end());
	printList(l2);
}

//list赋值和交换
void test2() {
	//1.赋值
	list<int> l1;
	l1.push_back(10);
	l1.push_back(20);
	l1.push_back(30);
	l1.push_back(40);
	printList(l1);
	list<int> l2 = l1;
	printList(l2);
	list<int> l3;
	l3.assign(l1.begin(), l1.end());
	printList(l3);
	//2.交换
	list<int> l4;
	l4.assign(10, 1000);
	l1.swap(l4);
	printList(l1);
	printList(l4);
}

//list插入和删除
void test3() {
	list<int> l1;
	l1.push_back(10);
	l1.push_back(20);
	l1.push_back(30);
	l1.push_back(40);
	//头插
	l1.push_front(100);
	l1.push_front(200);
	l1.push_front(300);

	list<int>::iterator it = l1.begin();
	l1.insert(++it, 1000);//插入
	printList(l1);

	//移除
	l1.remove(100);
	printList(l1);
}

bool MyCompare(int v1, int v2) {
	//降序，让第一个数大于第二个数
	return v1 > v2;
}

//list反转和排序
void test4() {
	list<int> l1;
	l1.push_back(rand() % 100);
	l1.push_back(rand() % 100);
	l1.push_back(rand() % 100);
	l1.push_back(rand() % 100);
	printList(l1);

	//反转
	l1.reverse();
	printList(l1);

	//排序
	l1.sort();//默认规则从小到大
	printList(l1);
	l1.sort(MyCompare);//从大到小排序,高级排序要指定规则
	printList(l1);
}

int main() {
	test1();
	cout << "==============================" << endl;
	test2();
	cout << "==============================" << endl;
	test3();
	cout << "==============================" << endl;
	test4();
}
```

### 2.8 set/multiset容器

#### 2.8.1 set基础操作

简介：所有排序都会在插入时自动被排序

本质：set/multiset属于关联式容器，底层数据结构使用二叉树实现

set不允许有重复元素出现，multiset允许

```C++
#include<iostream>
#include<set>
using namespace std;

void printSet(const set<int>& s) {
	for (set<int>::const_iterator it = s.begin(); it != s.end(); it++) {
		cout << *it << " ";
	}
	cout << endl;
}

void printSet(const multiset<int>& s) {
	for (multiset<int>::const_iterator it = s.begin(); it != s.end(); it++) {
		cout << *it << " ";
	}
	cout << endl;
}

//set容器的构造和赋值
void test1() {
	set<int> s1;

	pair<set<int>::iterator, bool> ret = s1.insert(2);//增加数据只有insert接口
	s1.insert(1);//增加数据只有insert接口
	s1.insert(3);//增加数据只有insert接口
	s1.insert(4);//增加数据只有insert接口
	s1.insert(4);//增加数据只有insert接口
	printSet(s1);//1 2 3 4 在插入时会自动排序，且不允许有重复元素

	//第一个值是该数所在位置的迭代器，第二个数显示插入失败还是成功
	cout << *ret.first <<" "<< ret.second << endl;

	set<int> s2(s1);//拷贝构造
	printSet(s2);
}

//set的大小和交换
void test2() {
	set<int> s1;

	s1.insert(2);
	s1.insert(1);
	s1.insert(3);
	s1.insert(4);
	s1.insert(4);
	cout << s1.size() << endl;
}

//set的插入和删除
void test3() {
	set<int> s1;

	s1.insert(2);
	s1.insert(1);
	s1.insert(3);
	s1.insert(4);

	s1.erase(s1.begin(),--s1.end());
	printSet(s1);//4
}

//set查找和统计
void test4() {
	set<int> s1;

	s1.insert(2);
	s1.insert(1);
	s1.insert(3);
	s1.insert(4);

	cout << *s1.find(2) << endl;//返回的是迭代器
	cout << *s1.find(4) << endl;//返回的是迭代器
	bool b=s1.find(100) == s1.end();//不存在返回的s1.end()
	cout << b << endl; //1	true

	cout << s1.count(1) << endl;//返回1的数量：1
}

//set和multiset的区别
void test5() {
	multiset<int> ms;
	ms.insert(10);
	ms.insert(20);
	ms.insert(30);
	ms.insert(50);
	ms.insert(30);

	printSet(ms);//允许重复数字
}

int main() {
	test1();
	cout << "============================" << endl;
	test2();
	cout << "============================" << endl;
	test3();
	cout << "============================" << endl;
	test4();
	cout << "============================" << endl;
	test5();
}
```

#### 2.8.2 pair对组创建

功能：成对出现的数据，利用队组可以返回两个数据

```C++
//pair队组创建
void test6() {
	//第一个方式
	pair<string, int> p1("tom", 20);
	cout << "姓名：" << p1.first << "年龄：" << p1.second << endl;
	//第二个方式
	pair<string, int> p2 = make_pair("Jerry", 30);
	cout << "姓名：" << p2.first << "年龄：" << p2.second << endl;
}
```

#### 2.8.3 set容器排序

```C++
#include<iostream>
#include<set>
#include<string>
using namespace std;

class MyComapre {//仿函数
public:
	bool operator()(int v1, int v2) const {//第一个小括号代表重载小括号，第二个代表参数列表
		return v1 > v2;
	}
};

class Person {
public:
	Person(string name, int age) {
		this->m_Name = name;
		this->m_Age = age;
	}

	string m_Name;
	int m_Age;
};

class ComaprePerson {
public:
	bool operator()(const Person &p1,const Person &p2) const {
		return p1.m_Age > p2.m_Age;
	}
};

//set容器排序
void test1() {
	set<int> s1;
	s1.insert(10);
	s1.insert(40);
	s1.insert(20);
	s1.insert(120);
	for (set<int>::iterator it = s1.begin(); it != s1.end(); it++) {
		cout << *it << " ";
	}
	cout << endl;
	//指定排序规则从大到小
	set<int, MyComapre> s2;
	s2.insert(10);
	s2.insert(40);
	s2.insert(20);
	s2.insert(120);
	for (set<int>::iterator it = s2.begin(); it != s2.end(); it++) {
		cout << *it << " ";
	}
	cout << endl;
}

//自定义类型必须指定排序规则
void test2() {
	set<Person,ComaprePerson> s;
	Person p1("刘备",41);
	Person p2("关羽",31);
	Person p3("张飞",23);
	Person p4("赵云",33);
	s.insert(p1);
	s.insert(p2);
	s.insert(p3);
	s.insert(p4);

	for (set<Person,ComaprePerson>::iterator it = s.begin(); it != s.end(); it++) {
		cout << it->m_Name << " " << it->m_Age << endl;
	}
	cout << endl;
}

int main() {
	test1();
	cout << "====================" << endl;
	test2();
}
```

### 2.9 map/multimap容器

简介：

* map中所有元素都是pair
* pair中第一元素为key（键值），起到索引作用，第二个元素为value（实值）
* 所有元素都会根据元素的键值自动排序

本质：map。multimap属于关联式容器，底层结构是用二叉树实现的

优点：可以快速按照key值找到value值

multimap允许有重复key，map不允许，value两个容器都允许有重复的key值

```C++
#include<map>
#include<iostream>
using namespace std;

void printMap(map<int, int>& m) {
	for (map<int, int>::iterator it = m.begin(); it != m.end(); it++) {
		cout << "key=" << it->first << "\tvalue=" << it->second << endl;
	}
}

//map容器构造和赋值
void test1() {
	map<int, int> m;

	m.insert(pair<int, int>(2, 90));
	m.insert(pair<int, int>(3, 120));
	m.insert(pair<int, int>(4, 110));
	m.insert(pair<int, int>(1, 110));
	printMap(m);//按照key排序
}

//map大小和交换
void test2() {
	map<int, int> m;

	m.insert(pair<int, int>(2, 90));
	m.insert(pair<int, int>(3, 120));
	m.insert(pair<int, int>(4, 110));
	m.insert(pair<int, int>(1, 110));

	cout << "大小：" << m.size() << "\t是否为空：" << m.empty() << endl;
}

//map插入和删除
void test3() {
	map<int, int> m;
	//插入
	m.insert(pair<int, int>(1, 12));
	m.insert(make_pair(2, 231));
	m.insert(map<int, int>::value_type(4, 12));
	m[3] = 421;
	m[5];//key=5,value=0
	m[3] = 1231;//可以改变键为3的值
	printMap(m);
	//删除
	m.erase(m.begin());
	m.erase(4);//按照key删除
	printMap(m);
}

//map查找和统计
void test4() {
	map<int, int> m;

	m.insert(pair<int, int>(2, 90));
	m.insert(pair<int, int>(3, 120));
	m.insert(pair<int, int>(4, 110));
	m.insert(pair<int, int>(1, 110));

	map<int,int>::iterator pos= m.find(1);
	if (pos != m.end()) {
		cout << "key" << pos->first << "\tvalue=" << pos->second << endl;
	}
	else {
		cout << "fucked fucking" << endl;
	}
}

class MyCompare {
public:
	bool operator()(int v1,int v2) const {
		return v1 > v2;
	}
};

//map排序
void test5() {
	map<int, int,MyCompare> m;

	m.insert(pair<int, int>(2, 90));
	m.insert(pair<int, int>(3, 120));
	m.insert(pair<int, int>(4, 110));
	m.insert(pair<int, int>(1, 110));
	for (map<int, int>::iterator it = m.begin(); it != m.end(); it++) {
		cout << "key=" << it->first << "\tvalue=" << it->second << endl;
	}

}

int main() {
	test1();
	cout << "====================" << endl;
	test2();
	cout << "====================" << endl;
	test3();
	cout << "====================" << endl;
	test4();
	cout << "====================" << endl;
	test5();
}
```



## 3 STL函数对象

### 3.1 函数对象

概念：重载函数调用操作符，其对象称为函数对象；函数对象使用重载的()时，行为类似函数调用，也叫仿函数

本质：函数对象（仿函数）是一个类，不是一个函数

特点：

* 函数对象在使用时，可以像普通函数那样调用，可以有参数，可以有返回值
* 函数对象超出普通函数的概念，函数对象可以有自己的状态
* 函数对象可以作为参数传递

```C++
#include<iostream>
#include<string>
using namespace std;

class MyAdd {
public:
	int operator()(int v1, int v2) {
		return v1 + v2;
	}
};

class MyPrint {
public:
	void operator()(string test) {
		cout<<test<<endl;
		this->count++;
	}

	MyPrint() {
		this->count = 0;
	}

	int count;
};

void doPrint(MyPrint& mp, string test) {
	mp(test);
}

int main() {
	//1.函数对象在使用时，可以像普通函数那样调用，可以有参数，可以有返回值
	MyAdd myAdd;
	cout << myAdd(1, 4) << endl;
	//2.函数对象超出普通函数的概念，函数对象可以有自己的状态
	MyPrint myPrint;
	myPrint("hello world");
	myPrint("hello world");
	myPrint("hello world");
	cout << "myPrint调用的次数：" << myPrint.count << endl;
	//3.函数对象可以作为参数传递
	MyPrint p;
	doPrint(p, "hello C++");
}
```

### 3.2 谓词

* 返回bool类型的仿函数称为谓词
* 如果operator接受一个参数称为一元谓词，两个参数称为两元谓词

#### 3.2.1 一元谓词

````C++
#include<iostream>
using namespace std;
#include<vector>
#include<algorithm>

class GreaeterFive {
public:
	bool operator()(int num) {
		return num > 5;
	}
};

int main() {
	vector<int> v;
	for (int i = 0; i < 10; i++) {
		v.push_back(i);
	}

	//查找容器中大于5的数字
	vector<int>::iterator it=find_if(v.begin(),v.end(),GreaeterFive());
	if (it == v.end()) {
		cout << "no" << endl;
	}
	else {
		cout << *it << endl;
	}
}
````

