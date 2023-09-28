# Linux C++ 

编译：

```shell
$ g++ -o [name] [filename] 
```

运行：

```shell
$ ./[name] #如果前面没重命名，默认[name]是a.out
```

源代码排版：

1. 递进层次应使用左缩进格式；
2. 每行代码不能过长，<=80字符；
3. 函数代码<=60line;
4. 使用空行区分不同功能的代码；
5. 符合语句书写格式要统一；
6. 除非特别有必要，否则不要在一行上书写多条语句；
7. 命名规范一

**while**循环

无限循环和哨兵

谓词函数 

ps: 布尔变量应写成谓词的形式，如`isEmpty`和`isTerminated`，这样放到if语句才便于理解。

P20，4.2.1 启发式



### 算法选择

算法选择的权衡指标：

1. 正确性：算法是否完全正确?
2. 效率：在某些场合，对程序效率的追求是否具有重要意义
3. 可理解性：算法是否容易理解。

算法评估：衡量算法的好坏，主要是效率。

## 递归

### 循环与递归的比较

|          | 循环                                 | 递归                             |
| -------- | ------------------------------------ | -------------------------------- |
| **执行** | 使用**显式的循环结构重复执行**代码段 | 使用**重复的函数调用执行**代码段 |
| **终止** | 满足终止条件时终止执行               | 问题简化到最简单情形时终止执行   |
| **重复** | 在当前迭代执行结束时进行             | 遇到对同名函数的调用时进行       |

循环和递归**都可能隐藏程序错误**，例如循环的条件测试可能永远为真，递归可能永远退化不到最简单情形。

理论上，任何递归程序都可以用循环迭代的方法解决，但注意：

- 递归函数的代码更短小精悍
- 一旦掌握递归的思考方法，递归程序更容易理解

### 递归信任

**递归实现是否检查了最简单情形**

- 在尝试将问题分解成子问题前，首先应检查问题是否已足够简单
- 在大多数情况下，递归函数以if开头
- 如果程序不是这样，仔细检查源程序

**是否解决了最简单情形**

- 大量递归错误是由没有正确解决最简单情形导致的
- 最简单情形不能调用递归

**递归分解是否是问题更简单**

- 只有分解出的子问题更简单，递归才能正确工作，否则将形成无限递归，算法无法终止

**问题简化过程是否能确实回归最简单情形，还是遗漏了某些情况**

- 如汉诺塔问题需要调用两次递归过程，程序中如果遗漏了任意一个都会导致错误

**子问题是否与原始问题完全一致**

- 如果递归过程改变了问题实质，则整个过程肯定会得到错误结果

**使用递归信任时，子问题的解是否正确组装为原始问题的解**

- 将子问题的解正确组装以形成原始问题的解也是必不可少的步骤

*满足了以上6个条件，递归就一定可以正常工作，这就是递归信任。*



## 容错



# 第五讲 C++ 程序组织与开发方法

## 库与接口

### 库与程序文件

- 程序文件:源文件(*.cpp)、头文件(*.h、*.hpp、*)
- 库:源文件与头文件

### 接口

- 通过接口使用库:包括指定库的头文件与源文件
- 优势:不需了解库的实现细节,只需了解库的使用方法

### 标准库

#### C标准库

- 标准输入输出库、工具与辅助函数库、字符串库

#### C++标准库

- 输入输出流库、字符串库、标准模板库

### 数学库

#### 数学库

> 数学库在windows和linux里的用法不同，在linux中如果用到数学库，编译时需要链接。(不用啊- -)

- 头文件:math.h/cmath
- 库文件:libm
- 链接方式:g++ -lm main.cpp
#### 数学函数
- 三角函数与反三角函数系列
- 幂函数与对数函数系列
- 其他数学函数

### 标准辅助函数库

#### 工具与辅助函数

- 头文件:stdlib.h/cstdlib

#### 常用函数

- void exit( int status );
- void free( void * p );
- void * malloc( size_t size );
- int rand();
- void srand( unsigned int seed );

## 随机数库

### 随机数的生成

rand()和srand()

### 接口设计原则

- **用途一致** 接口中所有函数都属于同一类问题
- **操作简单** 函数调用方便,最大限度隐藏操作细节
- **功能充足** 满足不同潜在用户的需要
- **性能稳定** 经过严格测试,不存在程序缺陷

### 随机数库接口

有了接口设计原则，我们可以据此分析出设计一个随机数库的思路。

#### 设计随机数接口

1. 随机化：void Randomize();
2. 生成指定范围内的指定整数：int GenerateRandomNumber( int low, int high );
3. 生成指定范围内的随机实数：double GenerateRandomReal( double low, double high );

#### 随机数库的实现

```c++
#include <iostream>
#include <cstdlib>
#include <ctime>
#include "random.h"

using namespace std;

/*
* 随机化：void Randomize();
*/
void Randomize()
{
srand( (int)time(NULL) ); //time()取当前时间，返回int，NULL时预定义的头 = 0
}

/*
* 生成指定范围内的指定整数：int GenerateRandomNumber( int low, int high );
*/
// 在设计这两个函数时要保证这两个数要能平均地映射到新的区间里
int GenerateRandomNumber( int low, int high )
{
double _d;
if( low > high )
{
cout << "GenerateRandomNumber: Make sure low <= high.\n";
exit( 1 ); // 退出
}
_d = (double)rand() / ((double)RAND_MAX + 1.0);
return (low + (int)(_d * (high - low + 1)));
}

/*
* 生成指定范围内的随机实数：double GenerateRandomReal( double low, double high );
*/
double GenerateRandomReal( double low, double high )
{
double _d;
if( low > high )
{
cout << "GenerateRandomReal: Make sure low <= high.\n";
exit( 2 );
}
_d = (double)rand() / (double)RAND_MAX;
return (low + _d * (high - low));
}
```

#### 随机数库测试

##### 单独测试库的所有函数

- 合法参数时返回结果是否正确
- 非法参数时返回结果是否正确,即容错功能是否正常

##### 联合测试

- 多次运行程序,查看生成的数据是否随机
- 测试整数与浮点数随机数是否均能正确工作

#### 演示：

新建文件夹`Random`, 在`Random`文件夹下分别建立`zyrandom.h`, `zyrandom.cpp`, `main.cpp`三个文件。

`zyrandom.h`:

```c++
void Randomize();
int GenerateRandomNumber(int min, int max); // 随机整数
double GenerateRandomReal(double min, double max); // 随机实数
```

`zyrandom.cpp` :

```cpp
#include "iostream"
#include "cstdlib"
#include "ctime"

using namespace std;

void Randomize(){
    srand((int)time(0)); // 用当前时间作为随机数生成器的种子
}

int GenerateRandomNumber(int min, int max){
    double _d;
    if(min > max){
        cout << "GenerateRandomNumber: Make sure min <= max\n";
        exit(1);
    }
    _d = (double)rand() / ((double)RAND_MAX + 1.0);
    return min + (int)(_d * (max - min + 1));
}

double GenerateRandomReal(double min, double max){
    double _d;
    if(min > max){
        cout << "GenerateRandomReal: Make sure min <= max\n";
        exit(2);
    }
    _d = (double)rand() / ((double)RAND_MAX);
    return min + _d * (max - min);
}
```

`main.cpp` :

```cpp
#include "iostream"
#include "zyrandom.h"

using namespace std;
int main() {
    Randomize();
    for (int i = 0; i < 8; i++) {
        int t = GenerateRandomNumber(10,99);
        cout << t << "  ";
    }
    cout << endl;
    for (int i = 0; i < 8; i++) {
        double d = GenerateRandomReal(10.0, 99.99);
        cout << d << "  ";
    }
    cout << endl;
    return 0;
}
```

编译：因为main.cpp用到了zyrandom.cpp的函数，所以要一起编译。

```shell
$ g++ main.cpp zyrandom.cpp 
```

运行：

```shell
$ ./a.out
# Output:
84  89  49  26  76  71  88  29  
72.7314  76.0392  52.9005  37.4676  73.8128  34.8042  49.0695  57.0016 
```

**思考：**怎么重新设计随机数库，让用户不需要调用randmize()就能正确生成随机数？

## 作用域与生存期

### 量的作用域与可见性

#### 作用域与可见性

- **作用域:**标识符的有效范围
- **可见性:**程序中某个位置是否可以使用某个标识符
- 标识符仅在其作用域内可见
- 位于作用域内的标识符不一定可见

#### 局部数据对象

- 定义于函数或复合语句块内部的数据对象(包括变量、常量与函数形式
  参数等)
- 局部数据对象具有块作用域,仅在定义它的块内有效
- 有效性从定义处开始直到该块结束
- 多个函数定义同名的数据对象是允许的,**原因**?

见：

```c++
int func( int x, int y )
{
int t;
t = x + y;
/* 单独出现的花括号对用于引入嵌套块 */
{
/* 允许在块中定义数据对象,作用域仅限本块 */
int n = 2;
cout << "n = " << n << endl;
}
return t;
}
```

#### 量的作用域与可见性

##### 全局数据对象

- 定义于函数或复合语句块之外的数据对象
- 全局数据对象具有文件(全局)作用域,有效性从定义处开始直到
- 本文件结束,其后函数都可直接使用
- 若包含全局数据对象定义的文件被其他文件包含,则其作用域扩展到宿主文件中,**这可能会导致问题,为什么?**
- **不要在头文件中定义全局数据对象** -放在头文件中非常容易被包含，一旦被包含，作用域就会扩散过去，编译器可能会报错：不允许定义同名的全局变量。

##### 函数原型作用域

- 定义在函数原型中的参数具有函数原型作用域,其有效性仅延续到
  此函数原型结束
- 函数原型中参数名称可以与函数实现中的不同,也可以省略

##### 作用域与可见性示例

```c++
#include "iostream"
#include "iomanip" // 制表

using namespace std;

int i;	/* 全局变量 i 作用域开始,可见 */
int func( int x );
int main()
{
int n;	/* 局部变量 n 作用域开始,可见 */
i = 10;	/* 全局变量 i 有效且可见 */
cout << "i = " << setw(2) << i << "; n = " << n << endl;
n = func( i );
cout << "i = " << setw(2) << i << "; n = " << n << endl;
} /* 局部变量 n 作用域结束,不再可见 */
int n;	/* 全局变量 n 作用域开始,可见 */
int func( int x )	/* 形式参数 x 作用域开始,可见 */
{
i = 0;	/* 全局变量 i 有效且可见 */
cout << "i = " << setw(2) << i << "; n = " << n << endl;
n = 20;	/* 全局变量 n 有效且可见 */
{	/* 嵌套块开始 */
int i = n + x; 	/* 局部变量 i、x 有效可见;全局变量 n 有效可见;全局变量 i 有效不可见 */
cout << "i = " << setw(2) << i << "; n = " << n << endl;
}	/* 局部变量 i 作用域结束,全局变量 i 有效且可见 */
return ++i;
}	/* 局部变量 x 作用域结束,不再可见 */
/* 文件结束,全局变量 i、n 作用域结束 */
```

### 量的存储类与生存期

**生存期:量在程序中存在的时间范围**
	C/C++ 使用存储类表示生存期——作用域表达量的空间特性,存储类表达量的时间特性
**静态(全局)生存期**
	全局数据对象具有静态(全局)生存期——生死仅与程序是否执行有关
**自动(局部)生存期**

- 局部数据对象具有自动(局部)生存期
- 生死仅与程序流程是否位于该块中有关
- 程序每次进入该块时就为该对象分配内存,退出该块时释放内存;
- **两次进入该块时使用的不是同一个数据对象**

### 函数的作用域与生存期

#### static关键字

##### 修饰局部变量:静态局部变量

- 使局部变量具有静态生存期
- 程序退出该块时局部变量仍存在,并且下次进入该块时使用上一次的数据值
- 静态局部变量必须进行初始化
- 不改变量的作用域,仍具有块作用域,即只能在该块中访问,其他代码段不可见

##### 修饰全局变量

- 使其作用域仅限定于本文件内部,其他文件不可见

示例： 检查下述程序的输出结果

```c++
#include <iostream>
using namespace std;
int func( int x );
int main()
{
int i;
for( i = 1; i < 4; i++ )
cout << "Invoke func " << i << " time(s): Return " << func(i) << ".\n";
return 0;
}
int func( int x )
{
static int count = 0; /* 定义静态局部变量count,函数结束后仍存在 */
cout << "x = " << x << ".\n";
return ++count;
}
```

#### 函数的作用域与生存期

所有函数具有文件作用域与静态生存期
	在程序每次执行时都存在,并且可以在函数原型或函数定义之后的
	任意位置调用
内部函数与外部函数
	外部函数:可以被其他文件中的函数所调用
	内部函数:不可以被其他文件中的函数所调用
	函数缺省时均为外部函数
	内部函数定义:使用 static 关键字
	内部函数示例:static int Transform( int x );
	内部函数示例:static int Transform( int x ){ ... }

### 声明与定义

声明不是定义
	定义在程序产生一个新实体
	声明仅仅在程序中引入一个实体
函数的声明与定义
	声明是给出函数原型,定义是给出函数实现代码
类型的声明与定义
	产生新类型就是定义
	类型定义示例:typedef enum __BOOL { FALSE, TRUE } BOOL;
	不产生新类型就不是定义,而仅仅是声明
	类型声明示例: enum __BOOL;

#### 全局变量的作用域扩展

**全局变量的定义不能出现在头文件中,只有其声明才可以出现**
在头文件中
声明格式:**使用 extern 关键字**

```c++
/* 库的头文件 */
/* 此处仅引入变量 a,其定义位于对应源文件中 */
extern int a; /* 变量 a 可导出,其他文件可用 */
/* 库的源文件 */
/* 定义变量 a */
int a;
```



## 典型软件开发流程

### 软件工程概要

### 问题的提出

### 需求分析

### 概要设计

### 详细设计

![](/CPP/Pictures/main.png)

### 编码实现

### 系统测试

### 经验总结
