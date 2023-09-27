# C++ 引用



[TOC]



## C++ 引用 vs 指针

引用很容易与指针混淆，它们之间有三个主要的不同：

- 不存在空引用。引用必须连接到一块合法的内存。
- 一旦引用被初始化为一个对象，就不能被指向到另一个对象。指针可以在任何时候指向到另一个对象。
- 引用必须在创建时被初始化。指针可以在任何时间被初始化。

## c++ 中创建引用

可以把引用看作变量的另一个名字，比如牡丹鹦鹉"蜜瓜"和"瓜毛毛"都是同一只鸟，如果"瓜毛毛"掉了一根飞羽，"蜜瓜"也会掉一根飞羽，因为它们本来就是同一只鸟。

例如：

```c++
int melon = 127;
```

为melon声明引用变量：

```
int& guamaomao = melon;
```

&读作**引用**，上面的声明可以读作 **"guamaomao 是一个初始化为melon 的整型引用"**

例：

```c++
#include "iostream"

using namespace std;

int main(){
    // 声明简单的变量
    int i;
    double d;
    
    // 声明引用变量
    int& r = i;
    double& s = d;

    i = 7;
    cout << "Value of i:" << i << endl;
    cout << "Value of i reference:" << r << endl;
    cout << "Address of i:" << &i << endl << "Address of r:" << &r <<endl;

    d = 2.7;
    cout << "Value of d:" << d << endl;
    cout << "Value of i reference:" << s << endl;
    cout << "Address of d:" << &d << endl << "Address of s:" << &s <<endl;

    return 0;
}
```

Output:

可以看到不管是i还是它的引用r / 或者不管是d还是它的引用s, **原始变量**和**引用变量**的地址都是一样的。

```c++
Value of i:7
Value of i reference:7
Address of i:0x7ffcd98a0c1c
Address of r:0x7ffcd98a0c1c
Value of d:2.7
Value of i reference:2.7
Address of d:0x7ffcd98a0c20
Address of s:0x7ffcd98a0c20
```

Note:

**C++11** 新增特性 -- **右值引用**，写法示例：

```c++
int &&rvalueReference = 1 + 2;
```

加入**右值引用**的目的主要是为了解决这样一个问题：在函数传值以及返回值的时候，一个对象被复制给另外一个对象，然后这个对象紧接着就被析构了-显然很浪费时间。使用右值引用的话就可以把一个对象的所有权“转让”给另外一个对象，而无需调用复制构造函数和析构函数。



引用通常用于函数参数列表和函数返回值。下面列出了 C++ 程序员必须清楚的两个与 C++ 引用相关的重要概念：

## C++ 把引用作为参数

```c++
#include "iostream"

using namespace std;

// 养成先声明再定义的好习惯*_*
void swap(int& x, int& y);

int main(){
    // 局部变量声明
    int a = 135;
    int b = 246;

    cout << "交换前，a的值：" << a << endl;
    cout << "交换前，b的值：" << b << endl;

    // 调用swap()函数来交换值
    swap(a,b);

    cout << "交换后，a的值：" << a << endl;
    cout << "交换后，b的值：" << b << endl;

    return 0;
}

// 定义函数 So that the values of x and y can be excahged.
void swap(int& x, int& y){
    int temp;
    temp = x;
    x = y;
    y = temp;

    return;
}
```

Output:

```c++
交换前，a的值：135
交换前，b的值：246
交换后，a的值：246
交换后，b的值：135
```



## C++ 把引用作为返回值

通过C++ **把引用作为返回值**，**会使c++程序更容易阅读和维护**。C++函数可以返回一个引用，方式与返回一个指针类似。

当函数返回一个引用时，则返回一个指向返回值的**隐式指针**。这样，函数就可以放在赋值语句的左边。例如，请看下面这个简单的程序：

```c++
#include "iostream"

using namespace std;

// 这其实是历史上的某些日期，dddd
double vals[] = {2.21,2.29,3.1,12.7,20.16};

double& setValues(int i){
    double& ref = vals[i]; // ref:reference
    return ref; // 返回第 i 个元素的引用，ref 是一个引用变量，ref 引用 vals[i]
}


// 主函数将调用上面定义的函数
int main(){
    cout << "T_T Before:" << endl;
    for (int i = 0; i < 5; ++i) {
        cout << "vals[" << i << "]=";
        cout << vals[i] << endl;
    }
    setValues(1) = 0;
    setValues(2) = 0;

    cout << "-_- After:" << endl;
    for (int i = 0; i < 5; ++i) {
        cout << "vals[" << i << "]=";
        cout << vals[i] << endl;
    }
    cout << "You change the history successfully!" << endl;
    
    return 0;
}
```

Output:

```c++
T_T Before:
vals[0]=2.21
vals[1]=2.29
vals[2]=3.1
vals[3]=12.7
vals[4]=20.16
-_- After:
vals[0]=2.21
vals[1]=0
vals[2]=0
vals[3]=12.7
vals[4]=20.16
You change the history successfully!
```

---

**以下内容尚需加深理解**：

当返回一个引用时，要注意被引用的对象不能超出作用域。所以返回一个对局部变量的引用是不合法的，但是，可以返回一个对静态变量的引用。

```cpp
int& func() {
   int q;
   //! return q; // 在编译时发生错误
   static int x;
   return x;     // 安全，x 在函数作用域外依然是有效的
}
```

