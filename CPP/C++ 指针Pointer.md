# C++ 指针Pointer

[TOC]

前置知识：&运算符访问变量内存地址，例：

```c++
#include "iostream"

using namespace std;

int main(){
    int var1;
    char var2[10];
    
    cout << "var1 变量的地址： ";
    cout << &var1 << endl;
 
    cout << "var2 变量的地址： ";
    cout << &var2 << endl;
 
    return 0;
}
```

Output:

```c++
var1 变量的地址： 0xbfebd5c0
var2 变量的地址： 0xbfebd5b6
```

## 指针

**指针**是一个变量，其值为另一个变量的地址，即，内存位置的直接地址。就像其他变量或常量一样，您必须在使用指针存储其他变量地址之前，对其进行声明。指针变量声明的一般形式为：

```c++
type *var-name;
```

在这里，**type** 是指针的基类型，它必须是一个有效的 C++ 数据类型，**var-name** 是指针变量的名称。用来声明指针的星号 * 与乘法中使用的星号是相同的。但是，在这个语句中，星号是用来指定一个变量是指针。以下是有效的指针声明：

```c++
int    *ip;    /* 一个整型的指针 */
double *dp;    /* 一个 double 型的指针 */
float  *fp;    /* 一个浮点型的指针 */
char   *ch;    /* 一个字符型的指针 */
```

所有指针的值的实际数据类型，不管是整型、浮点型、字符型，还是其他的数据类型，都是一样的，都是一个代表内存地址的长的十六进制数。不同数据类型的指针之间唯一的不同是，指针所指向的变量或常量的数据类型不同。

## C++中使用指针

```c++
#include <iostream>

using namespace std;

int main(){
    int a = 20;
    int *ip;

    ip = &a;

    cout << "Value of a variable:" << a << endl;
    cout << "Address stored in ip variable:" << ip << endl;
    cout << "Value of *ip variale:" << *ip << endl;

    return 0;
}
```

Output:

```
Value of a variable:20
Address stored in ip variable:0x7ffcb8e2bb5c
Value of *ip variale:20
```



## C++ Null 指针

在变量声明的时候，如果没有确切的地址可以赋值，为指针变量赋一个 NULL 值是一个良好的编程习惯。赋为 NULL 值的指针被称为**空**指针。

NULL 指针是一个定义在标准库中的值为零的常量。

```c++
#include <iostream>

using namespace std;

int main ()
{
   int  *ptr = NULL;

   cout << "ptr 的值是 " << ptr ;
 
   return 0;
}
```

Output:

```
ptr 的值是 0
```

在大多数的操作系统上，程序不允许访问地址为 0 的内存，因为该内存是操作系统保留的。然而，内存地址 0 有特别重要的意义，它表明该指针不指向一个可访问的内存位置。但按照惯例，如果指针包含空值（零值），则假定它不指向任何东西。

如需检查一个空指针，您可以使用 if 语句，如下所示：

```c++
if(ptr)     /* 如果 ptr 非空，则完成 */
if(!ptr)    /* 如果 ptr 为空，则完成 */
```

因此，如果所有未使用的指针都被赋予空值，同时避免使用空指针，就可以防止误用一个未初始化的指针。很多时候，未初始化的变量存有一些垃圾值，导致程序难以调试

**Note:** 空指针**不提倡**使用**NULL**来表示，C++11标准后，用**nullptr**来表示空指针。见：https://en.cppreference.com/w/cpp/language/nullptr

## C++ 指针的算术运算

### 递增一个指针

我们喜欢在程序中使用指针代替数组，因为变量指针可以递增，而数组不能递增，因为数组是一个常量指针。下面的程序**递增变量指针，以便顺序访问数组中的每一个元素**：

```c++
#include <iostream>
 
using namespace std;
const int MAX = 3;
 
int main ()
{
   int  var[MAX] = {10, 100, 200};
   int  *ptr;
 
   // 指针中的数组地址
   ptr = var;
   for (int i = 0; i < MAX; i++)
   {
      cout << "Address of var[" << i << "] = ";
      cout << ptr << endl;
 
      cout << "Value of var[" << i << "] = ";
      cout << *ptr << endl;
 
      // 移动到下一个位置
      ptr++;
   }
   return 0;
}
```

Output:

```
10
Address of var[0] = 0x7fffffffe24c
Value of var[0] = 10
Address of var[1] = 0x7fffffffe250
Value of var[1] = 100
Address of var[2] = 0x7fffffffe254
Value of var[2] = 200
```

### 递减一个指针

同样地，对指针进行递减运算，即把值减去其数据类型的字节数，如下所示：

```c++
#include <iostream>
 
using namespace std;
const int MAX = 3;
 
int main ()
{
   int  var[MAX] = {10, 100, 200};
   int  *ptr;
 
   // 指针中最后一个元素的地址
   ptr = &var[MAX-1];
   for (int i = MAX; i > 0; i--)
   {
      cout << "Address of var[" << i << "] = ";
      cout << ptr << endl;
 
      cout << "Value of var[" << i << "] = ";
      cout << *ptr << endl;
 
      // 移动到下一个位置
      ptr--;
   }
   return 0;
}
```

Output:

```
Address of var[3] = 0x7ffe97118034
Value of var[3] = 200
Address of var[2] = 0x7ffe97118030
Value of var[2] = 100
Address of var[1] = 0x7ffe9711802c
Value of var[1] = 10
```

### 指针的比较

## C++ 指向指针的指针（多级间接寻址）

指向指针的指针是一种多级间接寻址的形式，或者说是一个指针链。

指针的指针就是将指针的地址存放在另一个指针里面。

通常，一个指针包含一个变量的地址。当我们定义一个指向指针的指针时，第一个指针包含了第二个指针的地址，第二个指针指向包含实际值的位置。

```c++
#include <iostream>

using namespace std;

int main ()
{
    int  var;
//    int  *ptr;
    int  **pptr;
    int  *ptr = nullptr;
//    int  **pptr = nullptr;

    if(ptr){
        cout << "ptr非空" << endl;  /* 如果 ptr 非空，则完成 */
    } else {
        cout << "ptr为空" << endl; /* 如果 ptr 为空，则完成 */
    }

    var = 3000;

    // 获取 var 的地址
    ptr = &var;

    // 使用运算符 & 获取 ptr 的地址
    pptr = &ptr;

    // 使用 pptr 获取值
    // var, ptr, pptr的地址都是一样的
    cout << "Address of var is: " << &var << endl;
    cout << "Address of ptr is: " << ptr << endl;
    cout << "var 值为 :" << var << endl;
    cout << "*ptr 值为:" << *ptr << endl;
    cout << "**pptr 值为:" << **pptr << endl;

    return 0;
}
```

## C++ 指针 vs 数组

[link](https://www.runoob.com/cplusplus/cpp-pointers-vs-arrays.html)

指针和数组并不是完全互换的。例如

```c++
#include <iostream>
 
using namespace std;
const int MAX = 3;
 
int main ()
{
   int  var[MAX] = {10, 100, 200};
 
   for (int i = 0; i < MAX; i++)
   {
      *var = i;    // 这是正确的语法
      var++;       // 这是不正确的
   }
   return 0;
}
```

本教程下方的一篇笔记指出，教程示例中的 *var = i; 实际上是改变了数组中的第一个元素的值。

## C++ 指针数组

在我们讲解指针数组的概念之前，先让我们来看一个实例，它用到了一个由 3 个整数组成的数组：

```c++
#include <iostream>
 
using namespace std;
const int MAX = 3;
 
int main ()
{
   int  var[MAX] = {10, 100, 200};
 
   for (int i = 0; i < MAX; i++)
   {
      cout << "Value of var[" << i << "] = ";
      cout << var[i] << endl;
   }
   return 0;
}
```

Output:

```
Value of var[0] = 10
Value of var[1] = 100
Value of var[2] = 200
```

可能有一种情况，我们想要让数组存储指向 int 或 char 或其他数据类型的指针。下面是一个指向整数的指针数组的声明：

```c++
int *ptr[MAX];
```

在这里，把 **ptr** 声明为一个数组，由 MAX 个整数指针组成。因此，ptr 中的每个元素，都是一个指向 int 值的指针。下面的实例用到了三个整数，它们将存储在一个指针数组中，如下所示：

```c++
#include <iostream>

using namespace std;
const int MAX = 3;

int main ()
{
    int  var[MAX] = {10, 100, 200};
    int *ptr[MAX];

    for (int i = 0; i < MAX; i++)
    {
        ptr[i] = &var[i]; // 赋值为整数的地址
    }
    cout << "ptr0:" << ptr[0] << endl;
    cout << "ptr1:" << ptr[1] << endl;

    for (int i = 0; i < MAX; i++)
    {
        cout << "Value of var[" << i << "] = ";
        cout << *ptr[i] << endl;
    }
    return 0;
}
```

Output:

```
ptr0:0x7fffa37f9c94
ptr1:0x7fffa37f9c98
Value of var[0] = 10
Value of var[1] = 100
Value of var[2] = 200
```

也可以用一个指向字符的指针数组来存储一个字符串列表，如下：

```c++
#include <iostream>
 
using namespace std;
const int MAX = 4;
 
int main ()
{
 const char *names[MAX] = {
                   "Zara Ali",
                   "Hina Ali",
                   "Nuha Ali",
                   "Sara Ali",
   };
 
   for (int i = 0; i < MAX; i++)
   {
      cout << "Value of names[" << i << "] = ";
      cout << names[i] << endl;
   }
   return 0;
}
```

Output:

```
Value of names[0] = Zara Ali
Value of names[1] = Hina Ali
Value of names[2] = Nuha Ali
Value of names[3] = Sara Ali
```

Note:

`char *names[MAX]` 这种字符型的指针数组是存储指针的数组，但是在理解字符型指针数组的时候，可以将它理解为一个二维数组，如  `const char *names[4] = {"Zara Ali","Hina Ali","Nuha Ali","Sara Ali",}` 可以理解为一个 4 行 8 列的数组，可以用 `cout << *(names[i] + j)<< endl` 取出数组中的每个元素：

```c++
#include <iostream>

using namespace std;

const int MAX = 4;
int main ()
{
    const char *names[MAX] = {
        "Zara Ali",
        "Hina Ali",
        "Nuha Ali",
        "Sara Ali",
    };

    for (int i = 0; i < MAX; i++)
        for (int j = 0; j < 8; j++)
        {
            cout << "Value of names[" << i << "] = ";
            cout << *(names[i] + j)<< endl;
        }
    return 0;
}
```

Output:

```
Value of names[0] = Z
Value of names[0] = a
Value of names[0] = r
Value of names[0] = a
Value of names[0] =  
Value of names[0] = A
Value of names[0] = l
Value of names[0] = i
Value of names[1] = H
Value of names[1] = i
Value of names[1] = n
Value of names[1] = a
Value of names[1] =  
Value of names[1] = A
Value of names[1] = l
Value of names[1] = i
Value of names[2] = N
Value of names[2] = u
Value of names[2] = h
Value of names[2] = a
Value of names[2] =  
Value of names[2] = A
Value of names[2] = l
Value of names[2] = i
Value of names[3] = S
Value of names[3] = a
Value of names[3] = r
Value of names[3] = a
Value of names[3] =  
Value of names[3] = A
Value of names[3] = l
Value of names[3] = i
```

## C++ 传递指针给函数

C++ 允许您传递指针给函数，只需要简单地声明函数参数为指针类型即可。

下面的实例中，我们传递一个无符号的 long 型指针给函数，并在函数内改变这个值：

```c++
#include <iostream>
#include <ctime>
 
using namespace std;
 
// 在写函数时应习惯性的先声明函数，然后再定义函数
void getSeconds(unsigned long *par);
 
int main ()
{
   unsigned long sec;
 
 
   getSeconds( &sec );
 
   // 输出实际值
   cout << "Number of seconds :" << sec << endl;
 
   return 0;
}
 
void getSeconds(unsigned long *par)
{
   // 获取当前的秒数
   *par = time( NULL );
   return;
}
```

Output:

```
Number of seconds :1695625389
```

能接受指针作为参数的函数，也能接受数组作为参数，如下所示：

```c++
#include <iostream>
using namespace std;

// 函数声明
double getAverage(int *arr, int size);

int main(){
    // 带有5个元素的整形数组
    int balance[5] = {2016,3,1,12,7};
    double  avg;

    // 传递一个指向数组的指针作为参数
    avg = getAverage(balance, 5);

    // 输出返回值
    cout << "Average value is:" << avg << endl;

    return 0;
}

double getAverage(int *arr,int size){
    int i, sum = 0;
    double avg;

    for (int i = 0; i < size; ++i) {
        sum += arr[i];
    }

    avg = double(sum/size);

    return avg;
}
```

Output:

```
Average value is:407
```

## C++ 从函数返回指针

[link](https://www.runoob.com/cplusplus/cpp-return-pointer-from-functions.html)