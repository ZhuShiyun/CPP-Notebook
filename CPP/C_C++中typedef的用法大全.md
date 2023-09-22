# C/C++中typedef的用法大全

> 参考来源：C/C++中typedef的用法大全 https://www.jb51.net/article/282360.htm
>
> 其他参考：C++ typedef常见用法详解 https://www.jb51.net/article/277438.htm

typedef用法一共七种，分别是：为基本数据类型起别名、为结构体起别名、为指针类型起别名、为数组类型起别名、为枚举类型起别名、为模版函数起别名。

## 一、为基本数据类型起别名

```c++
typedef int myint;
myint x = 5;
```

"myint"是"int"的别名，可以使用"myint"来代替"int"声明变量。应用较少。

## 二、为结构体起别名

```c++
typedef struct {
    int x;
    int y;
} Point;
 
Point p = { 3, 4 };
```

## 三、为指针类型起别名

"intptr"是指向"int"类型的指针的别名，可以使用"intptr"来声明指针变量。为防止代码阅读者有障碍，慎用。

```c++
typedef int* intptr;
intptr p = new int;
*p = 5;
```

## 四、为函数指针类型起别名 (理解中。。)

在这个例子中，"func_ptr"是指向函数的指针类型的别名，可以使用"func_ptr"来声明函数指针变量。

```c++
typedef int (*func_ptr)(int, int);
int add(int a, int b) { return a + b; }
 
func_ptr f = add;
int result = (*f)(3, 4);

```

这个在DLL导出用到的比较多，如：

```c++
typedef MyInterface* (*CreateMyObjectFunc)();  
```

在这个例子中，typedef 声明了一个名为 CreateMyObjectFunc 的新类型。CreateMyObjectFunc 是一个函数指针类型，它指向一个返回值为 MyInterface* 类型的函数，该函数没有参数。

这种函数指针类型的定义通常用于动态加载库文件中的函数。通过这种方式，可以定义一个函数指针类型来代表动态加载的库文件中的函数，并将其作为参数传递给动态加载函数。然后可以使用该函数指针类型调用动态加载函数中的函数。在这种情况下，CreateMyObjectFunc 函数指针类型可以用于动态加载库文件中的一个函数，该函数返回一个 MyInterface 类型的指针。

## 五、为数组类型起别名

```c++
typedef int myarray[10];
myarray arr = { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };
```

## 六、为枚举类型起别名

```cpp
typedef enum { red, green, blue } Color;
Color c = green;
```

## 七、为模版函数起别名

在这个例子中，"IntStruct"是模板类型"MyStruct"的具体化，可以使用"IntStruct"来声明"MyStruct<int>"类型的变量。

```c++
template <typename T>
struct MyStruct {
    T value;
};
 
typedef MyStruct<int> IntStruct;
IntStruct s = { 5 };
```

