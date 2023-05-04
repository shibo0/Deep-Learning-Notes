![在这里插入图片描述](https://img-blog.csdnimg.cn/a17d58366dc54f618143930788eb9f69.png)

Going Deeper with Convolutions

```
C. Szegedy et al., "Going deeper with convolutions," 2015 IEEE Conference on Computer Vision and Pattern Recognition (CVPR), Boston, MA, USA, 2015, pp. 1-9, doi: 10.1109/CVPR.2015.7298594.
```

**背景**

《GoogLeNet-Going deeper with convolutions》是Google公司Inception系列的开山之作，在这篇文章中首次提出了Inception模块，后面的Inception v2&v3、Inception v4也是在这篇文章的基础上改进的。GoogLeNet是2014年ILSVRC挑战赛冠军，将Top5 的错误率降低到6.67%，是一个22层的深度网络。

> ILSVRC：大规模图像识别挑战赛
ImageNet Large Scale Visual Recognition Challenge 是李飞飞等人于2010年创办的图像识别挑战赛，自2010起连续举办8年，极大地推动计算机视觉发展。比赛项目涵盖：图像分类(Classification)、目标定位(Object localization)、目标检测(Object detection)、视频目标检测(Object detection from video)、场景分类(Scene classification)、场景解析(Scene parsing)。竞赛中脱颖而出大量经典模型： alexnet，vgg，googlenet，resnet，densenet等。

**摘要**

提出深度卷积神经网络结构Inception：

贡献1：ImageNet大规模视觉识别挑战赛2014（ILSVRC14）在分类（classification）和检测（detection）上取得最好结果。
贡献2：提高网络内部计算资源的利用率，增加网络深度（depth）和广度（width）并保持计算预算不变。
基础：赫布理论（Hebbian principle）和多尺度信息处理（multi-scale processing）

## Motivation and High Level Considerations

The most straightforward way of improving the performance of deep neural networks is by increasing their size. This includes both increasing the depth – the number of levels – of the network and its width: the number of units at each level.

However this simple solution comes with two major drawbacks:

1. Bigger size typically means a larger number of parameters, which makes the enlarged network more prone to overfitting, especially if the number of labeled examples in the training set is limited

2. Another drawback of uniformly increased network size is the dramatically increased use of computational resources.

## 架构细节

Inception架构的主要想法是考虑【怎样用密集模块来近似最优的局部稀疏结构 】

假设转换不变性，我们所需要做的是找到最优的局部构造并在空间上重复它。

![在这里插入图片描述](https://img-blog.csdnimg.cn/5c50f263917a4322bf67fdfd5c71f8fa.png)

不同大小的卷积核，意味着不同尺度特征的抽象融合；
1. 卷积核大小采用1、3和5，方便对齐。stride=1时，分别设定pad=0、1、2即可直接拼接在一起；
2. 并行池化；
3. 局部信息由1×1卷积核提取，靠前面的层提取局部信息；大范围空间信息由大卷积核提取，靠后面的层提取大范围空间信息。 网络越到后面，特征越抽象，随着层数的增加，3x3和5x5卷积的比例也要增加。

**问题**：在具有大量滤波器的卷积层之上，即使适量的5×5卷积也非常昂贵，池化层输出和卷积层输出的合并会导致这一阶段到下一阶段输出数量不可避免的增加，导致在几个阶段内计算量爆炸。

**改进**：在计算要求会增加太多的地方，明智而审慎地(judiciously)减少维度。

太过密集压缩的嵌入向量不便于模型处理。在3×3和5×5卷积之前，1×1卷积用来计算降维。

1×1卷积作用：

1. 升/降维度；
2. 跨通道信息交融；
3. 减少参数量/计算量；
4. 增加模型深度，提高非线性表达能力。

## GoogLeNet

GoogLeNe是在ILSVRC 2014竞赛的提交中使用的Inception架构的特例。

![在这里插入图片描述](https://img-blog.csdnimg.cn/26114b7e8f604dd1979fc4a487ab7cc7.png)

- 9个inception模块堆叠
- depth，共22层，算上池化层就27层
- 权重和计算量均匀分配给各层
- output size = #1×1 + #3×3 + #5×5 + pool proj
- 所有卷积都使用了修正线性激活(rectified linear activation，ReLU)
- avg pool：Global Average Pooling（全局平均池化），一个channel用一个平均值代表，取代FC层，减少参数量。优点：①便于迁移学习②top-1提高了0.6%的准确率
- 线性层：使网络能很容易地适应其它的标签集。

![在这里插入图片描述](https://img-blog.csdnimg.cn/ed910cd2115f4abfb751ace2fcebdeba.png)

## 训练方法

- 数据并行：一个batch均分k份，让不同节点前向和反向传播，再由中央param server优化更新权重
- 异步随机梯度下降：momentum=0.9，学习率每8次遍历下降4%
- 裁剪为原图的8%-100%，宽高比为3/4-4/3之间
- 一些图像增强技巧：等概率使用不同插值方法
