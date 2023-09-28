# 关于 第五章 C++ 程序组织与开发方法/随机数库的实现 中 GenerateRandomNumber() 实现 的解释（看了可以放弃思考版）

先看代码：

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

本笔记将以QA形式详细解释以下代码的逻辑：

大多知识点过于基础，丢在这里仅供理解的连贯性以及温故知新。

```c++
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
```

1. `_d = (double)rand() / ((double)RAND_MAX + 1.0);`: 这一行代码用于生成一个介于0和1之间的随机浮点数。`rand()` 函数用于生成一个0到 `RAND_MAX` 之间的整数，然后将其转换为浮点数并除以 `RAND_MAX + 1.0`，从而得到一个0到1之间的浮点数。

4. `return min + (int)(_d * (max - min + 1));`: 这一行代码将上一步生成的随机浮点数 `_d` **映射**到指定范围 `[min, max]` 内的整数。它首先将 `_d` 乘以 `(max - min + 1)`，这样 `_d` 就变成了一个介于0和 `max - min + 1` 之间的浮点数。然后，将结果转换为整数，并加上 `min`，以确保最终生成的整数在指定的范围内。

**Q: 细说映射**

**A**：在代码中，映射指的是将一个数域（例如0到1之间的浮点数）映射到另一个数域（例如`min`到`max`之间的整数）。在这个特定的代码段中，我们希望将生成的随机浮点数 `_d` 映射到指定范围 `[min, max]` 内的整数。

这是如何进行映射的过程：

1. `_d` 是一个介于0和1之间的随机浮点数。例如，它可以是0.345。

2. `(max - min + 1)` 是指定整数范围 `[min, max]` 内整数的数量。如果 `min` 是5，`max` 是10，那么这个数量就是6（包括5和10在内）。

3. `_d * (max - min + 1)` 将 `_d` 乘以整数范围内的整数数量，这将使 `_d` 的范围变为0到6之间的浮点数。在我们的例子中，这意味着 `_d` 的范围可能是0到6之间的某个浮点数，例如2.07。

4. `(int)(_d * (max - min + 1))` 将上一步计算的浮点数强制转换为整数。这将丢弃小数部分，保留整数部分。在我们的例子中，如果计算结果是2.07，那么强制转换后就会得到整数2。

5. 最后，将上述整数结果加上 `min`，这是因为我们希望生成的随机整数在 `[min, max]` 范围内。所以如果 `min` 是5，之前计算的结果是2，那么最终生成的随机整数就是5 + 2 = 7。

这个映射的过程确保了生成的随机整数在指定的范围内，并且保持了均匀的分布，因为 `_d` 在0到1之间的浮点数是均匀分布的。这就是代码中如何映射随机浮点数到整数范围的过程。

**Q： RAND_MAX 是怎么来的呢？它是什么**

**A**：`RAND_MAX` 是一个常量，通常定义在C或C++的标准库中，代表了随机数生成器 `rand()` 函数返回的最大可能值。

具体来说，`RAND_MAX` 的值是与系统和编译器有关的。它表示 `rand()` 函数可以返回的最大整数值。通常情况下，`RAND_MAX` 是一个较大的正整数，例如32767。

在使用 `rand()` 函数生成随机数时，通常会将其结果除以 `RAND_MAX`，以获得0到1之间的浮点数，然后再进行其他计算以获得所需范围内的随机数。这是因为 `rand()` 函数生成的随机整数在0到 `RAND_MAX` 之间，通过除以 `RAND_MAX`，可以将其映射到0到1之间的浮点数。

总之，**`RAND_MAX` 是一个编程语言标准库中定义的常量，代表了 `rand()` 函数返回的最大整数值，用于在生成随机数时进行归一化。**

**Q： 为什么是(max - min + 1）而不是max-min**

**A：**在这段代码中，使用 `(max - min + 1)` 而不是简单的 `max - min` 是为了确保生成的随机整数包括 `max` 在内。

让我们来考虑一个具体的例子，假设你希望生成一个范围在3到6之间的随机整数。如果你使用 `max - min`，也就是 `6 - 3 = 3`，然后将其乘以生成的随机浮点数，你会得到一个范围在0到3之间的随机浮点数。

然后，你将这个随机浮点数转换为整数，得到的整数范围是0到3。如果你再加上 `min`，也就是3，你最终得到的随机整数范围是3到6。然而，这个范围不包括6，**因为生成的随机浮点数永远不会达到1，只会接近1。**

通过使用 `(max - min + 1)`，你实际上将整数范围扩展到了4，也就是从0到3变成了0到4。这样，当你将随机浮点数转换为整数并加上 `min` 时，你得到的范围就是3到6，包括6在内。这确保了生成的随机整数范围是 `[min, max]`，而不是 `[min, max)`，也就是包含了 `max`。