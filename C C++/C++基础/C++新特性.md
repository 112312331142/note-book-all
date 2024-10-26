### 1.auto 关键字推导

auto的作用：使用了auto关键字以后，编译器会在编译期间自动推导出变量的类型，这样我们就不用手动指明变量的数据类型

```cpp
auto a=10,b=11.3;// error: inconsistent deduction for 'auto': 'int' and then 'double'
```

推导的时候不能有二义性

auto的限制：

1. 使用auto必须初始化
2. auto不能在函数的参数中使用：因为我们在定义函数的时候只是对参数进行了声明，指明了参数的类型，但并没有给它赋值，只有实际调用函数的时候才会给参数赋值，而auto要求必须对变量进行初始化，所以这是矛盾的
3. auto关键字不能定义数组

auto的应用：

* 使用auto定义迭代器




### 2.decltype推导

auto使用的前提是：必须要对auto声明的类型进行初始化，否则编译器无法推导出auto的实际类型。但有时候可能需要根据表达式运行完成之后，对结果的类型进行推导，因为编译期间，代码不会运行，此时auto也就无能为力

decltype是根据摆到是的实际类型推演出定义变量时所用的类型

* 推演表达式类型作为变量的定义类型

  ```cpp
  int main() {
      //decltype
      short a = 100;
      short b = 100;
      int d = 100;
      int e = 100;
      decltype(a + b) c;
      decltype(c + d) f;
      std::cout << "c:" << typeid(c).name() << std::endl;//int
      std::cout << "f:" << typeid(f).name() << std::endl;//int

      const int &r = b;
      decltype(r) g = 0;//引用类型和const声明的类型必须初始化，使用decltype：(const and volatile)不会被继承
      std::cout << "g:" << typeid(g).name() << std::endl;//int
      const int con=12;
      decltype(con) h=0;
      std::cout << "h:" << typeid(h).name() << std::endl;//int
  }
  ```

* 推演函数返回值的类型

  ```cpp
  void *getMalloc(int size) {
      return malloc(size);
  }

  int main() {
      std::cout << typeid(decltype(getMalloc)).name() << std::endl;//function pointer void int

      std::cout << typeid(decltype(getMalloc(8))).name() << std::endl;//pointer void(void *)

      decltype(getMalloc(0)) ret = getMalloc(100);

  }
  ```

### 3.基于范围的for循环

```cpp
for (declaration : expression){
    //循环体
}
//declaration：表示此处要定义一个变量，该变量的类型为要遍历序列中存储元素的类型。C++11标准中，declaration参数处定义的变量类型可以用auto关键字表示，该关键字可以试编译器自动推导
//expression：表示要遍历的序列
```

在使用新语法格式的for循环遍历某个序列时，如果需要遍历的同时修改序列中元素的值，实现方案是在declaration参数处定义引用类型的变量

```cpp
std::vector<int> v{1, 2, 3, 4, 5};
for (auto &item: v) {
    item++;
}
for (auto &item: v) {
    std::cout << item << " ";
}
```

### 4.列表初始化

在C++11中，初始化列表的适用性大大增加了，它现在可以用于任何类型对象的初始化

类成员变量里面有const类型的，在构造函数中必须使用列表初始化进行初始化

C++11标准中提供了使用{}对标准容器进行初始化

### 5.使用using起别名

* 在C语言和C++中可以使用typedef重定义一个类型：

  `typedef unsigned int uint_t`

```c++
#include "iostream"
#include "string"
#include "map"

using db = double;  //c++11

typedef void(*function)(int, int);//c99，函数指针类型定义

using function = void (*)(int, int);//c++11，函数指针类型定义

//重定义std::map
typedef std::map<std::string, int> map_int_t;
using map_int_t = std::map<std::string, int>;

function function1(int a, int b) {
    std::cout << a << " " << b << std::endl;
    return nullptr;
}

int main() {
    function1(21, 31);
}
```

### 6. final关键字

final关键字修饰的类不能被其他类继承

```cpp
class A final{
public:
    A(){}
};
class B:public A{
    
};
```

final用来修饰虚函数，则表示该虚函数不可被子类重写

### 7.右值引用

#### 7.1 左值和右值

之所以要进行右值引用，是因为右值往往是没有名称的，在实际开发中我们可能需要对右值进行修改，因此要使用它只能借助引用的方式

选定`&&`表示右值引用

1. 左值（lvalue）： 左值是指一个具有持久状态的对象，可以出现在赋值表达式的左侧。

  特点:

  * 具有名称（可以被引用）。
  * 具有持久性，即在表达式求值后仍然存在。
  * 可以取地址（使用 & 运算符）。

2. 右值（rvalue）： 右值是指一个临时对象，通常出现在赋值表达式的右侧。
   特点:

   * 不具有名称（不能被直接引用）。
   * 通常是临时的，表达式求值后即消失。
   * 不能取地址（使用 & 运算符会导致编译错误）。

#### 7.2 左值引用和右值引用

1. 左值引用（lvalue reference）： 左值引用必须绑定到一个左值。

   语法: T&

   特点: 绑定到一个具有持久状态的对象，该对象可以取地址。

2. 右值引用：之所以要进行右值引用，是因为右值引用往往是没有名称的，在实际开发中我们可能需要对右值进行修改

   `类型&& 引用变量名字 = 右值`

#### 7.3 右值引用的实际用途

* 移动语义：如果一个类中涉及到资源管理（比如指针），用户必须显示提供拷贝构造、赋值运算符重载以及析构函数，否则编译器将会自动生成一个默认的，如果遇到拷贝对象或者对象之间相互赋值，就会出错  

  将临时对象的资源转移到s2中

* 我们用对象a初始化对象b，然后对象a我们就不再使用，但是对象a的空间还在，既然拷贝拷贝构造函数，实际上就是把a对象的内容复制一份到b中，那么为什么我们不能直接使用a的空间。这样就避免乐新的空间的分配，大大降低了构造的成本。这就是移动构造函数的初衷

* 拷贝的构造函数中，对于指针，我们一定要采用深层复制，而移动构造函数中，对于指针，我么采用浅层复制，浅层复制之所以危险，是因为两个指针共同指向同一片内存区域，若第一个指针将其释放，另一个指针的指向就不合法了。所以我们只要避免第一个指针释放空间就可以了，避免的方法就是将第一个指针（比如a->value）置为NULL，这样在调用析构函数的时候，由于判断是否为NULL的语句，所以析构a的时候并不会回收a->value指向的空间

* 移动构造函数的参数和拷贝构造函数不同，拷贝构造函数的参数是一个左值引用，但是移动构造函数的初值是一个右值引用。意味着，移动构造函数的参数是一个右值或者将亡值的引用。也就是说，只用用一个右值，或者将亡值初始化成另一个对象的时候，才会调用移动构造函数，而那个move语句，就是将一个左值变成一个将亡值

move函数：

唯一的功能就是：将一个左值强制转换为右值引用，通过右值引用使用该值，实现移动语义

注意：被转换的左值，其声明周期并没有随着左右值的转化而改变，即std::move转化的左值变量lvalue不会被销毁

为了保证移动语义的传递，程序员在编写移动构造函数时，最好使用std::move转移拥有资源的成员为右值

### 8.智能指针

在C++11中通过引入智能指针的概念，使得C++程序员不需要手动释放内存

#### 8.1 概述

智能指针是原始指针的封装，其优点是自动分配内存

并不是所有的指针都可以封装成智能指针，很多时候原始值真能要更方便

各种指针中最常用的是裸指针，其次是unique_ptr和shared_ptr,weak_ptr是shared_ptr的一个补充

指针指针只能解决一部分问题，即独占/共享所有权指针的释放、传输

智能指针没有从根本上解决C++内存安全问题，不加以注意依然会造成内存安全问题

#### 8.2 unique_ptr共享指针

##### 8.2.1 基本使用

* 在任何给定的时刻，只能有一个指针管理内存
* 当指针超出作用域时，内存将自动释放
* 该类型指针不可Copy，只可以Move

三种创建方式：

* 通过已有裸指针创建
* 通过new创建
* 通过`std::make_unique`创建

unique_ptr可以通过get()获取地址

unique_ptr实现了`->`与`*`

* 可以通过->调用成员函数
* 可以通过*调用解引用

```cpp
#include "iostream"
#include "memory"
#include "cat.h"

int main(int argc, char *argv[]) {
    std::cout << argc << " " << argv << std::endl;
//    Cat c1("OK");
//    c1.cat_info();
//    {
//        Cat c1{"OK"};
//        c1.cat_info();
//    }

    //row pointer 不会调用析构函数
/*    Cat *c_p1 = new Cat("yy");
    int *i_p1 = new int(100);
    c_p1->cat_info();
    {//局部作用域
        Cat *c_p1 = new Cat("yy_scope");
        i_p1 = new int(200);
        c_p1->cat_info();
        delete c_p1;
    }
    delete c_p1;
    std::cout << *i_p1 << std::endl;//200*/

    // 三种创建方式
    //第一种
    Cat *c_p2 = new Cat("ys");
    std::unique_ptr<Cat> u_c_p2(c_p2);
//    c_p2->cat_info();
//    u_c_p2->cat_info();
//    c_p2->set_cat_name("ooooo");
//    u_c_p2->cat_info();
    c_p2 = nullptr;
    delete c_p2;
    u_c_p2->cat_info();

    //第二种
    std::unique_ptr<Cat> u_c_p3(new Cat("dd"));
    std::unique_ptr<int> u_i_p3(new int(1000));
    u_c_p3->cat_info();
    u_c_p3->set_cat_name("xin");
    u_c_p3->cat_info();
    std::cout << *u_i_p3 << std::endl;
    std::cout << "int address:" << u_i_p3.get() << std::endl;
    std::cout << "cat address:" << u_c_p3.get() << std::endl;


    //第三种 std::make_unique
    std::unique_ptr<Cat> u_c_p4 = std::make_unique<Cat>();
    std::unique_ptr<int> u_i_p4 = std::make_unique<int>(2000);
    u_c_p3->cat_info();
    u_c_p3->set_cat_name("oo00");
    u_c_p3->cat_info();
    std::cout << *u_i_p4 << std::endl;
    std::cout << "int address:" << u_i_p4.get() << std::endl;
    std::cout << "cat address:" << u_c_p4.get() << std::endl;

    //get和常量类型

    std::cout << "-----------end-------------" << std::endl;
    return 0;
}
```

##### 8.2.2 函数调用

* unique_ptr不可copy，只可以move
* Passing by value
  1. 需要用std::move来转移内存拥有权
  2. 如果参数直接传入std::make_unique语句，自动转移为move
* Passing by reference
  1. 如果设置参数为const则不能改变指向
  2. reset()方法为只能指针清空方法
* Return by value
  1. 指向一个local object
  2. 可以用作链式函数


#### 8.3 shared_ptr独占指针

* shared_ptr技术指针又称共享指针，与unique_ptr不同的是它是可以共享数据的
* shared_ptr创建了一个计数器与类对象所指的内存相关联
* copy则计数器加一，销毁则计数器减一，api为use_count()



