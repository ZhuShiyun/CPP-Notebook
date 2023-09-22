# C++中setiosflags( ) 的用法

```cpp
#include<iomanip> // 头文件
setiosflags( ios::fixed ) // 基础格式
```

在遇到要**计算浮点数**且希望能**控制其输出**、**精度**、**小数点后的位数**等时，用`setiosflags( ios::fixed )`来控制。

## 1. setprecision( )

使用`setprecision(n)`可控制输出流显示浮点数的数字个数。C++默认的流输出数值有效位是6。`setprecision(n)`就是输出n个数，会有四舍五入。

```cpp
#include<iostream>
#include<iomanip>
#include<cmath>

using namespace std;
int main() {
    double s=20.7843000;

    cout << s << endl;
    cout << "setprecision( 1 )"<< setprecision( 1 )<< s << endl;
    cout << "setprecision( 2 )"<< setprecision( 2 )<< s << endl;
    cout << "setprecision( 3 )"<< setprecision( 3 )<< s << endl;
    cout << "setprecision( 4 )"<< setprecision( 4 )<< s << endl;
    cout << "setprecision( 5 )"<< setprecision( 5 )<< s << endl;
    cout << "setprecision( 6 )"<< setprecision( 6 )<< s << endl;
    cout << "setprecision( 7 )"<< setprecision( 7 )<< s << endl;
    cout << "setprecision( 8 )"<< setprecision( 8 )<< s << endl;

return 0;

}
```

Output:

```c++
20.7843
setprecision( 1 )2e+01
setprecision( 2 )21
setprecision( 3 )20.8
setprecision( 4 )20.78
setprecision( 5 )20.784
setprecision( 6 )20.7843
setprecision( 7 )20.7843
setprecision( 8 )20.7843
```

可见，小数部分末尾为0时，是无法输出的。

## 2. setiosflags(ios::fixed)

`setprecision(n)`与`setiosflags(ios::fixed)`合用，可以控制小数点右边的数字个数。

```cpp
#include<iostream>
#include<iomanip>
#include<cmath>

using namespace std;
int main() {

    double s=20.7843000;
    cout << s << endl;
    cout << "setprecision( 1 )"<< setprecision( 1 )<< s << endl;
    cout << "setprecision( 2 )"<< setprecision( 2 )<< s << endl;
    cout << "setprecision( 3 )"<< setprecision( 3 )<< s << endl;
    cout << "setprecision( 4 )"<< setprecision( 4 )<< s << endl;
    cout << "setprecision( 5 )"<< setprecision( 5 )<< s << endl;
    cout << "setprecision( 6 )"<< setprecision( 6 )<< s << endl;
    cout << setiosflags( ios::fixed );  // 之后的输出会被控制小数点右边数字的个数。
    cout << "setprecision( 7 )"<< setprecision( 7 )<< s << endl;
    cout << "setprecision( 8 )"<< setprecision( 8 )<< s << endl;

return 0;

}
```

Output:

```
20.7843
setprecision( 1 )2e+01
setprecision( 2 )21
setprecision( 3 )20.8
setprecision( 4 )20.78
setprecision( 5 )20.784
setprecision( 6 )20.7843
setprecision( 7 )20.7843000
setprecision( 8 )20.78430000
```

## 3. setiosflags(ios::fixed|ios::showpoint)

当`setprecision()`的精度为0的时候，有`showpoint`，结果就会显示小数点，没`showpoint`,就不显示小数点。

```c++
#include<iostream>
#include<iomanip>
#include<cmath>

using namespace std;
int main() {

    double s=20.7843000;

    cout << s << endl;
    cout << setiosflags( ios::fixed);
    cout << "setprecision( 0 )结果不显示小数点："<< setprecision( 0 )<< s << endl;
    cout << "setprecision( 1 )"<< setprecision( 1 )<< s << endl;
    cout << "setprecision( 2 )"<< setprecision( 2 )<< s << endl;
    cout << "setprecision( 3 )"<< setprecision( 3 )<< s << endl;
    cout << setiosflags( ios::fixed|ios::showpoint );
    cout << "setprecision( 0 )结果显示小数点："<< setprecision( 0 )<< s << endl;
    cout << "setprecision( 1 )"<< setprecision( 1 )<< s << endl;
    cout << "setprecision( 2 )"<< setprecision( 2 )<< s << endl;
    cout << "setprecision( 3 )"<< setprecision( 3 )<< s << endl;

return 0;

}
```

Output:

```
20.7843
setprecision( 0 )结果不显示小数点：21
setprecision( 1 )20.8
setprecision( 2 )20.78
setprecision( 3 )20.784
setprecision( 0 )结果显示小数点：21.  
setprecision( 1 )20.8
setprecision( 2 )20.78
setprecision( 3 )20.784
```

