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

auto的引用：

* 使用auto定义迭代器



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

