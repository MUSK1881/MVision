# 图像处理
## 形态学操作 腐蚀 膨胀  开闭运算

      形态学操作就是基于形状的一系列图像处理操作。
      通过将 结构元素 作用于输入图像来产生输出图像。
      最基本的形态学操作有二：
      腐蚀与膨胀(Erosion 与 Dilation)。 
      他们的运用广泛:
          消除噪声
          分割(isolate)独立的图像元素，以及连接(join)相邻的元素。
          寻找图像中的明显的极大值区域或极小值区域。 连通域
      【1】膨胀Dilation
      选择核内部的最大值(值越大越亮 约白)
      此操作将图像 A 与任意形状的内核 (B)，通常为正方形或圆形,进行卷积。
      内核 B 有一个可定义的 锚点, 通常定义为内核中心点。
      进行膨胀操作时，将内核 B 划过图像,将内核 B 覆盖区域的最大相素值提取，
      并代替锚点位置的相素。显然，这一最大化操作将会导致图像中的亮区开始”扩展” 
      (因此有了术语膨胀 dilation )。
      背景(白色)膨胀，而黑色字母缩小了。
      【2】腐蚀 Erosion 
      选择核内部的最小值(值越小越暗 约黑)
      腐蚀在形态学操作家族里是膨胀操作的孪生姐妹。它提取的是内核覆盖下的相素最小值。
      进行腐蚀操作时，将内核 B 划过图像,将内核 B 覆盖区域的最小相素值提取，并代替锚点位置的相素。
      以与膨胀相同的图像作为样本,我们使用腐蚀操作。
      从下面的结果图我们看到亮区(背景)变细，而黑色区域(字母)则变大了。
      
[膨胀 腐蚀](shape_oper_eroding_dilating.cpp)

## 开闭运算提取线特征  水平线 垂直线
    利用形态学操作提取水平线和垂直线
    腐蚀膨胀来提取线特征
[开闭运算提取线特征](extract_horizontal_vertical_lines.cpp)

## 图像阈值处理  阈值二值化
    图像阈值操作
    最简单的图像分割的方法。
    应用举例：从一副图像中利用阈值分割出我们需要的物体部分
    （当然这里的物体可以是一部分或者整体）。
    这样的图像分割方法是基于图像中物体与背景之间的灰度差异，而且此分割属于像素级的分割。
    为了从一副图像中提取出我们需要的部分，应该用图像中的每一个
    像素点的灰度值与选取的阈值进行比较，并作出相应的判断。
    （注意：阈值的选取依赖于具体的问题。即：物体在不同的图像中有可能会有不同的灰度值。
    一旦找到了需要分割的物体的像素点，我们可以对这些像素点设定一些特定的值来表示。
    （例如：可以将该物体的像素点的灰度值设定为：‘0’（黑色）,
    其他的像素点的灰度值为：‘255’（白色）；当然像素点的灰度值可以任意，
    但最好设定的两种颜色对比度较强，方便观察结果）。
    【1】阈值类型1：二值阈值化
         大于阈值的 设置为最大值 255  其余为0
         先要选定一个特定的阈值量，比如：125，
        这样，新的阈值产生规则可以解释为大于125的像素点的灰度值设定为最大值(如8位灰度值最大为255)，
        灰度值小于125的像素点的灰度值设定为0。
    【2】阈值类型2：反二进制阈值化
        小于阈值的 设置为最大值 255  其余为0
    【3】阈值类型3：截断阈值化
        大于阈值的 设置为阈值  其余保持原来的值
    【4】阈值类型4：阈值化为0
        大于阈值的 保持原来的值 其余设置为0
    【5】阈值类型5：反阈值化为0 
        大于阈值的 设置为0  其余保持原来的值 
[图像阈值操作](threshold.cpp)

## 图像卷积操作  平滑滤波  自定义滤波器算子  sobel求梯度算子
      Sobel 算子x方向、y方向导数 合成导数
      一个最重要的卷积运算就是导数的计算(或者近似计算).
      为什么对图像进行求导是重要的呢? 假设我们需要检测图像中的 边缘 球图像梯度大的地方
      【1】Sobel算子
        Sobel 算子是一个离散微分算子 (discrete differentiation operator)。 
              它用来计算图像灰度函数的近似梯度。
        Sobel 算子结合了高斯平滑和微分求导。
      假设被作用图像为 I:
          在两个方向求导:
              水平变化: 将 I 与一个奇数大小的内核 G_{x} 进行卷积。比如，当内核大小为3时, G_{x} 的计算结果为:
          G_{x} = [-1 0 +1
             -2 0 +2
             -1 0 +1]
              垂直变化: 将:m I 与一个奇数大小的内核 G_{y} 进行卷积。
                        比如，当内核大小为3时, G_{y} 的计算结果为:
          G_{y} = [-1 -2 -1
              0  0  0
             +1 +2 +1]
             在图像的每一点，结合以上两个结果求出近似 梯度:
                   sqrt（GX^2 + GY^2）
      Sobel内核
      当内核大小为 3 时, 以上Sobel内核可能产生比较明显的误差(毕竟，Sobel算子只是求取了导数的近似值)。
       为解决这一问题，OpenCV提供了 Scharr 函数，但该函数仅作用于大小为3的内核。
      该函数的运算与Sobel函数一样快，但结果却更加精确，其内核为:
          G_{x} = [-3  0 +3
             -10 0 +10
             -3  0 +3]
          G_{y} = [-3 -10 -3
              0  0  0
             +3 +10 +3]
[Sobel 算子](Sobel_ker.cpp)

## 图像金字塔  下采样 + 高斯核卷积
      图像金字塔
      使用OpenCV函数 pyrUp 和 pyrDown 对图像进行向上和向下采样。
      然后高斯平滑
      当我们需要将图像转换到另一个尺寸的时候， 有两种可能：
          放大 图像 或者
          缩小 图像。
      我们首先学习一下使用 图像金字塔 来做图像缩放, 图像金字塔是视觉运用中广泛采用的一项技术。
      图像金字塔：
          一个图像金字塔是一系列图像的集合 - 
      所有图像来源于同一张原始图像 - 通过梯次向下采样获得，直到达到某个终止条件才停止采样。
      有两种类型的图像金字塔常常出现在文献和应用中:
         【1】 高斯金字塔(Gaussian pyramid): 用来向下采样
         【2】 拉普拉斯金字塔(Laplacian pyramid): 用来从金字塔低层图像重建上层未采样图像
      在这篇文档中我们将使用 高斯金字塔 。
      高斯金字塔：想想金字塔为一层一层的图像，层级越高，图像越小。
      高斯内核:
      1/16 [1  4  6  4  1
            4  16 24 16 4
            6  24 36 24 6
            4  16 24 16 4
            1  4  6  4  1]
      下采样：
          将 图像 与高斯内核做卷积 
          将所有偶数行和列去除。
          显而易见，结果图像只有原图的四分之一。
      如果将图像变大呢?:
          首先，将图像在每个方向扩大为原来的两倍，新增的行和列以0填充(0)
          使用先前同样的内核(乘以4)与放大后的图像卷积，获得 “新增像素” 的近似值。
      这两个步骤(向下和向上采样) 分别通过OpenCV函数 pyrUp 和 pyrDown 实现, 

[图像金字塔](pyramid.cpp)

## Laplacian 算子 的离散模拟。 图像二阶导数  梯度的梯度 0值的话 边缘概率较大

      Laplacian 算子 的离散模拟。 图像二阶导数  梯度的梯度 0值的话 边缘概率较大
      Sobel 算子 ，其基础来自于一个事实，即在边缘部分，像素值出现”跳跃“或者较大的变化。
      
      如果在此边缘部分求取一阶导数，你会看到极值的出现。
      
      你会发现在一阶导数的极值位置，二阶导数为0。
      
      所以我们也可以用这个特点来作为检测图像边缘的方法。
      
       但是， 二阶导数的0值不仅仅出现在边缘(它们也可能出现在无意义的位置),
      但是我们可以过滤掉这些点。
      Laplacian 算子
          从以上分析中，我们推论二阶导数可以用来 检测边缘 。 
          因为图像是 “2维”, 我们需要在两个方向求导。使用Laplacian算子将会使求导过程变得简单。
          Laplacian 算子 的定义:
          L Laplace(f) = df^2/ dx^2 + df^2 / dy^2
          OpenCV函数 Laplacian 实现了Laplacian算子。 
          实际上，由于 Laplacian使用了图像梯度，它内部调用了 Sobel 算子。
          
[ Laplacian ](laplacian.cpp)


## candy边缘检测  消除噪声  计算梯度幅值和方向   非极大值 抑制  滞后阈值

      Canny 边缘检测 边缘检测最优算法
      综合使用 高斯平滑 soble梯度检测 非极大值抑制 滞后阈值 等操作来检测物体边缘
      Canny 边缘检测算法 是 John F. Canny 于 1986年开发出来的一个多级边缘检测算法，
      也被很多人认为是边缘检测的 最优算法, 最优边缘检测的三个主要评价标准是:
              低错误率: 标识出尽可能多的实际边缘，同时尽可能的减少噪声产生的误报。
              高定位性: 标识出的边缘要与图像中的实际边缘尽可能接近。
              最小响应: 图像中的边缘只能标识一次。
      步骤：:
       【1】消除噪声。 使用高斯平滑滤波器卷积降噪。 下面显示了一个 size = 5 的高斯内核示例:
          K = 1/159 [2  4  5  4  2
               4  9  12 9  4
               5 12  15 12 5
                 4  9  12 9  4
               2  4  5  4  2]
       【2】计算梯度幅值和方向。 
             此处，按照Sobel滤波器的步骤:
          在两个方向求导:
              水平变化: 将 I 与一个奇数大小的内核 G_{x} 进行卷积。比如，当内核大小为3时, G_{x} 的计算结果为:
          G_{x} = [-1 0 +1
             -2 0 +2
             -1 0 +1]
              垂直变化: 将:m I 与一个奇数大小的内核 G_{y} 进行卷积。
                        比如，当内核大小为3时, G_{y} 的计算结果为:
          G_{y} = [-1 -2 -1
              0  0  0
             +1 +2 +1]
             在图像的每一点，结合以上两个结果求出近似 梯度:
                    G = sqrt（GX^2 + GY^2）
                    梯度角度方向 = arctan(GY/GX)
                    梯度方向近似到四个可能角度之一(一般 0, 45, 90, 135)
        【3】非极大值 抑制。 这一步排除非边缘像素， 仅仅保留了一些细线条(候选边缘)。
        【4】滞后阈值: 最后一步，Canny 使用了滞后阈值，滞后阈值需要两个阈值(高阈值和低阈值):
          如果某一像素位置的幅值超过 高 阈值, 该像素被保留为边缘像素。
          如果某一像素位置的幅值小于 低 阈值, 该像素被排除。
          如果某一像素位置的幅值在两个阈值之间,该像素在连接到一个高于 高阈值的像素时被保留。
      Canny 推荐的 高:低 阈值比在 2:1 到3:1之间。

[candy边缘检测](canny_Edge_Detector.cpp)


## 霍夫线变换 霍夫圆变换  先candy边缘检测 再找圆  再找直线
    霍夫 圆变换   在图像中检测圆. 
[霍夫圆变换](hough_Circle_Transform.cpp)

    霍夫线变换  检测图像中的直线 先candy边缘检测 在找直线
    使用OpenCV的以下函数 HoughLines 和 HoughLinesP 来检测图像中的直线.
    霍夫线变换
        霍夫线变换是一种用来寻找直线的方法.
        是用霍夫线变换之前, 首先要对图像进行边缘检测的处理，
        也即霍夫线变换的直接输入只能是边缘二值图像.
    众所周知, 一条直线在图像二维空间可由两个变量表示. 例如:
        在 笛卡尔坐标系: 可由参数: (m,b) 斜率和截距表示.  y = m*x + b
        在 极坐标系: 可由参数: (r,\theta) 极径和极角表示  r = x * cos(theta) + y*sin(theta)
       一般来说, 一条直线能够通过在平面 theta - r 寻找交于一点的曲线数量来 检测. 
       越多曲线交于一点也就意味着这个交点表示的直线由更多的点组成. 
       一般来说我们可以通过设置直线上点的 阈值 来定义多少条曲线交于一点我们才认为 检测 到了一条直线.
       这就是霍夫线变换要做的. 它追踪图像中每个点对应曲线间的交点. 
       如果交于一点的曲线的数量超过了 阈值, 
       那么可以认为这个交点所代表的参数对 (theta, r_{theta}) 在原图像中为一条直线.
    标准霍夫线变换和统计概率霍夫线变换
    OpenCV实现了以下两种霍夫线变换:
        【1】标准霍夫线变换
      它能给我们提供一组参数对 (\theta, r_{\theta}) 的集合来表示检测到的直线
            在OpenCV 中通过函数 HoughLines 来实现
        【2】统计概率霍夫线变换
            这是执行起来效率更高的霍夫线变换. 
            它输出检测到的直线的端点 (x_{0}, y_{0}, x_{1}, y_{1})
            在OpenCV 中它通过函数 HoughLinesP 来实现

[霍夫线变换](hough_Line_Transform.cpp)


## 图像像素重映射 remapping  随机矩阵空间映射  仿射变换  

    重映射是什么意思?
        把一个图像中一个位置的像素放置到另一个图片指定位置的过程.
        为了完成映射过程, 有必要获得一些插值为非整数像素坐标,
        因为源图像与目标图像的像素坐标不是一一对应的.
        我们通过重映射来表达每个像素的位置 (x,y) :
       goal(x,y) = f(s(s,y))

[霍夫线变换](remapping.cpp)


## 求得对直方图均衡化的映射矩阵 在对原图像进行映射 图像直方图计算  反向投影 利用直方图模型 搜索 对应的 图像区域

    求得对直方图均衡化的映射矩阵 在对原图像进行映射
    图像的直方图是什么?
        直方图是图像中像素强度分布的图形表达方式.
        它统计了每一个强度值（灰度 0~255 256个值）所具有的像素点个数.
    直方图均衡化是什么?
        直方图均衡化是通过拉伸像素强度分布范围来增强图像对比度的一种方法.
        说得更清楚一些, 以上面的直方图为例, 你可以看到像素主要集中在中间的一些强度值上.
     直方图均衡化要做的就是 拉伸 这个范围. 见下面左图: 绿圈圈出了 少有像素分布其上的 强度值. 
    对其应用均衡化后, 得到了中间图所示的直方图. 均衡化的图像见下面右图.
    直方图均衡化是怎样做到的?
        均衡化指的是把一个分布 (给定的直方图) 映射 
        到另一个分布 (一个更宽更统一的强度值分布), 所以强度值分布会在整个范围内展开.
        要想实现均衡化的效果, 映射函数应该是一个 累积分布函数 (cdf) 
[直方图均衡化](histogram_Equalization.cpp)

    图像的直方图计算
    单个通道内的像素值进行统计 
    什么是直方图?
        直方图是对数据的集合 统计 ，并将统计结果分布于一系列预定义的 bins 中。
        这里的 数据 不仅仅指的是灰度值 (如上一篇您所看到的)，
         统计数据可能是任何能有效描述图像的特征。
        先看一个例子吧。 假设有一个矩阵包含一张图像的信息 (灰度值 0-255):
    OpenCV提供了一个简单的计算数组集(通常是图像或分割后的通道)
    的直方图函数 calcHist 。 支持高达 32 维的直方图。
[直方图计算](histogram_Calculation.cpp)

    直方图对比
        如何使用OpenCV函数 compareHist 产生一个表达两个直方图的相似度的数值。
        如何使用不同的对比标准来对直方图进行比较。
    要比较两个直方图( H_{1} and H_{2} ), 
    首先必须要选择一个衡量直方图相似度的 对比标准 (d(H_{1}, H_{2})) 。
    OpenCV 函数 compareHist 执行了具体的直方图对比的任务。
    该函数提供了4种对比标准来计算相似度：
    【1】相关关系 Correlation ( CV_COMP_CORREL )  与均值的偏差 积
    【2】平方差  Chi-Square ( CV_COMP_CHISQR )
    【3】交集？Intersection ( CV_COMP_INTERSECT ) 对应最小值纸盒
    【4】Bhattacharyya 距离( CV_COMP_BHATTACHARYYA ) 巴氏距离（巴塔恰里雅距离 / Bhattacharyya distance）
    本程序做什么?
        装载一张 基准图像 和 两张 测试图像 进行对比。
        产生一张取自 基准图像 下半部的图像。
        将图像转换到HSV格式。
        计算所有图像的H-S直方图，并归一化以便对比。
        将 基准图像 直方图与 两张测试图像直方图，基准图像半身像直方图，以及基准图像本身的直方图分别作对比。
        显示计算所得的直方图相似度数值。
[直方图对比](histogram_Comparison.cpp)


## 在图像中寻找轮廓  candy边缘检测  再找物体轮廓



## 计算物体的凸包  边缘包围圈
    计算物体的凸包  边缘包围圈
    对图像进行二值化     candy边缘检测也得到 二值图
    寻找轮廓 
    对每个轮廓计算其凸包
    绘出轮廓及其凸包
    
[计算物体的凸包  边缘包围圈](convex_Hull.cpp)

## 创建包围轮廓的矩形和圆形边界框
    创建包围轮廓的矩形和圆形边界框
    使用Threshold检测边缘  二值化 阈值 thresh
    找到轮廓  findContours
    
[创建包围轮廓的矩形和圆形边界框](creating_Bounding_boxes_circles.cpp)


## 为轮廓创建可倾斜的边界框和椭圆

    为轮廓创建可倾斜的边界框和椭圆
[为轮廓创建可倾斜的边界框和椭圆](bounding_rotated_boxe_ellipses.cpp)    

## 轮廓矩
      轮廓矩
      因为我们常常会将随机变量（先假定有任意阶矩）作一个线性变换，
      把一阶矩（期望）归零，
      二阶矩（方差）归一，以便统一研究一些问题。
      三阶矩，就是我们所称的「偏度」。
          典型的正偏度投资，就是彩票和保险：
            一般来说，你花的那一点小钱就打水漂了，但是这一点钱完全是在承受范围内的；
             而这点钱则部分转化为小概率情况下的巨大收益。
          而负偏度变量则正好相反，「一般为正，极端值为负」，
            可以参照一些所谓的「灰色产业」：
            一般情况下是可以赚到一点钱的，但是有较小的概率「东窗事发」，赔得血本无归。
      四阶矩，又称峰度，简单来说相当于「方差的方差」，
      和偏度类似，都可以衡量极端值的情况。峰度较大通常意味着极端值较常出现，
      峰度较小通常意味着极端值即使出现了也不会「太极端」。
      峰度是大还是小通常与3（即正态分布的峰度）相比较。
      至于为什么五阶以上的矩没有专门的称呼，主要是因为我们习惯的线性变换，
      只有两个自由度，故最多只能将前两阶矩给「标准化」。
      这样，标准化以后，第三、第四阶的矩就比较重要了，前者衡量正负，
      后者衡量偏离程度，与均值、方差的关系类似。换句话说，
      假如我们能把前四阶矩都给「标准化」了，那么五阶、六阶的矩就会比较重要了吧。
      
[轮廓矩](ccontours_moments.cpp)

## 基于距离变换和分水岭算法的图像匹配
    图像 模板匹配  是一项在一幅图像中寻找与另一幅模板图像最匹配(相似)部分的技术.
    使用OpenCV函数 matchTemplate 在模板块和输入图像之间寻找匹配, 获得匹配结果图像
    使用OpenCV函数 minMaxLoc 在给定的矩阵(上述得到的匹配结果矩阵)中寻找最大和最小值(包括它们的位置).
    什么是模板匹配?
    模板匹配是一项在一幅图像中寻找与另一幅模板图像最匹配(相似)部分的技术.
    我们需要2幅图像:
        原图像 (I): 在这幅图像里,我们希望找到一块和模板匹配的区域
        模板 (T): 将和原图像比照的图像块
    我们的目标是检测最匹配的区域:
    为了确定匹配区域, 我们不得不滑动模板图像和原图像进行 比较 :
    通过 滑动, 我们的意思是图像块一次移动一个像素 (从左往右,从上往下). 
    在每一个位置, 都进行一次度量计算来表明它是 “好” 或 “坏” 地与那个位置匹配 (或者说块图像和原图像的特定区域有多么相似).
    对于 T 覆盖在 I 上的每个位置,你把度量值 保存 到 结果图像矩阵 (R) 中. 在 R 中的每个位置 (x,y) 都包含匹配度量值:
    上图就是 TM_CCORR_NORMED 方法处理后的结果图像 R . 最白的位置代表最高的匹配. 正如您所见, 红色椭圆框住的位置很可能是结果图像矩阵中的最大数值, 所以这个区域 (以这个点为顶点,长宽和模板图像一样大小的矩阵) 被认为是匹配的.
    实际上, 我们使用函数 minMaxLoc 来定位在矩阵 R 中的最大值点 (或者最小值, 根据函数输入的匹配参数) .
    OpenCV中支持哪些匹配算法
    【1】 平方差匹配 method=CV_TM_SQDIFF  square dirrerence(error)
         这类方法利用平方差来进行匹配,最好匹配为0.匹配越差,匹配值越大.
    【2】标准平方差匹配 method=CV_TM_SQDIFF_NORMED  standard  square dirrerence(error)
    【3】 相关匹配 method=CV_TM_CCORR
         这类方法采用模板和图像间的乘法操作,所以较大的数表示匹配程度较高,0标识最坏的匹配效果.
    【4】 标准相关匹配 method=CV_TM_CCORR_NORMED
    【5】 相关匹配 method=CV_TM_CCOEFF
         这类方法将模版对其均值的相对值与图像对其均值的相关值进行匹配,1表示完美匹配,
         -1表示糟糕的匹配,0表示没有任何相关性(随机序列).
    【6】标准相关匹配 method=CV_TM_CCOEFF_NORMED
    通常,随着从简单的测量(平方差)到更复杂的测量(相关系数),
    我们可获得越来越准确的匹配(同时也意味着越来越大的计算代价). 
    最好的办法是对所有这些设置多做一些测试实验,
    以便为自己的应用选择同时兼顾速度和精度的最佳方案.
    在这程序实现了什么?
        载入一幅输入图像和一幅模板图像块 (template)
        通过使用函数 matchTemplate 实现之前所述的6种匹配方法的任一个. 用户可以通过滑动条选取任何一种方法.
        归一化匹配后的输出结果
        定位最匹配的区域
        用矩形标注最匹配的区域


[基于距离变换和分水岭算法的图像匹配](template_Matching.cpp)
