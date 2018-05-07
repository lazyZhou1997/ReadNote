---
ebook:
  title: OpenCV入门教程-于仕琪
  authors: zhoulanlan
---

# 第一章 预备知识
## 1.8 库文件
- [ ] **动态链接库**和**静态链接库**。

# 第二章 OpenCV介绍

# 第三章 图像的基本操作
## 3.1 图像的表示
- [ ] 一般来说，**像素值越大，该点越亮**。
- [ ] 对于图像显示，大部分设备都使用**无符号8位整数**(`CV_8U`)。
- [ ] 图像数据在计算机内存中的存储顺序为以图像**最左上点**(也可能是**最左下点**)开始。

## 3.2 Mat 类
- [ ] 图像类、矩阵类**Mat的声明：**
  ```C++
  class CV_EXPORTS Mat
   {
   public:
    //一系列函数 ...
    /* flag参数中包含许多关于矩阵的信息，如: -Mat 的标识
    -数据是否连续 -深度 -通道数目
    */
    int flags;

    //矩阵的维数，取值应该大于或等于2 
    int dims;
    //矩阵的行数和列数，如果矩阵超过2维，这两个变量的值都为-1 
    int rows, cols;
    //指向数据的指针
    uchar* data;
    //指向引用计数的指针 //如果数据是由用户分配的，则为 NULL
    int* refcount;
    //其他成员变量和成员函数
    ... 
  };
  ```

## 3.3 创建 Mat 对象
- [ ] `create()`函数创建对象。
  - [ ] 如果 create()函数指定的参数与图像之前的**参数相同**，则**不进行实质的内存申请操作**;如果**参数不同**，则**减少原始数据内存的索引**，并**重新申请内存**。
  ```C++
  Mat M(2,2, CV_8UC3);//构造函数创建图像 
  M.create(3,2, CV_8UC2);//释放内存重新创建图像
  ```
- [ ] 有些 `type` 参数如 `CV_32F` **未注明通道数目**，这种情况下它表示**单通道**。
- [ ] 零矩阵 `Mat Z = Mat::zeros(2,3, CV_8UC1);`
- [ ] 全为1的矩阵 `Mat O = Mat::ones(2, 3, CV_32F);`
- [ ] 对角线上为1的矩阵 `Mat E = Mat::eye(2, 3, CV_64F);`

## 3.4 矩阵的基本元素表达
- [ ] OpenCV 中有模板类 Vec，可以表示一个**向量**。OpenCV 中使用 Vec 类预定义了**一些小向量**，可以将之用于矩阵元素的表达。如：
  ```C++
  typedef Vec<uchar, 2> Vec2b;
  typedef Vec<uchar, 3> Vec3b;
  typedef Vec<uchar, 4> Vec4b;
  ```
- [ ] 对于 Vec 对象，可以使用[]符号**如操作数组般读写**其元素，如：
  ```C++
  Vec3b color; //用color变量描述一种RGB颜色 
  color[0]=255; //B分量
  color[1]=0; //G分量
  color[2]=0; //R分量
  ```

## 3.5 像素值的读写
- [ ] 函数 `at()` 来实现**读**去矩阵中的某个像素，或者对某个像素进行**赋值**操作，例如：
  ```C++
  uchar value = grayim.at<uchar>(i,j);//读出第i行第j列像素值
  grayim.at<uchar>(i,j)=128; //将第i行第j列像素值设置为128
  ```
  - [ ] 如果要遍历图像，并**不推荐**使用 `at()` 函数。使用这个函数的优点是代码的**可读性高**，但是**效率并不是很高**。
- [ ] Mat 也增加了**迭代器**的支持，以便于矩阵元素的遍历，例如：
  ```C++
  Mat grayim(600, 800, CV_8UC1); 
  Mat colorim(600, 800, CV_8UC3);
  
  //遍历所有像素，并设置像素值 
  MatIterator_<uchar> grayit, grayend;
  for( grayit = grayim.begin<uchar>(), grayim.end<uchar>(); grayit != grayend; ++grayit)
      *grayit = rand()%255;
  
  //遍历所有像素，并设置像素值
  MatIterator_<Vec3b> colorit, colorend;
  for( colorit = colorim.begin<Vec3b>(),
  colorim.end<Vec3b>(); colorit != colorend; ++colorit) {
    (*colorit)[0] = rand()%255; //Blue 
    (*colorit)[1] = rand()%255; //Green 
    (*colorit)[2] = rand()%255; //Red
  }
  ```
- [ ] 通过 **指针** 操作来访问像素是 **非常高效** 的，同时也是 **危险的** 。如：
  ```C++
  Mat grayim(600, 800, CV_8UC1); 
  Mat colorim(600, 800, CV_8UC3);

  //遍历所有像素，并设置像素值
  for( int i = 0; i < grayim.rows; ++i) {
    //获取第 i 行首像素指针
    uchar * p = grayim.ptr<uchar>(i);
  
  //对第 i 行的每个像素(byte)操作
  for( int j = 0; j < grayim.cols; ++j )
    p[j] = (i+j)%255;
  }
  //遍历所有像素，并设置像素值
  for( int i = 0; i < colorim.rows; ++i) {
    //获取第 i 行首像素指针
    Vec3b * p = colorim.ptr<Vec3b>(i);
    for( int j = 0; j < colorim.cols; ++j ) {
      p[j][0] = i%255; //Blue 
      p[j][1] = j%255; //Green 
      p[j][2] = 0; //Red
    }
  ```

## 3.6 选取图像的局部区域
- [ ] 如果将局部区域**赋值**给新的 **Mat对象**，新对象与原始对象 **共用相同的数据区域** ，不新申请内存。
- [ ] 提取矩阵的一行或者一列可以使用函数 `row()` 或 `col()`。
  ```C++
  //取i行的元素乘以2之后赋值给j行
  A.row(j) = A.row(i)*2;
  ```
- [ ] **Range对象** 可以用来表示矩阵的多个 **连续的行** 或者多个 **连续的列** 。定义如下：
  ```C++
  class Range
  {
  public:
    ...
    int start, end;
  };
  ```
  - [ ] `Range` 类还提供了一个静态方法 **all()**，这个方法的作用如同 `Matlab` 中的“:”， 表示 **所有的行** 或者 **所有的列** 。如：
  ```C++
  //创建一个单位阵
  Mat A = Mat::eye(10, 10, CV_32S);
  //提取第 1 到 3 列(不包括 3)
  Mat B = A(Range::all(), Range(1, 3));
  //提取 B 的第 5 至 9 行(不包括 9)
  //其实等价于 C = A(Range(5, 9), Range(1, 3))
  Mat C = B(Range(5, 9), Range::all());
  ```
- [ ] 从图像中提取感兴趣的区域的方法：**使用构造
函数** 或 **使用括号运算符**。
  - [ ] 使用构造函数：
  ```C++
  //创建宽度为 320，高度为 240 的 3 通道图像 
  Mat img(Size(320,240),CV_8UC3);
  //roi 是表示 img 中 Rect(10,10,100,100)区域的对象 
  Mat roi(img, Rect(10,10,100,100));

  //或者使用Range对象
  Mat roi4(img, Range(10,100),Range(10,100));
  ```
  - [ ] 使用括号运算符：
  ```C++
  Mat roi2 = img(Rect(10,10,100,100));
  //获取使用Range对象
  Mat roi3 = img(Range(10,100),Range(10,100));
  ```
- [ ] 取对角线元素 `Mat Mat::diag(int d) const`
  - [ ] 参数 **d=0** 时，表示 **取主对角线** ;当参数 **d>0** 是，表示取主对角线 **下方的次对角线**，如 d=1 时，表示取主对角线下方，且紧贴主多角线的元素;当参数 **d<0** 时， 表示取主对角线 **上方的次对角线**。
  - [ ] `diag()` 函数也 **不进行内存复制操作**，其复杂度也是 O(1)。

## 3.7 Mat表达式
- [ ] 下面的列表中使用 A 和 B 表示 **Mat类型的对象**，使用 s 表示  **Scalar对象**，alpha 表示 **double值**。
  - [ ] 加法，减法，取负:A+B，A-B，A+s，A-s，s+A，s-A，-A
  ```C++
  C = A + B + 1;
  ```
  - [ ] 缩放取值范围：`A*alpha`
  - [ ] 矩阵 **对应元素** 的乘法和除法: A.mul(B)，A/B，alpha/A。
  - [ ] **矩阵乘法** :A*B (注意此处是矩阵乘法，而不是矩阵对应元素相乘)
  - [ ] 矩阵 **转置** :A.t()
  - [ ] 矩阵 **求逆** 和 **求伪逆** :A.inv()
  - [ ] 矩阵 **比较运算** :A cmpop B，A cmpop alpha，alpha cmpop A。此处 cmpop 可以是>，>=，==，!=，<=，<。如果 **条件成立** ，则结果矩阵(8U 类型矩 阵)的 **对应元素被置为255** ;否则置0。
  - [ ] 矩阵 **位逻辑运算** :A logicop B，A logicop s，s logicop A，~A，此处 logicop 可以是&，|和^。
  - [ ] 矩阵 **对应元素** 的最大值和最小值:min(A, B)，min(A, alpha)，max(A, B)， max(A, alpha)
  - [ ] 矩阵中元素的 **绝对值** :abs(A)
  - [ ] **叉积** 和 **点积** :A.cross(B)，A.dot(B)

## 3.8 Mat类
- [ ] Mat类定义：
  ```C++
  template<typename _Tp> class Mat_ : public Mat {
  public:
    //只定义了几个方法
    //没有定义新的属性 
  };
  ```
  - [ ] 使用 **Mat_类**，那么就可以在 **变量声明时确定元素的类型**，访问元素时不再需要指定元素类型，即使得代码简洁，又减少了出错的可能性。
  - [ ] 例如：
  ```C++
  #include <iostream>
  #include "opencv2/opencv.hpp" #include <stdio.h>
  using namespace std;
  using namespace cv;

  int main(int argc, char* argv[])
  {
    Mat M(600, 800, CV_8UC1);
    for( int i = 0; i < M.rows; ++i) {
      //获取指针时需要指定类型
      uchar * p = M.ptr<uchar>(i);
      
      for( int j = 0; j < M.cols; ++j ) {
        double d1 = (double) ((i+j)%255);
        //用 at()读写像素时，需要指定类型 M.at<uchar>(i,j) = d1;
        //下面代码错误，应该使用 at<uchar>() //但编译时不会提醒错误
        //运行结果不正确，d2 不等于 d1
        double d2 = M.at<double>(i,j); 
      }
    }

    //在变量声明时指定矩阵元素类型 
    Mat_<uchar> M1 = (Mat_<uchar>&)M; 
    for( int i = 0; i < M1.rows; ++i) {
      //不需指定元素类型，语句简洁 37
      uchar * p = M1.ptr(i);
      for( int j = 0; j < M1.cols; ++j ) {
        double d1 = (double) ((i+j)%255);
        //直接使用 Matlab 风格的矩阵元素读写，简洁 M1(i,j) = d1;
        double d2 = M1(i,j);
      } 
    }
  return 0; 
  }
  ```

## 3.9 Mat类的内存管理
- [ ] Mat由两个数据部分组成：**矩阵头(包含矩阵尺寸，存储方法，存储地址等信息)** 和一个指向存储所有像素值的 **矩阵的指针**。
- [ ] 创建一个矩阵，并用 **随机数填充**。填充的范围由 randu()函数的第二个参数和第三个参数确定，下面代码是介于 0 到 255 之间。
  ```C++
  Mat R = Mat(3, 2, CV_8UC3);
  randu(R, Scalar::all(0), Scalar::all(255));
  ```
- [ ] Mat的输出格式：
  - [ ] 默认格式：
  ```C++
  cout << "R (default) = " << endl << R << endl << endl;
  ```
  - [ ] Python格式:
  ```C++
  cout << "R (python) = " << endl << format(R,"python") << endl
  ```
  - [ ] 以逗号分割格式:
  ```C++
  cout << "R (csv) = " << endl << format(R,"csv" ) << endl << endl;
  ```
  - [ ] numpy格式:
  ```C++
  cout << "R (numpy) = " << endl << format(R,"numpy" ) << endl << endl;
  ```
  - [ ] C语言格式：
  ```C++  
  cout << "R (c) = " << endl << format(R,"C" ) << endl << endl;
  ```
- [ ] Mat转 **IplImage** 和 **CvMat**：
  - [ ] Mat 转为 **IplImage** 或者 **CvMat**，可以直接用 **= 赋值**，如：
    ```C++
    Mat img(Size(320, 240), CV_8UC3);
    IplImage iplimg = img; //转为IplImage结构 
    CvMat cvimg = img; //转为CvMat结构
    ```
    - [ ] **不可将 Mat 对象 提前释放。**
  - [ ] IplImage 和 CvMat 格式转为 Mat：
    - [ ] Mat 类有两个构造函数，可以实现 IplImage 和 CvMat 到 Mat 的转换。这两个函数都有一个参数 **copyData**。如果 copyData 的值是 **false**，那么 Mat 将与 IplImage 或 CvMat **共用同一矩阵数据，并且需要用户手动管理内存**；如果值是 **true**，Mat 会**新申请内存**，然后将 IplImage 或 CvMat 的数据复制到 Mat 的数据区。**建议设置为true**
    - [ ] 构造函数声明：
    ```C++
    Mat::Mat(const CvMat* m, bool copyData=false) 
    Mat::Mat(const IplImage* img, bool copyData=false)
    ```

# 第四章 数据获取与存储
## 4.1 读写图像文件
-  [ ] 读文件函数 `Mat imread(const string& filename, int flags=1 );`
  -  [ ] 读取失败返回 **NULL**。
  -  [ ] `Mat::empty()` 函数可以检查读取是否成功。
  -  [ ] 根据 **图像内容** 来确定文件读取的格式，而不是扩展名。
  -  [ ] 文件格式的支持需要特定的库。
-  [ ] 写文件函数：
  ```C++
  bool imwrite(const string& filename, InputArray image,
    const vector<int>& params=vector<int>())
  ```
  - [ ] 文件的格式由 filename 参数指定的 **文件扩展名确定**。
  - [ ] 第三个参数 params 可以指定文件格式的一些 **细节信息**。
    - [ ] JPEG:表示图像的质量，取值范围从 0 到 100。数值越大表示图像质量越高，当然文件也越大。默认值是 95。
    - [ ] PNG:表示压缩级别，取值范围是从 0 到 9。数值越大表示文件越小， 但是压缩花费的时间也越长。默认值是 3。
    - [ ] PPM，PGM 或 PBM:表示文件是以二进制还是纯文本方式存储，取值为 0 或 1。如果取值为 1，则表示以二进制方式存储。默认值是 1。
  - [ ] 并不是所有的 Mat 对象都可以存为图像文件，目前支持的格式只有 **8U 类型 的单通道和 3 通道(颜色顺序为 BGR)矩阵**；如果需要要保存 16U 格式图像，只能使用 PNG、JPEG 2000 和 TIFF 格式。如果希望将其他格式的矩阵保存为图像文件，可以先用 **Mat::convertTo()函数或者 cvtColor()函数** 将矩阵转为可以保存的格式。
  - [ ] 如果文件已经存在，imwrite()函数会 **直接覆盖**。

## 4.2 读写视频
**略**