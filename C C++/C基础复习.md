#### 1

指针数组和数组指针：

指针数组：本质上是是一个数组，只不过数组是用来存放指针的数组

```c++
#include "stdio.h"

int main() {
    int a = 0, b = 1, c = 2;

    //指针数组
    int *arr1[3] = {&a, &b, &c};
    int **pp = arr1;
    printf("%d", (**pp));
    printf("\n");

    //数组指针
    int arr2[3] = {a, b, c};
    int (*p)[3] = &arr2;
    printf("%d,%d,%d", *(*p + 0), *(*p + 1), *(*p + 2));
}
```

