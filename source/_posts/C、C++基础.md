---
title: C、C++基础
date: 2019-05-06 22:52:46
tags:
---

工作中的工程主要是C，不过公共的库函数设计上面存在一些问题，例如全局变量设计不合适、函数声明不合理、部分函数定义有问题，导致可移植性有点差，改成cpp文件后，报了很多错误，这里记录一些容易错误的地方，也顺带回顾一下基础知识。

---

### 全局变量的声明和使用（跨文件）

- [How do I use extern to share variables between source files?](https://stackoverflow.com/questions/1433204/how-do-i-use-extern-to-share-variables-between-source-files)
- [What is the difference between a definition and a declaration?](https://stackoverflow.com/questions/1410563/what-is-the-difference-between-a-definition-and-a-declaration)

错误的参考

- [C语言：全局变量在多个c文件中公用的方法](https://blog.csdn.net/flyingdust21/article/details/51679943)

上面的链接中Jonathan Leffler已经回答的很好了。对于声明和定义来说：

- 一个变量被声明是指告知编译器，有个某种类型的变量会被使用，但编译不会为它分配内存。
- 一个变量并定义是指编译器会分配对应变量的存储空间。

一个变量可以被多次声明，但在给定的作用域范围内只能定义一次。一个变量的定义可以是一种声明，反之不是。

最佳实践方式是使用包含`extern`关键字的头文件来声明变量，然后用某个包含该头文件的源文件对该变量进行定义。

指导原则：

- 头文件中值包含`extern`方式声明的变量，不要使用`static`或其他不合格的定义（静态全局变量只在本地文件中可以访问，因此有多个副本）
- 对于任何变量，仅在一个头文件中声明（SPOT-Single Point of Truth）
- 不要在在源文件中使用`extern`方式声明变量，源文件中应该用include头文件的方式来声明他们。——我司的实践规范，但实际代码并没有这么遵循，这样代码不好维护。
- 对于任何变量，只能有一个源文件进行定义，并且建议初始化为0。
- 头文件和源文件中的声明和定义应该保持一致。
- 不需要在函数中用`extern`来声明变量。
- 尽量避免全局变量。

例如有`file3.h`、`file1.c`和`file2.c`三个文件（头文件和源文件名称一致会更好）：

**file3.h**

```C
extern int global_variable;  /* Declaration of the variable */
```

**file1.c**

```C
#include "file3.h"  /* Declaration made available here */
#include "prog1.h"  /* Function declarations */

/* Variable defined here */
int global_variable = 37;    /* Definition checked against declaration */

int increment(void) { return global_variable++; }
```

**file2.c**

```C
#include "file3.h"
#include "prog1.h"
#include <stdio.h>

void use_it(void)
{
    printf("Global variable: %d\n", global_variable++);
}
```

---