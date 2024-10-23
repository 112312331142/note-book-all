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
//
// Created by Administrator on 2024/10/20.
//
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










#### 1 新基础类型

整数类型： long long 

```cpp
int main(){
    long long x=52525LL;
    unsigned long long y=4141LL;

    cout<<numeric_limits<long long>::max() <<" "<<numeric_limits<long long>::min()<<endl;
}
```

```cpp
    char16_t c16='s';
    char32_t c32= u's';
```

#### 2 内联和嵌套命名空间

```C++
#include "iostream"
namespace Parent{
    namespace Child1{
        void fn() {
            std::cout << "1 ";
        }
    }
    inline namespace Child2{
        void fn(){
            std::cout<<"2 ";
        }
    }
}

int main(){
    Parent::Child1::fn();
    Parent::Child2::fn();
}
```

