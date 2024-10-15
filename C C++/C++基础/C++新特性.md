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

#### 3 auto占位符

auto：声明变量时根据初始化表达式自动推断该变量的类型、声明函数时函数返回值的占位符

推导时从左到右推导，不能声明非静态成员变量