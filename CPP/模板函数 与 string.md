# 模板函数 与 string 

2023.09.27

学习模板函数时写了如下代码作为练习：

```c++
/**
 * 泛型编程 模板 
 */
#include "iostream"
#include "string"

using namespace std;

// 交换两个参数的模板函数
template<typename T>
T Swap(T &param1, T &param2){
    T t = param1;
    param1 = param2;
    param2 = t;
}

int main(){
    setlocale(LC_ALL,"");

    float a = 2.7;
    float b = 10.4;
    Swap(a,b);
    cout << "a:" << a << "b:" << b << endl;

    string c = "qwe";
    string d = "rty";
    Swap(c,d);
    cout << c << d << endl;
    return 0;
}为什么float类型的数字就可以成功交换而string不行？
```

Output:

```c++
a:10.4b:2.7
// 这里有个空行
进程已结束，退出代码为 139 (interrupted by signal 11: SIGSEGV)
```

ChatGPT的解释：

这个问题出现的原因是在你的 `Swap` 函数中，你没有指定返回类型（缺少 `void` 或其他类型），导致默认的返回类型是 `int`，而你的 `Swap` 函数没有返回语句，这会导致未定义的行为。

为了修复这个问题，你需要将 `Swap` 函数的返回类型设置为 `void`，因为交换函数不需要返回任何值。

将代码改成这样就好了：

```c++
/**
 * 泛型编程 模板
 */
#include "iostream"
#include "string"

using namespace std;

template<typename T>
void Swap(T &param1, T &param2){      // 唯一改动
    T t = param1;
    param1 = param2;
    param2 = t;
}

int main(){
    setlocale(LC_ALL,"");

    float a = 2.7;
    float b = 10.4;
    Swap(a,b);
    cout << "a:" << a << "b:" << b << endl;

    string c = "qwe";
    string d = "rty";
    Swap(c,d);
    cout << c << d << endl;
    return 0;
}
```

以及， 改成这样也不行：

```c++
template<typename T>
T Swap(T &param1, T &param2){
    T t = param1;
    param1 = param2;
    param2 = t;
    return 0; // 不是说缺返回值嘛QAQ
}
```

运行提示：

```c++
a:10.4b:2.7
terminate called after throwing an instance of 'std::logic_error'
  what():  basic_string::_M_construct null not valid

进程已结束，退出代码为 134 (interrupted by signal 6: SIGABRT)
```

**Q：为什么float类型的数字就可以成功交换而string不行？**

A：在本段代码中，`float` 类型的数字可以成功交换，而 `string` 类型的字符串不能成功交换的原因是因为 C++ 的模板函数对不同类型的参数有不同的处理方式。

对于 `float` 类型的参数，模板函数 `Swap` 可以正常工作，因为它们是**基本数据类型**，交换操作是直接对值进行的。

但对于 `string` 类型的参数，情况稍有不同。`string` 是一个**类类型**（class type），交换两个 `string` 对象时，默认情况下会调用它们的**拷贝构造函数**和**析构函数**。因此，当你的 `Swap` 函数尝试交换两个 `string` 对象时，它们的内容被交换了，但对象本身的状态也会受到影响。这可能导致一些未定义的行为，例如在程序尝试销毁已被交换的 `string` 对象时出现问题，从而导致 SIGSEGV 错误。

为了避免这个问题，可以使用**模板特化**来针对 `string` 类型进行特殊处理，以确保对象的状态不会受到破坏。以下是一个修改后的代码示例，使用了模板特化来处理 `string` 类型：

```c++
#include "iostream"
#include "string"

using namespace std;

template<typename T>
void Swap(T &num1, T &num2){
    T t = num1;
    num1 = num2;
    num2 = t;
}

// 模板特化，针对 string 类型进行特殊处理
// 在本题中非必须。
template<>
void Swap(string &str1, string &str2){
    string temp = str1;
    str1 = str2;
    str2 = temp;
}

int main(){
    setlocale(LC_ALL,"");
    float a = 2.7;
    float b = 10.4;
    Swap(a, b);
    cout << "a:" << a << " b:" << b << endl;

    string c = "qwe";
    string d = "rty";
    Swap(c, d);
    cout << c << " " << d << endl;
    return 0;
}
```

**Q**：既然把T改成void就可以让两个string成功交换了，那你前面说的模板特化还有必要吗？

A：你提出的观点是正确的。如果将 `Swap` 函数的返回类型更改为 `void`，那么**在这个特定的情况下，确实不需要模板特化来处理 `string` 类型**。**只有当你想要保持 `Swap` 函数返回一个值，并且在不同类型之间有不同的交换逻辑时，模板特化才是有必要的。**

在你的示例中，将 `Swap` 函数的返回类型更改为 `void` 是解决问题的有效方法，因为它适用于所有类型，包括 `float` 和 `string`。这样，不需要额外的特化来处理 `string` 类型，因为 `void` 返回类型适用于所有类型的参数。

所以，根据你的具体需求，你可以选择使用模板特化或者将返回类型更改为 `void`，两种方法都是有效的，取决于你的代码设计。