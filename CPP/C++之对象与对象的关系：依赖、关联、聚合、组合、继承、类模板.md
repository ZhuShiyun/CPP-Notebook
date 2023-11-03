[TOC]



## 1 - 依赖(Dependency)

### 1.图示

<img src="https://img-blog.csdnimg.cn/8eb0eece148a4b9984f7b5e968fc2616.png" style="zoom:67%;" />

​	

### 2.定义

依赖(Dependency)（使用一个）:

程序类之间的“依赖”关系主要体现出的是一种使用关系，对于两个相对独立的对象， 当一个对象负 责构造另一个对象的实例，或者当一个对象依赖于另一个对象所提供的服务时，这两个对象之间主要体 现为依赖关系。

在类型的设计中，依赖关系主要体现在目标类型的对象（实例），作为当前类型方法的局部对象或 者方法的参数类型

### 3.实现

```cpp
class Book{}; // 书籍
class Food{}; // 食物
class Human // 人类
{
public:
    void Read(Book book);
    void Eat(Food food);
};
 
class Notebook_computer //笔记本计算机
{
    
};
class desktop_computer //台式计算机
{
    class Book{}; // 书籍
    class Food{}; // 食物
    class Human // 人类
{
public:
    void Read(Book book);
    void Eat(Food food);
};
 
class Notebook_computer //笔记本计算机
{
 
};
class desktop_computer //台式计算机
{
private:
    string cup;
    int Memory;
    int ssdist; //Solid state disk
};
class Student
{
public:
    void Study_Programming(Notebook_computer compute); //学习编程
    void Study_Programming(desktop_computer compute);
    void Study_Programming(Book book); //
};
```

## 2.关联(Association)

### 1.图示

![](https://img-blog.csdnimg.cn/c67e6c8aba644e09812feb7040e3bdcd.png)

### 2.定义

使用一个

对于两个相互独立的对象，当对象A与另一个对象B存在固定的对应关系时，这两个对象之间为关 联关系。关联关系是依赖关系的特例。

在类型的设计中，关联关系主要体现在目标类型的指针或引用，作为当前类型的属性成员。没有整 体和部分的关系，只是有关系而已。

### 3.实现

```cpp
class Book {}; // 书籍
class Person
{
private:
	Book* ptr;// 弱关联
public:
	Person() :ptr(nullptr) {}
	Person(Book* p) :ptr(p) {}
	~Person() {}
	void SetBook(Book* p) { ptr = p; }
	void Study()
	{
		if (Ptr != nullptr)
		{
			cout << "看一会书 ... " << endl;
		}
		else
		{
			cout << "没有书可以看 ... " << endl;
		}
	}
};
// 人对象与书籍对象没有整体和部分的关系，只是有关系而已。
class Student
{
	Book& _book;// 强关联
public:
	Student() {} // error;
	Student(Book bk) :_book(bk) {} // error;
	Student(Book& book) :_book(book) {} // ok;
	Student() {}
};
// 学生对象与书籍对象没有整体和部分的关系，只是有关系而已
class Dog;
class Master // 狗主
{
	Dog* pdog;
public:
};
class Dog
{
	Mastrer* pm;
};
// 主人对象与狗对象没有整体和部分的关系，只是有关系而已。
```

 如果Employee类所代表的物理事物与TimeCard类所代表的物理事物存在组成或者构成关系，则此 时的关联就演变为聚合（松散的包含）或者组合（较强的构成）关系。

## 3.聚合(Aggregation)

### 1.图示

![](https://img-blog.csdnimg.cn/6b35690e561944928fd2300a3708413a.png)

### 2.关联与聚合 定义

当对象A是对象B的属性时,称对象B包含对象A。意味着 "用...来实现,用...来构造"

相比于聚合，组合是一种耦合度更强的关联关系。存在组合关系的类表示“整体-部分”的关联关系，“整 体”负责“部分”的生命周期，他们之间是共生共死的；并且“部分”单独存在时没有任何意义。

在下图的例子中，People与Soul、Body之间是组合关系，当人的生命周期开始时，必须同时有灵 魂和肉体；当人的生命周期结束时，灵魂肉体随之消亡；无论是灵魂还是肉体，都不能单独存在，他们 必须作为人的组成部分存在。
3.实现

一个类中包含了另一个类的对象作为属性成员，即在类中含有类类型的数据成员 。

```cpp
class Soul {};
class Body {};
class People
{
	Soul _soul;
	Body _body;
	//组合关系中的成员变量一般会在构造方法中赋值
public:
	People(Soul soul, Body body) :_soul(soul), _body(_body)
	{
	}
public:
	void study() {
		cout << " 学习要用灵魂" << soul.getName() << endl;
	}
	void eat() {
		cout << "吃饭用身体：" << body.getName() << endl;
	}
};
```



## 4.组合(Composition )

### 1.图示

<img src="https://img-blog.csdnimg.cn/b0a0f021956a4fc0b12dfa66459531e2.png" style="zoom:67%;" />

### 2.定义

当对象A是对象B的属性时,称对象B包含对象A。意味着 "用...来实现,用...来构造"

相比于聚合，组合是一种耦合度更强的关联关系。存在组合关系的类表示“整体-部分”的关联关系，“整 体”负责“部分”的生命周期，他们之间是共生共死的；并且“部分”单独存在时没有任何意义。

在下图的例子中，People与Soul、Body之间是组合关系，当人的生命周期开始时，必须同时有灵 魂和肉体；当人的生命周期结束时，灵魂肉体随之消亡；无论是灵魂还是肉体，都不能单独存在，他们 必须作为人的组成部分存在。

### 3.实现

一个类中包含了另一个类的对象作为属性成员，即在类中含有类类型的数据成员

```cpp
class Soul{};
class Body{};
class People
{
Soul _soul;
Body _body;
//组合关系中的成员变量一般会在构造方法中赋值
public:
People(Soul soul, Body body):_soul(soul),_body(_body)
{
}
public:
void study(){
cout<<" 学习要用灵魂"<<soul.getName()<<endl;
}
void eat(){
cout<<"吃饭用身体："<<body.getName()<<endl;
}
};
```

### 4.类型的组合

对象线段是用两个点对象来实现: 点与线的组合

```cpp
class Point // 点
{
private:
	float _x;
	float _y;
protected:
	Point() :_x(0.0), _y(0.0) {}
	Point(float x, float y) :_x(x), _y(y) {}
	Point(const Point&) = default; // 按位拷贝 inline 函数
public:
	Point& operator=(const Point&) = default; // 按位赋值 inline 函数
	~Point() {}
	float& pointX() { return _x; }
	const float& pointX() const { return _x; }
	float& pointY() { return _y; }
	const float& pointY() const { return _y; }
};
class Line // 线
{
private:
	Point _start;
	Point _end;
	float _length;
	float distance()
	{
		float xd = _start.pointX() - _end.pointX();
		float yd = _start.pointY() - _end.pointY();
		_length = sqrt(xd * dx + yd * yd);
	}
public:
	Line() :_length(0) {}
	Line(const Point& a, const Point& b) :_start(a), _end(b)
	{
		distance();
	}
	Line(const Line&) = default;
	Line& operator=(const Line&) = default;
	~Line() {}
	float length() const
	{
		return _length;
	}
	const Point& get_Start()const { return _start; }
	const Point& get_End() const { return _end; }
	void set_Start(const Point& a)
	{
		_start = s;
		distance();
	}
	void set_End(const Point& b)
	{
		_end = b;
		distance();
	}
};
int main()
{
	Point a(1, 2), b(5, 5);
	Line s(a, b);
	cout << s.length() << endl;
	return 0;
}
```

发一个可应用于多个软件的文件加密模块，该模块可以对文件中的数据进行加密并将加密之后的数 据存储在一个新文件中，具体的流程包括三个部分，分别是读取源文件、加密、保存加密之后的文件， 其中，读取文件和保存文件使用流来实现，加密操作通过求模运算实现。这三个操作相对独立，为了实 现代码的独立重用，让设计更符合单一职责原则，这三个操作的业务代码封装在三个不同的类中。 

## 5.继承(Inheritance)

### 1.图示

<img src="https://img-blog.csdnimg.cn/5b88f2fa72b34241add6ec4c86b5b57d.png" style="zoom:50%;" />

## 6.类模板(Class template)

### 1.图示

<figure>
<img src="https://img-blog.csdnimg.cn/37d6f3e8182b4cc4855415d864593ecf.png" height=400/>
<img src="https://img-blog.csdnimg.cn/b9d17969eb1546cc823fdb8b08715cd1.png" height=200/>
</figure>


