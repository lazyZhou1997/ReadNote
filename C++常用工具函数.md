参考资料：
> [算法库（参考手册)](http://classfoo.com/ccby/article/tZTzs)

# 

# 不修改内容的序列操作
## `find()`函数
- [ ] 函数声明:
```C++
template <class InputIterator, class T>
    InputIterator find (InputIterator first,
                        InputIterator last,
​                        const T& val);
```
- [ ] 参数说明：
    - [ ] `first`，`last`范围即`[first,last)`。
- [ ] 返回：返回指向范围中与 val 等值的**第一个元素的迭代器**。如果没有元素匹配，则返回**last迭代器**。
- [ ] 复杂度：O(n)
- [ ] 代买示例：
```C++
int a[] = {1, 2, 3, 4, 5, 6, 7, 8};
vector<int> v(a, a + sizeof(a) / sizeof(int));
cout << *find(v.begin(), v.end(), 5) << endl;
```

# 修改内容的序列操作
## `copy()`
- [ ] 函数声明：
```C++

template <class InputIterator, class OutputIterator>
    OutputIterator copy (InputIterator first,
​        InputIterator last, OutputIterator result);
```
- [ ] 参数说明：
    - [ ] `first`,`last`范围即`[first,last)`。
    - [ ] `result`指向**目标序列**初始位置的**输出迭代器**（Output iterators）。该迭代器**不能指向**范围 [first,last) 中的任何元素。
- [ ] 返回：返回所有元素拷贝完成后，表示**目标范围末尾**（End）的迭代器。
- [ ] 代买示例：
```C++
// 将 SmallVector 拷至标准输出流中
std::copy(
    SmallVector.begin(),
    SmallVector.end(),
    std::ostream_iterator<int>(std::cout, " "));
std::cout << std::endl;
```
## `sort()`
- [ ] 函数声明：
```C++
template <class RandomAccessIterator>
    void sort (RandomAccessIterator first,
        RandomAccessIterator last);
//重载函数 
template <class RandomAccessIterator, class Compare>
    void sort (RandomAccessIterator first,
        RandomAccessIterator last, Compare comp);
```
- [ ] 参数说明：
    - [ ] `first`,`last`初始及末尾位置的**随机访问迭代器**（Random-access Iterators），范围即`[first,last)`。
    - [ ] `comp`二元谓词（Binary）函数，以**两个元素为参数**，然后**返回**一个可转换成**bool类型**的值。其返回值表明按所指定的严格弱序排序时，**第一个参数所传进来的元素是否在第二个参数所传进来的元素前面**。**该函数不能修改其参数**。可以是**函数指针**（Function pointer）类型或**函数对象**（Function object）类型。
- [ ] 复杂度：O(n*log2(n))。
- [ ] 代码示例一：
```C++
#include <iostream>
#include <vector>

bool IsGreate(int a, int b) {
    return a > b;
}

class IsLess {
public:
    bool operator()(int, int);
};

bool IsLess::operator()(int a, int b) {
    return a < b;
}

int main(int arg, char **args) {

    IsLess less;
    //定义vector
    int a[] = {6, 3, 8, 1, 9, 5, 8, 4, 6};
    std::vector<int> v(a, a + sizeof(a) / sizeof(int));

    //默认的sort()函数
    std::sort(v.begin(), v.end());
    //输出
    std::cout << "默认排序结果：";
    std::copy(v.begin(), v.end(), std::ostream_iterator<int>(std::cout, ","));//分隔符为","
    std::cout << std::endl;

    //重载排序函数，采用传入函数指针
    std::sort(v.begin(), v.end(), IsGreate);
    //输出
    std::cout << "传入函数指针，采用重载函数：";
    std::copy(v.begin(), v.end(), std::ostream_iterator<int>(std::cout, ","));//分隔符为","
    std::cout << std::endl;

    //重载排序函数，采用传入函数对象
    std::cout << "传入函数对象，采用重载函数：";
    std::copy(v.begin(), v.end(), std::ostream_iterator<int>(std::cout, ","));
    std::cout << std::endl;

    return -1;
}
```
- [ ] 代码示例二：
```C++
#include <iostream>
#include <vector>

class Point {
public:
    Point(int x, int y) : x(x), y(y) {};

    int x, y;

    //使用友元函数
    friend bool PointLessThan(Point &a, Point &b);

    friend std::ostream &operator<<(std::ostream &, Point &);
};

std::ostream &operator<<(std::ostream &cout, Point &point) {
    cout << "(" << point.x << "," << point.y << ")" << std::endl;
    return cout;
}

//先比较x，再比较y
bool PointLessThan(Point &a, Point &b) {
    if (a.x == b.x) {
        return a.y < b.y;
    }

    return a.x < a.y;
}

int main(int arg, char **args) {

    std::vector<Point> points;

    points.push_back(Point(3, 7));
    points.push_back(Point(1, 9));
    points.push_back(Point(1, 3));
    points.push_back(Point(6, 3));
    points.push_back(Point(7, 2));
    points.push_back(Point(6, 2));

    //排序
    std::sort(points.begin(), points.end(), PointLessThan);
    for (std::vector<Point>::iterator current = points.begin(); current != points.end(); current++) {
        std::cout << *current << std::endl;
    }

    return 1;
}
```

# 划分操作
# 排序操作
# 二分法查找操作
## `binary_search`
- [ ] 函数声明：
```C++
template <class ForwardIterator, class T>
    bool binary_search (
        ForwardIterator first, 
        ForwardIterator last,const T& val);
//重载函数   
template <class ForwardIterator,
    class T, class Compare>
    bool binary_search (
        ForwardIterator first,
        ForwardIterator last,
        const T& val, Compare comp);
```
- [ ] 参数说明：
    - [ ] `first`，`last`初始及末尾位置的**正向访问迭代器**，范围即`[first,last)`。
    - [ ] `val`用于**查找的值**。
    - [ ] `comp`二元谓词（Binary）函数，以**两个元素为参数**，然后**返回**一个可转换成**bool类型**的值。其返回值表明按所指定的严格弱序排序时，**第一个参数所传进来的元素是否在第二个参数所传进来的元素前面**。**该函数不能修改其参数**。可以是**函数指针**（Function pointer）类型或**函数对象**（Function object）类型。
- [ ] 返回值：如果**找到**一个元素的值等价于val，则**返回true**。否则，返回false。
- [ ] 复杂度：`O(log2(N) + 2)`。
- [ ] 代码示例：
```C++

```
# 集合操作
# 堆操作
# 最大/最小操作
# 数值操作
# C 库算法