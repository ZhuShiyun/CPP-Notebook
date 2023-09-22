# 析构函数的初学

---

[TOC]



## 一、什么是析构函数

- 名字和类名相同，在前面加‘~’, 没有参数和返回值，一个类最多只能有一个析构函数 。
- 析构函数对象消亡时即自动被调用。可以定义析构函数来在对象消亡前做善后工作，比如释放分配的空间等 。
- 如果定义类时没写析构函数，则编译器自动生成缺省析构函数。缺省析构函数什么也不做。

```c++
#include<iostream>
using namespace std;
class A
{
	public:
		A()
		{
			cout<<"构造函数"<<endl; 
		}
		~A()
		{
			cout<<"析构函数"<<endl;
		 } 
};
int main() 
{
	A a;//a为A的对象 
	return 0;
}//在对象消失时，析构函数自动被调用，释放对象占用的空间
```

## **二、析构函数和数组**

- 对象数组生命期结束时，**对象数组的每个元素的析构函数都会被调用**

```c++
#include<iostream>
using namespace std;
class text
{
	public:
		~text()
		{
			cout<<"1111"<<endl; 
		}
 } ;
int main()
{
 	text array[2];//array是一个数组，里面有两个text的对象 
 	cout<<"end main"<<endl;//所所以对象消亡后调用两次析构函数，即两次输出“1111” 
	 return 0; 
  } 
```

## 三、析构函数和运算符delete

- delete运算导致析构函数调用
- 栈中内存由系统自动分配和释放；使用new创建的指针对象在堆中分配内存，不需要时，需要手动释放

代码举例一：有new出来的对象，没有delete

```c++
#include<iostream>
using namespace std;
class A
{
	public:
		A()
		{
			cout<<"构造函数"<<endl; 
		}
		~A()
		{
			cout<<"析造函数"<<endl; 
		 } 
 };
 int main()
 {
 	A a;//最后的析构函数是a的析构，a是在栈中分配的内存 
 	A * p; 
 	p=new A();//new出来的对象，用构造函数A()初始化 
	// 最后没有delete,该对象就不会消亡，就不会引发析构函数的调用 
 	cout<<"end of main"<<endl;
 	return 0;
 }
```
代码举例二：有new和delete

```c++
#include<iostream>
using namespace std;
class A
{
	public:
		A()
		{
			cout<<"构造函数"<<endl; 
		}
		~A()
		{
			cout<<"析构函数"<<endl; 
		 } 
 };
 int main()
 {
 	A a;//创建了A的对象a,调用无参的构造函数，输出第一行的“构造函数” 
 	A * p; 
 	p=new A();//new A在堆中分配对象，同时调用无参的构造函数， 输出第二行的“构造函数”
 	delete p;//删除p指向的空间，也就是释放这个对象，必然要调用析构函数，输出第三行的“析构函数” 
 	cout<<"end of main"<<endl;
 	return 0;
 }

```

代码举例三：new一个对象数组，delete []

```c++
#include<iostream>
using namespace std;
class A
{
	public:
		A()
		{
			cout<<"构造函数"<<endl; 
		}
		~A()
		{
			cout<<"析构函数"<<endl; 
		 } 
 };
int main()
{
	A * a;//定义一个指针a 
	a=new A[3];//调用构造函数3次 
	delete []a;//析构函数调用3次 
	return 0;
}
```

相反：如果new一个对象数组，只是delete一个对象（造成只调用析构函数一次）

```c++
#include<iostream>
using namespace std;
class A
{
	public:
		A()
		{
			cout<<"构造函数"<<endl; 
		}
		~A()
		{
			cout<<"析构函数"<<endl; 
		 } 
 };
int main()
{
	A * a; 
	a=new A[3];//调用构造函数3次 
	delete a;//析构函数调用1次 
	return 0;
}
```

## 四、析构函数在对象作为函数返回值(临时对象)返回后被调用

```cpp
#include<iostream>
using namespace std;
class cmyclass
{
	public:
		~cmyclass()
		{
			cout<<"destructor"<<endl;
		 } 
 } ;
cmyclass obj;//全局对象 
cmyclass fun(cmyclass sobj)//sobj是用构造函数初始化，但是没写构造函数，所以是系统默认的构造函数 
//fun()函数返回时，sobj(参数对象)会消亡，从而导致导致析构函数被调用(输出第一个destructor) 
{
	return sobj; //函数调用返回时生成临时对象返回 
}
int main()
{
	obj=fun(obj);//将临时对象fun(obj)的值赋值给obj，然后临时对象消亡，同时调用析构函数
	//(第二个desttuctor) 
	return 0;//此处整个程序结束，上面的全局对象也会消亡(第三个destructor) 
 } 
```
## 五、栈中对象释放顺序

- 先创建的后释放，后创建的先释放(先进后出) 

```c++
#include<iostream>
using namespace std;
class A
{
	private:
		int id;
	public:
		A(int i)
		{
			id=i;
			cout<<id<<"---->构造函数"<<endl; 
		}
		~A()
		{
			cout<<id<<"---->析构函数"<<endl; 
		}
};
int main()
{
	A a(1),b(2);
	cout<<"end of main"<<endl;
	return 0;
}
```

![](https://img-blog.csdnimg.cn/3b9610555d20474eb44d04c5c3d5feae.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5oiR5oS_77yM5oiR5oOz,size_10,color_FFFFFF,t_70,g_se,x_16)

## 六、经典代码（栈中对象释放：先进后出）

![](https://img-blog.csdnimg.cn/1b8b970cda284105a32c3cd029c9755f.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5oiR5oS_77yM5oiR5oOz,size_20,color_FFFFFF,t_70,g_se,x_16)

## 七、注意

- 如果类中没有申请资源时，析构函数可以不写，直接使用编译器生成的默认析构函数；有资源申请时，一定要写，否则会造成资源泄漏

---

版权声明：本文为CSDN博主「莫忘、莫念」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/m0_63783532/article/details/123855225

20230921 birb 有少许改动，仅用于整理学习。