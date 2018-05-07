---
ebook:
  title: OpenCV和Visual Studio图像识别应用开发-望熙兄弟
  authors: zhoulanlan
---
# 内容提要
- [ ] OpenCV是**跨平台的**、**提供多语言接口的库**，实现了图像处理和计算机视觉方面的很多**通用算法**。

# 第一章 系统安装与项目准备
## 1.1 认识OpenCV
- [ ] OpenCV主要包括**图像转换**、**图像处理**以及其他**数学运算处理功能**。
- [ ] OpenCV模块组：

|模块组|模块功能|
|:--:|--|
|core|数据类型、数据结构、内存管理|
|Highgui|读写图形文件、屏幕输入/输出处理、简单的UI功能|
|Imageproc|图形滤波、几何图像转换、形状分析|
|Calib3d|照相机校准、多视角3D重建|
|Feature2d|特征提取、描述与对比|
|Video|视频对象追踪与移动分析|
|Objdtect|使用级联式与方向梯度直方图分类器进行对象识别|
| ML |用于视频处理的统计模式与归类算法|
| Flann |用于高纬度数据的快速搜索|
| GPU |以选择的并行算法在GPU快速执行|
|Stitching|视频结合处理的方法：弯曲、混合、集束调整|
|Nonfree|有权利权的算法 （3.0已经删除）|

# 第二章 Core模块
## 2.1 显示图文件
- [ ] 文件应该放在`项目名称/项目名称`文件夹下面。
- [ ] 直接写入？与MFC或C++/CLI整合？
- [ ] 头文件：
  ```C++
  #include <opencv2/core/core.hpp>
  #include <opencv2/highgui/highgui.hpp>
  #include <iostream>
  using namespace cv;
  ```
- [ ] `Mat`定义了图像的**矩阵**类。
- [ ] `Point`代表二维点：
  ```C++
  Point pt;
  pt.x = 10;
  pt.y = 8;
  //或者
  Point pt(10,8);
  ```
- [ ] `Scalar`代表四元素向量，用于RGB颜色。`Scalar(蓝,绿,红,参数三可以不写);`。
- [ ] `Mat image; image = imread(char* fileName,CV_LOAD_IMAGE_COLOR);`可以载入图像。
- [ ] 展示图像：
  ```C++
  namedWindow("Display Window",CV_WINDOW_NORMAL);
  //CV_WINDOW_FREERATIO与CV_WINDOW_KEEPRATIO
  //CV_GUI_NORMAL与CV_GUI_EXPANDED

  //窗口内显示图像
  imshow("Display Window",image)；

  //窗口等待按键
  waitKey(0);
  ```

| 语法 | 说明 |
| :--: | :-- |
|`M.at<double>(i,j)`| 取得i行，j列的值，行列的初始值为0|
|`M.row(1)`|取得第1行的值|
|`M.col(3)`|取得第3列的值|
|`M.rowRange(1,4)`|取第1行到第四行的值|
|`M.colRange(1,4)`|取第1列到第四列的值|
|`M.rowRange(2,5).colRange(1,3)`|取第2行到第5行以及第1列到第3列的值|
|`M.diag()`|取得**对角值**|
|`Mat M2 = M1.clone()`|将M1**复制**给M2|
|`Mat M1; M1.copyTo(M2)`|将M1**复制**给M2|
|`Mat M2 = M1.t()`|M2是M1的**转置**(tanspose)|
|`Mat M2 = M1.inv()`|M2是M1的逆矩阵(inverse)|
|`Scalar s; Mat M2 = M1 + s;`|M2是M1加上s颜色值|
|`Mat image; image.size().height;image.size().width;`|Image矩阵的高和宽|

- [ ] 在函数传递时，Mat矩阵**默认是传址**的方式。
- [ ] OpenCV采用C++的引用计数来做内存管理，我们不用手动管理OpenCV的对象。
- [ ] `imread(const string& filename,int flags);`读取图文件。**flags**可以为：
  - [ ] `CV_LOAD_IMAGE_UNCHANGED`或者`flags`小于0，以原始文件形态读入。
  - [ ] `CV_LOAD_GRAYSCALE`或者`flags`等于0，以黑白方式读取。
  - [ ] `CV_LOAD_COLOR`或者`flags`大于0，以彩色方式读取。
  - [ ] 判断文件是否读取失败：
```C++
if(!image.data)
{
  cout<<"失败"<<endl;
  return -1;
}
```

## 2.2 图文件转换
- [ ] `cvtColor(InputArray src,OutputArray dst,int code,int dstCn = 0)`图像颜色空间转换。
  - [ ] `dstCn`是输出图像通道，默认值0表示由输入图像和颜色空间`code`**自动获取**。
- [ ] `imwrite(const string& filename,inputArray img);`存储图像

## 2.3 图文件混合
- [ ] `addWeighted(InputArray src1,double alpha,inputArray src2,double beta,double gamma,output dst,int dtype = 1);`以权重将**两个图合并**，可以用来**增加水印**。
  - [ ] `alpha`第一个图文件的权重，0~1&&<1
  - [ ] `beta`第二个图文件的权重，0~1&&<1
  - [ ] `gamma`两个图相加之后还要相加的值
  - [ ] `dtype`景深
  - [ ] 合并公式：`dst = alpha*src1+beta*src2+gamma`
- [ ] `resize(InputArray src, OutputArray dst, Size dsize, double fx = 0, double fx = 0, double fy = 0, int interpolation=InTER_LINEAR);`改变图像大小。
  - [ ] `dsize`输出图像的大小，如果等于0，用`dsize = Size(round(fx*src.cols),round(fy*src.rows));`计算,`fx`与`fy`不可都等于0.
  - [ ] `fx`：水平轴的缩放因子。
  - [ ] `fy`：垂直轴缩放因子。
  - [ ] `interpolation`插值方式：
- [ ] `size Mat::size() const;`返回矩阵大小。
- [ ] `size(rows,cols);`也返回矩阵大小。
- [ ] `add(InputArray src1,InputArray src2,OutputArray dst,InputArray mask=noArray(),int dtype=-1);`计算两**图像**或**颜色值**相加。
  - [ ] `mask：`掩码（mask），可有可无。
  - [ ] `dtype：`输出图像景深，可有可无。
- [ ] 像素是由**颜色空间**（color space）或**通道**（channel）与数据类型（data type）来描述。
- [ ] OpenCV的数据类型**CV_ABCD**：
  - [ ] **A**：每个像素**多少位**。
  - [ ] **B**：是否有**正负号**。
  - [ ] **C**：**类型前置码**，表示每个通道用什么类型表示。
  - [ ] **D**：每个像素**通道数目**。
- [ ] HSV与HLS：系统将颜色分为**色调**（Hue）、**饱和度**（saturation）以及**明亮度**（luminance）。
- [ ] YCrCb：主要用于**JPEG**图像。
- [ ] CIEL*a*b**：概念上的色彩空间，用来计算色距（distance of color）。
- [ ] **褪色**的目的是为了**加快**图文件的扫描速度和处理速度。