# 函数对象
- [ ] 定义说明：重载**函数调用操作符**的类，其对象常称为**函数对象**（function object），即它们是行为类似函数的对象。又称**仿函数**。
- [ ] 例如：
```C++
#include <iostream>
using namespace std;

//通过类来实现
class Sum {
public:
    int operator()(int a, int b);
};

int Sum::operator()(int a, int b) {
    return a + b;
}

//通过结构体来实现
struct Minus {
    int operator()(int a, int b);
};

int Minus::operator()(int a, int b) {
    return 1 - b;
}

int main() {

    Sum sum;
    cout << sum(1, 2) << endl;//3
    Minus minus;
    cout<<minus(1,2)<<endl;//-1
}
```

# 可变参数宏
- [ ] **缺省号**代表一个可以变化的**参数表**。使用保留名 __VA_ARGS__ 把参数传递给宏。
- [ ] 例如：
```C++
#define debug(...) printf(__VA_ARGS__)

//定义vector
#define CLASSFOO_VECTOR(type, name, ...) \
static const type name##_a[] = __VA_ARGS__; \
std::vector<type> name(name##_a, name##_a + sizeof(name##_a) / sizeof(*name##_a))
//使用
CLASSFOO_VECTOR(int, BigVector1, { 8, 23, -5, 7, 29, 0, 7, 7, -7, 1, -1 });
```
# 初始化成员变量
- [ ] 在构造函数后面调用`成员变量(值)`：
```C++
#include <iostream>

class Test {
public:
    Test(int a, int b);

    int a;
    int b;

    ~Test() {}
};

Test::Test(int a, int b) : a(a), b(b) {}//初始化成员函数的新方式

int main(int arg, char **args) {

    Test test(20, 80);
    std::cout << test.a << " " <<test.b << std::endl;//输出：20 80

    return -1;
}
```