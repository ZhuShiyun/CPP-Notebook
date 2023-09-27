# C++ 泛型编程

> [泛型编程](https://so.csdn.net/so/search?q=泛型编程&spm=1001.2101.3001.7020)：编写与类型无关的通用代码，是代码复用的一种手段。模板是泛型编程的基础

参考：

> C++入门泛型编程介绍 https://blog.csdn.net/m0_53421868/article/details/121188326
>
> C++ 模板 https://www.runoob.com/cplusplus/cpp-templates.html
>
> C++ 泛型编程 https://blog.csdn.net/weixin_45423515/article/details/126593257#t2

格式：

```c++
template<typename T1, typename T2,......,typename Tn>
ret-type返回值类型 func-name函数名(参数列表){
	// 函数主体
}
```

例如，交换两个值的例子：

```c++
/**
 * 泛型编程 模板
 */
#include "iostream"

using namespace std;

template<typename T>
T Swap(T &num1, T &num2){
    T t = num1;
    num1 = num2;
    num2 = t;
}

int main(){
    float a = 2.7;
    float b = 10.4;
    Swap(a,b); // 隐式实例化
    cout << "a:" << a << "b:" << b <<endl;
    return 0;
}
```

流程图：

![](https://img-blog.csdnimg.cn/663904833f2b48a4b16f184979482b38.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBASVTojqvmiY7nibk=,size_20,color_FFFFFF,t_70,g_se,x_16)