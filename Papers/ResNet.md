### Deep Residual Learning for Image Recognition

> K. He, X. Zhang, S. Ren and J. Sun, "Deep Residual Learning for Image Recognition," 2016 IEEE Conference on Computer Vision and Pattern Recognition (CVPR), Las Vegas, NV, USA, 2016, pp. 770-778, doi: 10.1109/CVPR.2016.90.

**摘要**

更深的神经网络更难训练。我们提出了一个残差学习框架，以缓解比以前使用的网络要深得多的网络的训练。我们明确地将各层重新表述为学习参考层输入的残差函数，而不是学习未参考的函数。我们提供了全面的经验证据，表明这些残差网络更容易优化，并能从大大增加的深度中获得准确性。
在ImageNet数据集上，我们评估了深度为152层的残差网络，比VGG网络[41]深8倍，但仍然具有较低的复杂性。这些残差网络的组合在ImageNet测试集上取得了3.57%的误差。这一结果赢得了ILSVRC 2015分类任务的第一名。我们还介绍了对具有100层和1000层的CIFAR-10的分析。
表征的深度对于许多视觉识别任务来说是至关重要的。仅仅由于我们极深的表征，我们在COCO物体检测数据集上获得了28%的相对改进。深度残差网络是我们提交给ILSVRC和COCO 2015比赛的基础。在那里我们还赢得了ImageNet检测、ImageNet定位、COCO检测和COCO分割等任务的第一名。


## 1 Introduction

最近的证据显示，网络深度具有至关重要的意义。然而更深的网络将会引发臭名昭著的梯度消失/膨胀问题[1, 9]，它从一开始就阻碍了收敛性。由于归一化技术，梯度消失问题得到极大解决。但是仍然存在退化问题（degradation problem）。

文中通过引入深度残差模块解决退化问题。

## 2 Deep Residual Learning

### 2.1 残差学习

假设$H(x)$为最终要学习的映射，$x$是输入，让网络拟合$F(x)=H(x)-x$.如果卷积层后加Batch Normalization层，则不需要偏置项

残差$F(x)$与自身输入$x$维度必须一致才能实现逐元素相加

残差可以表现为多层的CNN，逐元素加法可以表现为两个feature maps逐通道相加

### 2.2 网络结构

**普通无残差**： 类似VGG，每个block内filter数不变，feature map大小减半时filter个数 X2，用步长为2的卷积执行下采样，Global Average Pooling取代全连接层，更少的参数和计算量防止过拟合

**残差网络**： 实线代表维度相同的直接相加，虚线代表出现了下采样，即步长为2的卷积

**残差分支出现下采样时**：
A方案：多出来的通道padding补0填充
B方案：用1X1卷积升维

![模型结构](https://pic1.zhimg.com/80/v2-563fa7324a16af7dd18c8db9a443a810_720w.webp)
