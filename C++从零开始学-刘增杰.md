---
ebook:
  title: C++从零开始学-刘增杰
  authors: zhoulanlan
---
# C++从零开始学-刘增杰

# 第一章 认识C++
## 1.2 C++的特色
- [ ] C++中结构可以作为一种特殊的类，拥有函数，但是没有私有和保护的成员。
- [ ] C++中包含私有、公有和保护成员（Java中有私有、公有、包内以及保护）。
- [ ] C++中通过消息处理对象。
- [ ] C++友元函数可以访问封装类中私有成员。
- [ ] C++可以定义虚函数来支持动态联编。
- [ ] C++核心语言和标准库（STL）。
- [ ] STL提供容器、迭代器以及算法（搜索和排序）。

## 1.3 关于ANSI/ISO C++标准
 C++标准库分类|说明
 --|--
语言支持（C1）|例如`<cstddef>`定义宏NULL和offsetof，以及其他标准类型size_t和ptrdiff_t。
输入/输出(C2)|例如 `<iostream>`支持cin、cout、cerr和clog。
诊断(C3)|例如`<stdexcept>`
一般工具(C4)|例如`<memory>`
字符串(C5)|例如`<string>`
容器(C6)|例如`<list>`
迭代器支持(C7)|例如`<iterator>`
算法支持(C8)|例如`<algorithm>`,包括置换、排序、合并和搜索。
数值操作(C9)|`<complex>`，支持复杂数值的定义和操作。
本地化(C10)|`<local>`，提供本地化包括字符类别、排序序列以及货币和日期表示。

## 1.4 C和C++语言编译过程
- [ ] **模板**处理在**连接**之前。
- [ ] 编译过程。
    - [ ] `预编译`：宏展开，`ifdef`和`ifndef`中挑选符合条件的代码，把对应文件的内容替换到`#include`语句处。
    - [ ]  `编译`：`.h`文件不理会，`.cpp`文件被编译。编译只负责单独的单元，可以调用函数不理会定义，但必须获得函数的申明。
    - [ ]  `链接`：把编译好的单元链接成一个文件，检查是否出现**重复**和**缺失**的定义。

## 1.5 编写代码前的准备-安装开发环境
- [ ] 唯一适合学习C++的编译器是**GCC**/**MinGW**。

# 第二章 C++程序结构
## 2.2 C++程序分析
- [ ] `include` 是C/C++的预处理指令，表示包含编译器能识别的C/C++代码文件，包括`.c`、`.hpp`、`.cpp`、`.hxx`、`.cxx`甚至`.txt`和`.abc`等。
- [ ] 头文件三部分：开头**版权和版本申明**，其次是**预处理块**，最后是**函数和类结构的声明**
- [ ] `main`函数必须有返回值int。
- [ ] `int main(argc, char** argv){}`
- [ ] `main()`函数本身已索引0为第一参数，argv至少为1，从argv[1]开始计数。
- [ ] 使用`extern`关键字可以申明变量名而不进行定义。
- [ ] 所有以**两个下划线开头**的名字，以及**一个下划线加上一个大写字母开头**的名字，不要使用，为标准库预留。
- [ ] 所有**以一个下划线开头并且第二个字符不是下划线也不是大写字母**（如`_range`）的变量不要命名在全局名字空间，会导致较差的可移植性。
- [ ] **空函数**预留未来的实现功能。
- [ ] 好的注释是在更抽象层次上进行解释，而不是简单的对源代码进行翻译。

## 2.3 输入输出对象
- [ ] cout和cin等是C++在库中预先定义好的流对象。
- [ ] 定义**流对象**会一般定义**缓冲区**，在缓冲区满了或者遇到`endl、\n、ends、flush`时才会对**缓冲区**中的数据进行输出。
- [ ] 如果要使用控制符，得包含`<iomanip>`头文件
```c++
#include <iostream>
#include <iomanip>

using namespace std;

int main(int argc, char **args) {


    double a = 123.456789012345;
    cout << a << endl;//默认精度为6，输出123.457，自动四舍五入
    cout << setprecision(9) << a << endl;//设置精度为9，输出123.456789，自动四舍五入

    cout << setprecision(6);//重新设置为默认精度
    cout << setiosflags(ios::fixed) << a << endl;//设定精度格式为小数点后几位


    return 0;
}
```

## 2.4 标识符
- [ ] 标识符用于定义资源。
- [ ] 超过32个字符，以后的字符忽略不计。

## 2.5 预处理
- [ ] 常用预处理命令
    - [ ] `#include`:包含头文件。
    - [ ] `#if`:条件。
    - [ ] `#else`:否则。
    - [ ] `#elif`:否则如果。
    - [ ] `#endif`:结束条件。
    - [ ] `#ifdef`:如果定义了一个符号，就执行。
    - [ ] `#ifndef`:如果没有定义一个符号，就执行。
    - [ ] `#define`:定义一个符号。
    - [ ] `#undef`:删除一个定义符号。
    - [ ] `#line`:重新定义当前行号和文件名。
    - [ ] `#error`:输出编译错误并停止编译。
    - [ ] `#pragma`:提供专用的特性，并且保证与C++的完全兼容性。
- [ ] **头文件**中一般包含**声明和全局变量**。
- [ ] `<>`编译器只搜索包含标准库头文件的默认目录。
- [ ] `""`编译器优先搜索编译源文件所在目录，再搜索包含标准库头文件的默认目录。**(如果头文件在其他目录中，必须指定完整路径)**。
- [ ] C++中最好使用`const`定义常量，它指定了类型。
- [ ] `#define`的缺点：1.无类型检查。2.不考虑作用域。3.符号名不能限制在一个命名空间中。
- [ ] 带参数的宏相当于一个函数的功能，如：`#define PRINT(a) cout<<(a)<<endl`。
- [ ] 使用**内联函数**代替宏，可以**增强类型检查**。

## 2.6 命名空间
- [ ] 命名空间是一种**逻辑分组**机制，将在**逻辑上**属于同一个集团的声明放在一起。
- [ ] 命名空间可以是**全局**的，也可以位于**另一个命名空间**之中，但是**不能**位于类和代码块中。
- [ ] 在命名空间中声明的**标识符**，除非引用了常量，默认具有**外部链接性**。
- [ ] **命名空间**代码实例：
```C++
#include <iostream>
#include <iomanip>
using namespace std;

namespace myName1{
    string user_name = "Myname1";
}
namespace myName2{
    string user_name = "Myname2";
}

int main(int argc, char **args) {

    cout<<myName1::user_name<<endl;//输出Myname1
    cout<<myName2::user_name<<endl;//输出Myname2

    return 0;
}
```
- [ ] `std`:C++中的所有标识符都包含在该命名空间中。
- [ ] **向后兼容**是指向**旧**的兼容。
- [ ] C++中新的头文件引用方式，无`.h`，C语言标准库中**以c开头**，如`<cstdio>`。

# 第三章 基本数据类型
## 3.1 常量与变量
- [ ] `typeid`取得**变量数据类型**
- [ ] `sizeof`取得**变量大小**
```C++
#include <iostream>
#include <iomanip>

using namespace std;


int main(int argc, char **args) {

    double a;
    cout << typeid(a).name() << endl;//输出i
    cout << typeid(double).name() << endl;//输出i
    cout << sizeof(a) << endl << sizeof(int) << endl;//输出8 4

    return 0;
}
```
- [ ] `const`变量必须在**定义的时候赋值**。（**宏**在编译时起作用`const`在运行时起作用）
- [ ] **八进制**以`0`开头，如：`010,-0536`。
- [ ] **指数形式**：数字+E(e)+**整数**。

## 3.3 typedef
- [ ] `cout << true << endl;//输出1`
- [ ] `char* pa,pb`，只定义了`pa`一个指针。
```C++
typedef char *PCHAR;
PCHAR pa,pb;//同时定义了两个指向字符变量的指针。
```
- [ ] C++中可以直接定义**"结构名 对象名"**定义结构体对象，C语言中不可以。

## 3.5 疑难解惑
- [ ] `FALSE`/`false`不同，`false`/`true`是C++标准新增加的关键字，`FALSE`/`TRUE`是通过`#define`定义的宏，为了和C语言兼容。

# 第四章 运算符和表达式

## 4.1 运算符概述
- [ ] `<<`左移操作，移出位被丢弃，右边空位补0.
- [ ] `>>`右移操作，移出位被丢弃，左边空位或者补0，或者补符号位（由不同的机器决定）。
- [ ] `,`运算符，将两个表达式进行连接，具有最低的优先级。（`for`循环中）

## 4.4 疑难解惑
- [ ] 掩码用`&0011`：隐藏前面部分，将前面部分全部置为0.
- [ ] 打开位用`|0011`：将后面部分置为1.分
- [ ] 转置位用`^0011`。将后面的部分进行**转置**，0变为1，1变为0.

# 第五章 程序流程控制

# 第六章 函数
## 6.1 函数的基本结构
- [ ] 声明是给**编译器**使用的，定义会被**连接器**使用。
- [ ] 函数名称着重说明**要做什么**。
- [ ] **形参**只在函数被调用时分配内存单元，调用结束就被释放。
- [ ] **引用类型**传递，相当于地址传递。`int test(int &a);`
- [ ] **默认参数**，**在函数声明时**设定。

## 变量的作用域
- [ ] 局部变量默认使用`auto`关键字来修饰。
- [ ] 在局部变量前面加上`static`关键字则为静态局部变量，，静态局部变量不存储在堆栈中，有其固定地址。
- [ ] `extern`告诉编译器存在一个**变量**或者**函数**，即使编译器在当前的文件中没有看见它，这个**变量**或**函数**可能在**某个文件**或者**当前文件后面**定义了。
- [ ] `rigester`声明寄存器变量，如果系统的寄存器被其他数据占用，则**寄存器变量变为局部变量**。**（不推荐使用）**
- [ ] 不可能得到**寄存器变量**的地址。
- [ ] **寄存器变量**不能作为全局和静态变量，可以在**形参**中使用。

## 6.4 内联函数
- [ ] 编译器将**内联函数**的表达式替换为**内联函数整体**。
- [ ] 内联函数不允许使用**循环语句**和**switch**。
```C++
#include<iostream>
#include<string>

using namespace std;
inline string dbtest(int a);//函数声明为内联函数

int main()
{

	for (int i = 0; i <= 10; i++) 
	{
		cout << i << ":" << dbtest(i) << endl;
	}
    return 0;
}

string dbtest(int a) //这里不用再次使用inline，再次使用也不会有错
{
	return a % 2 > 0 ? "奇" : "偶";
}
```

## 6.5 预处理器
- [ ] 宏定义中，标识符**大写**，使用**下划线**替代空格。
- [ ] 宏定义给硬编码数字具体的意义。
- [ ] 宏定义使得数据易于修改。
- [ ] 编译器通常不为普通的`const`常亮分配存储空间，而是将它们保存到**符号表**中。

## 6.6 函数重载
- [ ]  重载函数减少函数名数量，避免名字空间的污染。
- [ ]  重载函数功能相同或者相近，这样有利于理解。
- [ ]  必须考虑参数的默认值对函数重载的影响。
- [ ]  参数默认值、函数重载、模板。

## 6.8 疑难解惑
- [ ] 在一个文件中定义的内联函数**不能**在另一个文件中使用，通常放在头文件中。
- [ ] 内联函数中不能有**循环语句**、**if语句**或者**switch**语句，否则即使有`inline`关键字，编译器也会把该函数当做**非内联函数**处理。

# 第七章 数组与字符串
## 7.1  一维数组
- [ ] 数组的初始化赋值：`类型说明符 数组名[常量表达式] = {值，值...值};`
- [ ] 若数组定义个数省略，编译器根据初值个数来确定数组元素的个数。
- [ ] 数组初始化赋值在编译阶段进行，将减少运行时间。

## 7.2 数组与函数
- [ ] 数组变量本身就是**内存地址**，函数中固定为**传址**。

## 7.3 字符串类

- [ ] `string`类包含在命名空间`std`中。
- [ ] `string`类的三个构造函数:
```C++
string str; //默认构造函数，建立空串
string str("OK"); //采用C的字符串进行初始化
string str(str1); //调用拷贝构造函数
```
- [ ] 通过`[]`访问单个字符时**不会**进行范围检查，而通过`at()`会进行范围检查。
- [ ] `char c[] = "China"; cout<<c;`输出字符时遇到`'\0'`时停止。
- [ ] `cin.getline(字符数组名St,字符个数N,结束符);`一次读入**N**个字符（**可以包含空格**），直到遇到**结束符**停止（**默认结束符为`\n`**）。
- [ ] `char *strcpy(char *dest,char *source)`字符串拷贝函数。
- [ ] `char *strcat(char *dest,char *source)`在**dest**串后面贴上**source**。
- [ ] `int strcmp(char *s1,char *s2)`比较两个字符串，相等返回0。
- [ ] `strlwr(字符串)`将字符串中的**大写字母**转换为**小写字母**。
- [ ] `strupr(字符串)`将字符串中的**小写字母**转化为**大写字母**。
- [ ] `string`类的操作：
```C++
str.substr(pos,length1); //从pos位置起，长length1个字符。
str.empty(); //检查是否是空串。
str.insert(pos,str2); //将str2插入str的pos位置处。
str.remove(pos.length1);//从pos起，删除长度为length1的字串
str.find(str1);//返回str1首次在str中出现的索引。
str.find(str1.pos);//返回从pos处起，str1首次在str中出现时的索引。
str.length();//返回串的长度。
str.c_str();//将string类转换为C风格字符串，返回char*。
```

## 7.6 疑难解惑
- [ ] `memset(str,0,sizeof(str));`快速的为数组**按位**置0；
- [ ] `memset(str,-1,sizeof(str));`快速的为数组**按位**置1；
- [ ] `char* itoa(int value,char *string,int radix);`**value**:待转换的整数。**radix**:范围2-36，如10表示10进制，16表示16进制。**string**:保存转换后得到的字符串。**返回值char***:指向生成的字符串

# 第八章 指针
## 8.1 指针概述
- [ ] 用于表示各种**数据结构**。
- [ ] 使**主调函数**和**被调函数**之间共享变量和数据结构。

## 8.2 指针变量
- [ ] 指针变量用于指定一个变量在**内存中的首地址**。

## 8.3 指针与函数
- [ ] 返回值是指针的函数叫做**指针函数**。
- [ ] **函数指针**是指向函数的指针，指向的是**存放函数的地址**。定义：`数据类型 (*函数指针名)(参数表);`
- [ ] **函数指针**代表了**函数代码的首地址**，在对函数指针进行**初始化时**，直接将**函数名**赋值给**函数指针**即可。
```C++
#include <iostream>
using namespace std;

int fun(int a) {
    return a;
}

int main(int arg, char** args) {

    cout << fun << endl;//输出函数fun的地址,用远返回true
    int (*fp)(int a);
    fp = fun;
    cout << fp(5) << endl;//调用方式1
    cout << (*fp)(10) << endl;//调用方式2

    return 1;
}
```
## 8.4 指针与数组
- [ ] 对指针变量的加1或者减1，其实是加上或者减去**指针所指向数据类型的大小**。
- [ ] 两个指针**相加无意义**，两个指针**相减**得到的是地址之间**变量个数**。
- [ ] `int[] a;`**a**与**(&a[0])[1]**意义相同。
- [ ] 有趣的例子：
```C++
#include <iostream>
using namespace std;
int main(int arg, char **args) {

    const char *p = "www.sohu.com";
    while (*(++p));
    while (*(--p))
        cout << *p;
    cout << endl;
    //输出moc.uhos.www
    return 1;
}
```
- [ ] `const char *p;`**p**指向的**内容不可以改变**，指针**p**可以改变(**p++不会报错**)。
- [ ] `char* const p;`**p**指向的**内容可以改变**，指针**p**不可以改变(**p++会报错**)。相当于`char p[]`。

## 8.6 void指针
- [ ] `void*`为无类型指针，可以指向**任何数据**。主要用在**函数返回**和**函数参数**。

## 8.7 指向指针的指针
- [ ] **指针数组**：数组中存放的是指针。

## 8.8 动态内存配置
- [ ] 四个内存区域：代码区、全局数据区、栈区和堆区。
- [ ] 基本数据类型也可以分配到堆中。分配`指针变量名 = new 类型名(初始值);`或`指针变量名 = new 类型名;`，释放`delete 指针名`。
- [ ] 数组分配`指针变量名 = new 类型名[数组大小];`，释放：`delete[] 指针名`。
- [ ] `int atio(字符串);`将字符串转化为整数。

## 8.10 疑难解惑
- [ ] **数组指针**是指向数组的一个指针。**指针数组**中的每个元素都相当于一个指针变量。
- [ ] 一维**指针数组**定义：`类型名 *数组名[数组长度]`。例子：
```C++
#include <iostream>
using namespace std;

int main(int arg, char **args) {

    int *p[2];//指针数组，数组的元素全是指针
    int a[] = {1, 2, 3};
    int b[] = {3, 4, 5};
    p[0] = a;
    p[1] = b;

    for (int i = 0; i < 2; ++i) {
        for (int j = 0; j < 3; ++j) {
            cout << p[i][j] << " ";
        }
    }
    //输出结果：1 2 3 3 4 5
    return 1;
}
```
- [ ] 动态分配失败，返回`NULL`，表示发生了异常，比如**堆资源不足**。
- [ ] **堆空间**只能释放一次。

# 第九章 struct和其他复合类型
- [ ] 结构体定义后并不分配空间，定义**结构体变量**后才分配空间。
- [ ] 结构体定义方式：
```C++
struct 结构体名
{
    成员列表
}变量名列表;

//或者省略结构体名
struct
{
    成员变量
}变量名列表;
```
- [ ] 结构体变量初始化，**用花括号把每个成员的值括起来**。
```C++
person p = {102,"Zhang ping",'M',78.5};//结构体初始化
```
- [ ] 跳过前面的对后面的成员赋值，将会产生**编译错误**。只赋值前面的成员，后面的成员将会被**初始化为0**。
- [ ] 结构体变量之间可以相互赋值。
- [ ] 结构体的**不能**进行整体的输入和输出。
```C++
#include <iostream>
using namespace std;

int main(int arg, char **args) {

    struct person {
        int id;
        int age;
    };

    person person1 = {101, 18};//结构体变量初始化
    person person2;//定义结构体变量，分配空间
    person2 = person1;//结构体变量之间可以相互赋值
    cout << person2.id << " " << person2.age << endl;//不能整体输出，只能单个输出
    //cout<<person2;//不能整体输入输出

    //结构体变量的初始化
    person persons[3] = {
            {101,18},
            {102,19},
            {103,20}
    };
    return 1;
}
```

## 9.2 将结构体变量作为函数参数
- [ ] 把结构体传入函数中，其传递方式分为**传值**、**传址**和**传引用**。


## 9.3 union
- [ ] 对**不同的数据类型**使用**共同的存储空间**。
- [ ] 运行过程中只有一个成员处于活动状态。
- [ ] 定义方式：
```C++
union 共用体名
{
    类型名 共同体成员名;
};
```
- [ ] 共用体能够访问的是**最后一次**被赋值的成员，对一个新成员赋值之后原有的成员就**失去作用**。
```C++
#include <iostream>
using namespace std;

int main(int arg, char **args) {

    union Test {
        int a;
        int b;
        char c;
    };
    Test a;
    a.a = 1;
    a.b = 10;
    cout << a.a << endl;//输出10
    return 1;
}
```
- [ ] `union`占用的空间等于**最大成员占用的空间**。

## 9.4 enum
- [ ] 枚举定义如下：
```C++
enum 枚举名
{
    枚举值列表
};

enum 枚举名
{
    枚举值列表
}变量列表;

enum
{
    枚举值列表
}变量列表;
```
- [ ] 枚举作为**常量**，编译器默认赋值**0,1,2,3...**
- [ ] 枚举初始化赋值：
```C++
#include <iostream>
using namespace std;

int main(int arg, char **args) {

    enum City {
        BeiJing,
        ShangHai,
        TianJing = 1,
        YunNan = 9
    };

    cout << BeiJing << endl;//0
    cout << ShangHai << endl;//1
    cout << TianJing << endl;//1
    cout << YunNan << endl;//9
    City city = (City) 18;
    cout << city << endl;//18

    return 1;
}
```

## 9.6 疑难解惑
- [ ] C++的结构体中可以有**成员函数**。
- [ ] 编译时，不为**结构体类型**分配空间，只为**结构体变量**分配空间。

# 第10章 类
## 10.1 认识类
- [ ] **类**是**数据**和定义在这些数据上的**操作**的一个**封装集合**。
- [ ] 类的定义分为**说明部分**和**操作部分**。
- [ ] 类的一般定义格式：
```C++
class <类名>
{
    public: //访问修饰符，允许多次出现。
    <成员函数和数据成员说明>
    private:
    <成员函数和数据成员说明>
};
<各个成员函数的实现>
```
- [ ] 成员函数可以定义到**类体内**和**类体外**。
- [ ] 类是抽象的，不占用内存，对象是具体的，占用内存。
```C++
#include <iostream>
using namespace std;
//类声明
class Clock {
public:
    void setTime(int newH, int newM, int newS);

    void showTime();

private:
    int hour, minute, second;
};

//类定义
void Clock::setTime(int newH, int newM, int newS) {
    hour = newH;
    minute = newM;
    second = newS;
}

void Clock::showTime() {
    cout << hour << ":" << minute << ":" << second << endl;
}

int main(int arg, char **args) {

    Clock myClock;
    myClock.setTime(20, 40, 20);
    myClock.showTime();//输出：20:40:20

    //类对象指针调用
    Clock* pMyClock;
    pMyClock = &myClock;
    pMyClock->showTime();//输出：20:40:20
    return 1;
}
```

## 10.2 成员函数
- [ ] 类的**内联函数**，方法一：在类中声明的时候定义函数（**隐式定义**），方法二：在外部定义时加上`inline`关键字（**显示定义**）。
- [ ] `::`作用域限定符。
- [ ] 如果在`::`前无类名，则表示该函数是全局函数。如：`void :: display()`


## 10.3 嵌套类
- [ ] **嵌套类**可以作为外部类的底层实现，同时具有**隐藏**底层实现的作用。
- [ ] **嵌套类**和**外部类**没有相互关联关系（**和Java有区别**），相互访问**遵循两个普通类**的访问规则。
- [ ] 嵌套类成员或者在**嵌套类内部**进行定义，或者在**外部类相同作用域中**进行定义。
- [ ] 可以在**另一个头文件中定义嵌套类**，只在外围类中**前向声明**这个嵌套类，达到**隐藏实现**的效果。

## 10.4 const成员函数
- [ ] **定义和声明成员函数**的时候，使用`const`修饰，表示该函数不能改变类的数据成员，如果企图改变，**编译器会报错**。如：`void showTime() const;`
- [ ] `const`函数不能调用非`const`函数。

## 10.6 静态成员
- [ ] 静态成员的目的是为了实现**对个类对象之间数据的共享**。
- [ ] 静态成员的使用：`类名称::成员名称`。
- [ ] 声明是在前面加关键字`static`。
- [ ] 初始化`<数据类型> <类名>::<静态数据成员名>=<值>`
- [ ] 静态成员必须在**类体外**进行**初始化**。
- [ ] 引用格式为`<类名>::<静态成员名>`
- [ ] 调用静态函数格式为`<类名>::<静态成员函数名>(<参数表>)`
```C++
#include <iostream>
using namespace std;

class TestStaticMember {
public:
    static int num;//声明静态成员数据
    static int getNum();//声明静态函数
};

int TestStaticMember::num = 10;//初始化静态变量

//定义静态函数
int TestStaticMember::getNum() {
    return num;
}

int main(int arg, char **args) {

    cout << TestStaticMember::num << endl;//获取静态成员数据
    cout << TestStaticMember::getNum() << endl;//调用静态函数

    return 1;
}
```

## 10.7 友元
- [ ] `友元函数`避免调用类的成员函数产生开销。
- [ ] `友元关闭`是单向的，不具有`交换性`和`传递性`。
- [ ] 在类中声明一个函数时在前面加上`friend`关键字，友元函数不是成员函数，不用作用域限定符`::`来定义。
```C++
#include <iostream>
using namespace std;

class TestFriend {
private:
    int a = 1;
    friend int getA(TestFriend &testFriend);//必须要有一个传入对象的参数，可以为public或者private
};

//普通函数的定义方式，不用作用域限定符::
int getA(TestFriend &testFriend) {
    return testFriend.a;
}

int main(int arg, char **args) {

    TestFriend testFriend;
    cout<<getA(testFriend);//调用友元函数，不用对象名

    return 1;
}
```

## 10.9 疑难解惑
- [ ] **自身类的对象**不能作为自身类的成员，但是**自身类的引用和指针**却可以。
- [ ] 最好先声明**公有成员**，再声明**私有成员**。
- [ ] 一般按数据成员的大小，**由小到大**声明，这样可以提高**空间利用率**。
- [ ] 习惯性的将类定义的**声明部分**或者**整个定义部分（包括实现部分）**放入一个头文件。
- [ ] **轻量级对象**如点、颜色、矩形等用结构。
- [ ] **抽象和多级别**对象时，用类。
- [ ] 大多数情况，如果**只有数据**，结构是最好的选择。
- [ ] `mutable`关键字修饰的数据成员可以被`const成员函数`修改。

# 第十一章 构造函数和析构函数
## 11.1 构造函数初始化类对象
- [ ] 构造函数不需要也不能够被用户调用。

## 11.2 析构函数清除类对象
- [ ] 析构函数**没有**函数参数，一个类**只有一个**析构函数。
- [ ] 释放对象的时机：
    - [ ] **new对象**被`delete`时。
    - [ ] **自动对象**超出其作用域时，或者程序结束时。
    - [ ] 使用**完全限定名**显示调用对象的析构函数。
    - [ ] 析构函数不能定义为`const`、`volatile`或`static`。
```C++
class <类名>
{
public:
    ~<类名>();
};
<类名>::~<类名>
{
    //函数体
}
```

## 11.3 默认构造函数
- [ ] 只要显示的定义了构造函数，C++将**不会**再提供默认的构造函数。

## 11.5 类对象数组的初始化
- [ ] 初始化类对象数组时会调用**默认的无参构造函数**，如果该对象没有默认构造函数，将会报错。
- [ ] 数组中**每生成一个对象变量**就会**调用一次无参构造函数**。
```C++
#include <iostream>
using namespace std;

class A {
public:
    //A(int a);
    int a;
};

int main(int arg, char **args) {


    A *AAarry = new A[2];

    AAarry[0].a = 1;
    AAarry[1].a = 2;

    cout << AAarry[1].a << " " << AAarry[0].a << " " << AAarry[3].a << endl;

    delete[] AAarry;
    return 1;
}
```

## 11.6 拷贝构造函数
- [ ] C++中将一个类对象变量赋值给另一个类对象变量**传的是值**而不是引用。
```C++
#include <iostream>
using namespace std;

class A {
public:
    //A(int a);
    int a;
};

int main(int arg, char **args) {
    A a;
    a.a = 1;
    A b;
    b = a;
    b.a++;

    cout << a.a;//输出1

    return 1;
}
```
- [ ] 拷贝构造函数的**名字必须与类名相同，并且没有返回值**。
- [ ] 拷贝构造函数**只能有一个参数**，这个参数是这个类的**地址引用**。
- [ ] 在类中，如果不定义拷贝构造函数，系统会生成一个**默认的拷贝构造函数**。
```C++
class <类名>
{
public:
    <类名>(<类名>& 对象名)//拷贝构造函数的声明。
}
```
- [ ] 浅拷贝会导致**析构函数**中的资源被重复释放，从而运行时报错。

## 11.8 疑难解惑
- [ ] 先调用基类构造函数，再调用子类构造函数。
- [ ] 先调用子类析构函数，再调用基类析构函数。

# 第十二章 运算符的重载
- [ ] **运算符重载函数**的定义：
```C++
<返回类型说明符>operator<运算符符号>(<参数表>)
{
    <函数体>
}
```
- [ ] **类属关系运算符**`.`、**成员指针运算符**`.*`、`::`、**选择表达式**`?:`、`sizeof`不能重载，其他运算符都能重载。
- [ ] 无法改变优先级、结合性、操作数个数以及语法结构。
- [ ] **第一种**运算符的重载方式，运算符重载为**类的成员函数**。
    ```C++
    <返回类型>operator<运算符>(<参数表>)
    {
        <函数体>
    }
    ```
    - [ ] 运算符重载为**类成员函数**时，函数的**参数个数**会比原来**少一个**（**后置单目运算除外**）。
    - [ ] **前置单目运算符**重载为类的**成员函数**时，**没有形参**。
    - [ ] **后置单目运算符**重载为类的**成员函数**时，仍然带有一个**形参**。
    - [ ] 调用格式：`<对象名><运算符>(<参数>)`等价于`<对象名>.operator<运算符>(<参数>)`
    - [ ] 如果一个运算符需要**修改对象状态**，重载为**成员函数**比较好。
    ```C++
    #include <iostream>
    using namespace std;

    class A {
    public:
        int a;

        //运算符成员函数重载。
        void operator+(A a) {
            this->a += a.a;
            return;
        }
    };

    int main(int arg, char **args) {

        A a;
        A b;
        a.a = 10;
        b.a = 11;

        //a .operator+(b);
        a = a + b;
        cout << a.a;
        return 1;
    }
    ```
- [ ] **第二种**运算符重载方式，运算符重载为**类的友元函数**。
    ```C++
    friend <返回值>operator<运算符>(<参数表>)
    {
        <函数体>
    }
    ```
    - [ ] 运算符重载为**类的友元函数**时，操作数个数**无变化**。
    - [ ] 调用格式：`operator<运算符>(<参数表>)`等价于`<参数一><运算符><参数二>`
    - [ ] 一般情况下，**单目运算**最好重载为**类的成员函数**，**双目运算**最好重载为**类的友元函数**。
    - [ ] `=`、`()`、`[]`、`->`不能重载为**类的友元函数**。

## 12.2 重载前置运算符和后置运算符
- [ ] 不插入关键字`int`表示前置运算符重载。
```C++
#include <iostream>

using namespace std;

class A {
public:
    //A(int a);
    int a;

    //不插入int关键字，表示前置运算符重载
    A operator++() {
        a++;
        return *this;
    }
};

int main(int arg, char **args) {
    A a;
    a.a = 1;

    //错误调用方式
    //cout << a++ << endl;//编译错误

    //正确调用方式一
    cout << ++a.a << endl;//输出2，符合预期
    //正确调用方式二
    cout << a.operator++().a << endl;//输出3，符合预期
    return 1;
}
```
- [ ] 插入关键字`int`表示后置运算符重载
```C++
#include <iostream>
using namespace std;

class A {
public:
    //A(int a);
    int a;

    //不插入int关键字，表示前置运算符重载
    A operator++(int) {
        a++;
        return *this;
    }
};

int main(int arg, char **args) {
    A a;
    a.a = 1;

    //错误调用方式
    //cout << (++a).a << endl;//编译错误

    //正确调用方式一
    cout << (a++).a << endl;//输出2，符合预期
    //正确调用方式二，要传入一个整数参数
    cout << a.operator++(1).a << endl;//输出3，符合预期，
    return 1;
}
```

## 12.3 插入运算符`<<`和析取运算符`>>`的重载
- [ ] 常用的`<<`重载函数原型：`ostream& operator<<(ostream&,类型名)`，让自定义类型能够输出。
```C++
#include <iostream>
using namespace std;

class A {
public:
    //A(int a);
    int a;

    //友元重载让自定义类型能够输出
    friend ostream &operator<<(ostream &, A &a) {

        return cout << a.a;
    }
};

int main(int arg, char **args) {


    A a1;
    a1.a = 1;

    cout << a1;//输出1
    return 1;
}
```
- [ ] 常用`>>`重载函数原型：`istream &operator>>(istream&,类型名)`，让自定义类型能够输入。
```C++
#include <iostream>
using namespace std;

class A {
public:
    //A(int a);
    int a;

    //友元重载让自定义类型能够输入
    friend istream &operator>>(istream &, A &a) {

        return cin >> a.a;
    }
};

int main(int arg, char **args) {


    A a1;
    a1.a = 1;

    cin >> a1;//输入4
    cout << a1.a;//输出4
    return 1;
}
```

## 12.4 常用运算符的重载
- [ ] 默认的`=`可以对两个对象实现**浅拷贝**，可能会造成**内存泄露**等错误。
- [ ] 对象还**不存在**的时候会调用**拷贝构造函数**。
```C++
#include <iostream>
using namespace std;

class A {
public:
    //A(int a);
    int a;

    A() {

    }

    A(A &temp) {
        cout << "拷贝构造函数" << endl;
        a = temp.a;
    }

};

int main(int arg, char **args) {
    A a1;
    a1.a = 1;
    A a2 = a1;//输出拷贝构造函数
    return 1;
}
```
- [ ] 对象已经存在可以通过重载`=`来实现**深拷贝**。
```C++
#include <iostream>
using namespace std;

class A {
public:
    //A(int a);
    int a;

    A() {

    }

    A(A &temp) {
        cout << "拷贝构造函数" << endl;
        a = temp.a;
    }

};

int main(int arg, char **args) {
    A a1;
    a1.a = 1;
    A a2;

    a2 = a1;//不会输出拷贝构造函数
    cout << a2.a << endl;//输出1
    return 1;
}
```
- [ ] 注意在**赋值时**已经分配的**指针清除**。

## 12.6 疑难解惑
- [ ] **友元**破坏了封装，如非必要，尽量避免使用友元函数和友元类。

# 第十三章 类的继承
## 13.1 面向对象编程概述
- [ ] 多态性主要表现在**同一个基类**的**不同派生类对象**对**同一个消息响应不同**。
- [ ] 只有`public`继承才能将子类变量赋值给父类。

## 13.2 继承的基本概念
---|public|protected|private
--|--|--|--
公有继承|public|protected|不可见
保护继承|protected|protected|不可见
私有继承|private|private|不可见
- [ ] 继承方式默认是`private`的。
- [ ] 继承方式影响的是**外部访问**和**派生类的子类**。
- [ ] 继承可以继承父类的**所有成员变量和成员方法**，但不能继承**构造方法**。
- [ ] 显示调用基类**有参构造函数**：
```C++
#include <iostream>
using namespace std;

class B {
public:
    B(int a) {

    }
};

class A : public B {

public:
    int a;

    A() : B(1) {//显示调用基类有参构造函数，否则会出错

    }

    A(A &temp) : B(2) {//显示调用基类有参构造函数，否则会出错
        cout << "拷贝构造函数" << endl;
        a = temp.a;
    }

};

int main(int arg, char **args) {


    return 1;
}
```

## 13.3 子类存取父类成员
- [ ] 子类**共享**父类的静态成员，其内存在**编译时**就得到了确认，对该变量的操作都是**针对同一地址段的**。
```C++
#include <iostream>
using namespace std;

class B {
public:
    static int a;

    B(int a) {

    }
};

int B::a = 1;

class A : private B {

public:

    A() : B(1) {//显示调用基类有参构造函数，否则会出错
        a = 2;
        cout << a << endl;
    }

    A(A &temp) : B(2) {//显示调用基类有参构造函数，否则会出错
        cout << "拷贝构造函数" << endl;
        a = temp.a;
    }

};

int main(int arg, char **args) {

    A a;//输出2

    return 1;
}
```
- [ ] 由于在**多次继承**后，很难辨别哪些成员可以访问，所以继承一般使用**公有继承**。
- [ ] 作用域运算符`::`用于**指明**多继承中调用的是哪一个**类的成员**。

## 13.5 疑难解惑
- [ ] 构造函数的**执行顺序**：父类构造函数-》初始化列表-》子类构造函数
```C++
#include <iostream>
using namespace std;

class C {
public:
    int c = 1;

    C(int newC) {
        c = newC;
    }
};

int main(int arg, char **args) {
    C c(2);
    cout << c.c << endl;//输出2

    return 1;
}
```
- [ ] 如果子类中**重新定义了成员**，完成对基类成员的覆盖，此时可以用`::`引用基类版本的成员。
- [ ] 不能继承**构造函数**。
- [ ] 不能继承**析构函数**。
- [ ] 不能继承**用户定义的`new`运算符**。
- [ ] 不能继承**用户定义的赋值运算符**`=`。
- [ ] 不能继承**友元关系**。

# 第十四章 虚函数和抽象函数
- [ ] 在基类中声明为`virtual`并且在一个或者多个子类中被**重新定义**的成员函数称为**虚函数**。
- [ ] **虚函数**用于实现**多态**。
- [ ] 只有采用`指针->虚函数();`或者`引用对象.虚函数();`才会执行虚函数**动态绑定**。
- [ ] 静态绑定发生在**编译时期**，动态绑定发生在**运行期**。

## 14.2 抽象类与纯虚函数
- [ ] **纯虚函数**的声明。
```C++
class <类名>
{
    virtual <类型><函数名>(<参数表>)=0;
};
```
- [ ] 纯虚函数的定义会被编译器忽略。
- [ ] 带有纯虚函数的类称为抽象类。
- [ ] 抽象类不能定义对象，用于被继承，充当接口。
- [ ] 虚函数会**增加内存开销**。
- [ ] 使用**虚析构函数**让一个**基类的指针删除派生类对象**时，**派生类的虚构函数**会被调用。

## 14.3 抽象类的多重继承
- [ ] 一般**先定义多个抽象类**，再通过**多重继承抽象类**来实现**具有多个基类性质**的子类定义。

## 14.4 虚函数表
- [ ] 多态可以使用**不变的调用语句**调用**不同的实现函数**。
- [ ] C++通过**虚函数表**实现虚函数的调用。
- [ ] 虚函数表中保存**虚函数的地址**以及虚函数**由那个类继承**。
- [ ] 通过**对象地址**寻找**虚函数表的地址**，遍历该表的虚函数地址，通过地址调用相应的虚函数。

## 14.6 疑难解惑
- [ ] 为了提高程序的**清晰性**，最好在**每一层中显示声明虚函数**。
- [ ] 虚函数具有**继承性**，其在子类中即使没有显示声明也是虚函数。
- [ ] 虚函数和纯虚函数不能有`static`关键字修饰，因为`static`要求编译时绑定，而虚函数要求运行时绑定。

# 第十五章 C++中文件处理
## 15.1 文件的基本概念
- [ ] 文件操作步骤
    1. 建立文件流对象。
    2. 打开或建立文件。
    3. 进行读写操作。
    4. 关闭文件。
- [ ] 文件I/O的流类主要有三个，包含在`<fstream>`头文件中：`fstream(输入输出文件流)`、`ifstream(输入文件流)`、`ofstream(输出文件流)`。
- [ ] `ifstream`对象如果**重复使用**，注意在使用前调用`clear()`函数，否则会**出错**。
```C++
#include <iostream>
#include <fstream>
using namespace std;

int main(int arg, char **args) {
    char buffer[256];
    fstream out;

    out.open("/Users/zhouqinchun/CLionProjects/StudyCPlusPlus/cmake-build-debug/CMakeFiles/CMakeLists.txt", ios::in);
    if (!out.is_open()) {
        cout << "打开文件失败" << endl;
        return 0;
    }


    cout << "CMakeLists.txt" << " 的内容如下:" << endl;

    while (!out.eof()) {
        out.getline(buffer, 256, '\n');//表示字符达到256个或者遇到'\n'结束
        cout << buffer << endl;
    }

    out.close();
    return 1;
}
```

## 15.2 文件的打开与关闭
- [ ] 函数原型：`void open(const char* filename,int mode,int access);`。`access`变量是打开文件的属性：`0`普通文件，打开访问；`1`只读文件；`2`隐藏文件；`3`系统文件。
- [ ] 打开文件的模式`mode`：
    - [ ] `ios::app`追加模式。
    - [ ] `iOS::ate`文件打开后定义到文件尾，`iOS::app`包含此属性。
    - [ ] `ios::binary`以二进制方式打开文件，默认为文本方式。
    - [ ] `ios::in`以输入方式打开。
    - [ ] `ios::out`以输出方式打开。
    - [ ] `ios::nocreate`不建立文件，所以文件不存在时写文件打开失败。
    - [ ] `ios::noreplace`不覆盖文件，所以文件存在时写文件打开失败。
    - [ ] `ios::trunc`如果文件存在，把文件长度设为0。清除文件。
- [ ] 为防止流对象被销毁还联系着文件，**析构函数**会自动调用关闭函数`close()`

## 15.3 文本文件的处理
- [ ] 文本文件可以用`<<`输出到文件，用`>>`从文件输入(不能读入换行符),
```C++
#include <iostream>
#include <fstream>
using namespace std;

int main(int arg, char **args) {

    ofstream outfile;
    outfile.open("123.txt");
    outfile << "123" << endl;

    outfile.close();
    return 1;
}
```
```C++
#include <iostream>
#include <fstream>
using namespace std;

int main(int arg, char **args) {

    ifstream infile;
    infile.open("123.txt");

    if (infile.fail()) {
        cout << "打开文件失败！" << endl;
        return 0;
    }

    char c;
    while (!infile.eof()) {
        infile.get(c);//>>不能读入换行符，而get方法可以读入换行符。
        cout << c;
    }

    return 1;
}
```
- [ ] `get()`函数读取类对象的一个字符，并且可以将该值作为返回值，读到文件末尾返回`EOF`。
- [ ] `getline()`函数与带三个参数的`get()`函数类似，但是`getline()`函数会**除去分隔符**。
- [ ] `put(char)`函数用于向输出流写入一个字符。
- [ ] **状态**流对象成员函数：
    - [ ] `bad()`读写文件出错，返回`ture`。例如：当我们要对一个**不是打开为写状态的文件进行写入**时，或者我们**要写入的设备没有剩余空间**的时候。
    - [ ] `fail()`除了与bad() 同样的情况下会返回 true 以外，加上格式错误时也返回true ，例如当想要读入一个整数，而获得了一个字母的时候。
    - [ ] `good()`除了调用`eof()`、`bad()`、`fail()`返回`true`的情况下，都返回`false`。
- [ ] 文件流指针操作：
    - [ ] `tellg()`返回整数，当前**get指针**的位置。
    - [ ] `tellp()`返回整数，当前**put指针**的位置。
    - [ ] `seekg(pos_type position);`改变**流指针get**的位置，绝对位置。
    - [ ] `seekp(pos_type position);`改变**流指针put**的位置，绝对位置。
    - [ ] `seekg ( off_type offset, seekdir direction );`改变**流指针get**的位置，从**偏移位置**处开始】。
    - [ ] `seekp ( off_type offset, seekdir direction );`改变**流指针put**的位置，从**偏移位置**处开始】。
    - [ ] 偏移位置：

    | `ios::beg` | 从流开始位置处开始计算 |
    | -- | -- |
    | `ios::cur`|从流的当前位置处开始计算|
    | `ios::end`|从流的末尾位置处开始计算|

## 15.4 二进制文件的处理
- [ ] 二进制文件中使用`<<`与`>>`和`getline()`函数来输入输出数据是没有意义的，但是它们是符合语法的。1
- [ ] 二进制文件
- [ ] `write(char * buffer, streamsize size);`往二进制文件中写内容。
- [ ] `read(char * buffer, streamsize size );`

## 15.6 疑难解惑
- [ ] 流缓存类型`streambuf`。
- [ ] 缓存同步过程中：**输入流**的缓存被抹掉，**输出流**的缓存被全部写入物理媒介。
- [ ] 缓存同步：
    - [ ] 文件关闭时。
    - [ ] 缓存满了时。
    - [ ] 控制符指定，如`flush`和`endl`。
    - [ ] 明确调用`sync()`进行同步时。

# 第十六章 异常处理
## 16.1 异常的基本概念
- [ ] 异常并非总是类对象，throw可以抛出任何类型的对象，最常用的是类对象。
- [ ] `new`出来的对象在**抛出异常时不会被自动释放**，在抛出异常前应该**手动将其析构**。
- [ ] 当`throw`对象时，拷贝的是**引用对象**，而不只是**地址**。被抛出的对象会被**析构**。
- [ ] `catch(...){}`会保证所有的异常都能进入。

## 16.7 标准异常
- [ ] 所有标准异常处理类都派生自`Exception`类，标准异常类都支持一个what()方法返回一个描述异常的`char*`字符串。除了`Exception`类，所有的**标准异常类**都要求在**构造函数中**设置`what()`方法所返回的字符串。

## 16.9 异常规范
- [ ] 一个函数的**异常规范违例**只有在**运行时**才会被检测。
- [ ] 如果函数抛出一个没有被列在异常规范中的异常，则系统会调用C++标准库中定义的函数`unexpected()`。
- [ ] 格式：`返回值 函数名(参数表) throw(异常)`。
- [ ] VC++6.0不支持异常规范，在其中异常规范无效果。

## 16.10 异常与继承
- [ ] 在`catch{}`块中用`throw;`语句可以抛出上次抛出过的异常。
- [ ] `virtual const char* what() const{}`继承自`Exception`的一个虚方法。

## 16.13 疑难解惑
- [ ] **一般**在异常抛出后会调用析构函数，但是在**构造函数中**抛出异常将不会调用析构函数。

# 第十七章 模板与类型转换
## 17.1 模板
- [ ] 函数模板定义：
```C++
template<class 类型参数名1, class 类型参数名2,...>
函数返回类型 函数名(参数表)
{
    函数体
}
```
- [ ] 一般把一个普通的函数改造成一个通用函数模板。
- [ ] 类模板定义
```C++
template<class或者typename T,...>
class 类名
{};
```
- [ ] 模板特殊化：
```C++
#include<iostream>
#include<string>

using namespace std;

template<typename T>
class A
{
public:

	//其他类型调用这个函数时返回0
	T getInt() 
	{
		return 0;
	}
};

//特殊化模板
template<>//不能忘记了，否则会报错
class A<int>
{
public:
	
	int getInt() 
	{
		return 1;
	}

};

int main()
{
	A<double> d;
	A<int> i;

	cout << d.getInt() << endl;//输出0
	cout << i.getInt() << endl;//输出1
}
```
- [ ] 函数模板重载匹配规则（**有先后顺序**）：
    1. 寻找和使用**最符合函数名和参数类型的函数**。
    2. 寻找符合要求的**函数模板**。
    3. 寻到可以通过**类型转化**进行参数匹配的函数，不一定是强制类型转换。
- [ ] 调用函数不明确在编译时会报错：
```C++
#include<iostream>
#include<string>
using namespace std;

void Test(int a = 1) 
{
	cout << a << endl;
}

void Test() 
{
	cout << 0 << endl;
}

int main()
{
	Test();//编译不通过
	return 1;
}
```

## 类型识别和强制转换运算符
- [ ] 类型识别会付出一定的**效率代价**。
- [ ] **RTTI函数**依赖驻留在**虚函数表**中的信息来在运行时识别类型。
- [ ] 使用`typeid()`关键字
```C++
#include <iostream>
using namespace std;

int main(int arg, char **args) {
    //用typeid获取运行时类型信息
    cout << typeid(cin).name() << endl;

    return 1;
}
```
- [ ] 动态类型**安全转换**，转换失败返回NULL。`dynamic_cast<要转换的类型*>(被转换的指针)`。
- [ ] 四种安全的强制类型转换运算符：
    - [ ] `const_cast`去除`const属性`。
    - [ ] `reinterpret_cast`不同类型的指针类型转换。
    - [ ] `static_cast`基本类型转换。
    - [ ] `dynamic_cast`动态类型转换。
- [ ] `static_cast<T*>(a);`将地址`a`转换为类型`T`的地址，T和a必须是**指针、引用、算术类型或者枚举类型。**
- [ ] `const_cast<type_id>(expression);`用来修改类型的`const`或`volatile`属性，除了有`const`和`volatile`修饰外，`type_id`和`expression`类型一样。
    - [ ] 常量指针被转换为非常量指针，并且仍然指向原来的对象。
    - [ ] 常量引用被转换为非常量引用，并且仍指向原来的对象。
    - [ ] 常量对象被转换为非常量对象。
```C++
#include <iostream>

using namespace std;

//data和d公用同一片地址，但是data的数据在编译时被替换了
int main() {
    const int data = 8;
    int &d = const_cast<int & >(data);//引用
    cout << &data << ":" << data << endl;//0x7ffeed5d986c:8
    d += 2;
    cout << &d << ":" << d << endl;//0x7ffeed5d986c:12
    cout << &data << ":" << data << endl;//0x7ffeed5d986c:8
}
```

## 17.4 疑难解惑
- [ ] 模板类由两种实现方式：
    - [ ] 将C++模板类的声明和定义都**放在一个文件中**，使用时`#include`模板类文件名.h(或.cpp)。
    - [ ] 将C++模板类的声明放在`.h`文件中，定义放在`.cpp`文件中，并且在`.cpp`文件中。使用方法：
        1. 继承开发环境code::blocks下，只引入`.cpp`可以编译，只引入`.h`不可以编译。同时引入`.h`和`.cpp`可以编译。
        2. 在Linux GCC环境下，只能引入`.cpp`可以编译，如果同时引入`.h`和`.cpp`将不能编译。
- [ ] 模板类可以继承：
```C++
//仍然是模板类
template<T>
class SafeVector:public vector<T>
//不是模板类了
class SafeVector:public vector<int>
```

# 第十八章 容器与迭代器
## 18.2 迭代器
- [ ] 更倾向于**迭代器**而不是**下标**来访问容器。
- [ ] 输入迭代器：支持`*p、++p、p++、p!=q、p==q`，只允许读取，**不允许修改**。
- [ ] 输出迭代器：操作同输入迭代器一致，**可以修改**。
- [ ] 前向迭代器：输入和输出迭代器的组合。
- [ ] 双向迭代器：在前向迭代器上增加`--p`和`p--`操作。
- [ ] 随机存取迭代器。
- [ ] 迭代器的`end`操作指向末端的下一个元素，该元素不存在。
- [ ] 改变`vector`长度的操作会时**已存在的迭代器失效**，变得不再正确。

## 18.3 顺序容器
- [ ] `vector`的操作：
    - [ ] `push_back(t);`在向量**最后**添加元素。
    - [ ] `v[n];`返回v中位置为n的元素。
    - [ ] `v1 = v2;`将v1的元素替换为v2元素的**副本**。
    - [ ] `v1 == v2;`判断v1与v2是否相等。
    - [ ] `!，=，<，<=，>，>=`保持惯有意义。
- [ ] `vector`初始化对象
```C++
#include <iostream>
#include <vector>
using namespace std;

class A {
public:
    int a;

    A() {
        a = 1;
    }
};

int main() {

    vector<A> v(1);//指定初始化容量，会调用默认初始化构造函数
    cout << v[0].a << "容量：" << v.size() << endl;//输出1，1

}
```
- [ ] `deque`双端队列操作：
    - [ ] `push_back(t);`在末尾添加t。
    - [ ] `push_front(t);`在开头添加t。
- [ ] `list`：
    - [ ] `list<T>::iterator iter;`定义list的迭代器。
    - [ ] `push_back();`
    - [ ] `push_front();`
    - [ ] 进行迭代：
    ```C++
    for(iter = elements.begin();iter!=elements.end();iter++)
    {
        cout<<"元素"<<*iter<<endl;¬
    }
    ```
    - [ ] `erase(elements.begin());`删除首位元素。

## 18.4 关联容器
- [ ] `set`集合，由链表组织。
    - [ ] 如果要修改某一个元素的值，必须先删除原有元素，再插入新元素。
- [ ] `map`不支持副本建，`multimap`支持副本建。
    - [ ] **键**本身不能修改，除非删除。
    ```C++
    map<char,int,less<char>> map1;//定义map
    map<char,int,less<char>>::iterator mapIter;//定义迭代器
    map1['c'] = 3;//赋值初始化
    //遍历
    for(mapIter=map1.begin();mapIter!=map1.end();++mapIter)
    {
        cout<<""<<(*mapIter).first<<(*mapIter).second;//first对应第一个键，second对应第二个值。
    }
    //检索'd'
    map<char,int,less<char>>::const_iterator ptr;
    ptr = find('d');
    cout<<ptr->first<<ptr->second<<endl;
    ```

## 18.5 容器适配器
- [ ] 适配器是使**一类事物的行为**类似于**另一类事物的行为**的一种机制。
- [ ] `stack`用`vector`、`list`、`deque`实现。
- [ ] `queue`默认用`deque`实现。
- [ ] `priority_queue`默认用`vector`实现，**按优先级插入元素**。