title: Const关键字的使用
date: 2018-07-09 14:24:30
tags:
---
## const关键字的使用

阅读 Redis 源码时遇到很多地方使用 const 关键字，只知道这个修饰符用来定义常量，不明白详细用法，遂做总结。

### const的作用

大多数语言中 const都是一个关键字，用来定义常量，当一个变量被const修饰时，它的值就不能再被改变。

在 C 语言中，#define（预编译命令）也有这个作用。相比于 #define，const最大的用处是：可以保护被修饰的变量，防止意外修改，增强程序的健壮性。

### const的用法

#### 修饰局部变量

```c
const int n=5;
int const m=6;
const char* str="hello";
```

const修饰变量时，要给变量进行初始化，否则之后不能进行赋值。

经const修饰的变量，一旦被意外修改，在编译阶段就会被检查出来，避免了程序运行时异常终止的状况。

#### 修饰指针

常量指针：

```c
#include<stdio.h>

int main()
{
		int a=5;
		int b=6;
		const int* n=&a;
		*n = 8; //error:read-only variable is not assignable
        a = 7;  //*n=7
		n=&b;  //*n=6
		return *n;
}
```

常量指针指向的值不能改变，但指针可以指向其他地址，也可以用其他引用来改变变量的值。

指针常量：

```c
#include<stdio.h>

int main()
{
		int a=5;
		int b=6;
		const int* n=&a;
		*n = 8; //*n=8
        a = 7;  //*n=7
        int *p=&a;
        *p = 9; //*n=9
		n=&b;  //error
		return *n;
}
```

指针常量指向的地址不能改变，地址中保存的数值可以改变，也可以通过其他指向该地址的指针修改。

#### 修饰函数参数

函数参数中注意是指针常量还是常量指针，也有情况是两者结合，则指针和指向的值都不可改变，但很少见。

#### 修饰全局变量

尽量少用全局变量，如果用，尽量多用const修饰。

