Very Deep Convolutional Networks for Large-Scale Image Recognition

```
S. Liu and W. Deng, "Very deep convolutional neural network based image classification using small training sample size," 2015 3rd IAPR Asian Conference on Pattern Recognition (ACPR), Kuala Lumpur, Malaysia, 2015, pp. 730-734, doi: 10.1109/ACPR.2015.7486599.
```

[pdf](https://arxiv.org/pdf/1409.1556v6.pdf)

**背景**

2014年，牛津大学计算机视觉组（Visual Geometry Group）和Google DeepMind公司一起研发了新的卷积神经网络，并命名为VGGNet。VGGNet是比AlexNet更深的深度卷积神经网络，该模型获得了2014年ILSVRC竞赛的第二名，第一名是GoogLeNet

**摘要**

这篇工作探究了卷积网络深度在大规模数据集上的性能表现。主要贡献是彻底评估了使用非常小的（3×3）卷积滤波器的网络，这表明将深度提高到16-19层就可以实现对现有技术的显著改进。

## 1 Introduction

由于GPU等高性能计算系统和大规模开源数据集，卷积神经网络在图像和视频识别取得巨大的成功。

在这篇文章中，作者讨论了卷积网络架构的一个重要方面-`深度`。

## 2 ConvNet Configurations

训练过程，固定输入为224\*224 RGB图片。唯一的预处理是减去由训练集计算的平均RGB值。在卷积网络中，作者应用3\*3的接受野以及1\*1的接受野，3\*3的接受野是一个最小的尺寸，可以捕捉到上下、左右、中间的概念，1\*1的接受野可以看作一个输入通道的线性转换，后接非线性。卷积步伐定为1像素。最大池化以2\*2像素窗执行，步伐为2.堆叠的卷积层之后跟着三个全连接层，所有隐藏层配置ReLU非线性激活函数，没有应用局部相应归一化LRN。

卷积层的宽度（通道的数量）相当小，从第一层的64开始，然后在每个maxpooling层之后增加2倍，直到达到512。在每个maxpooling之后增加2倍，直到达到512。

**两个堆叠的3\*3卷积层有一个有效的5\*5的接受野，三个堆叠的3\*3卷积层有一个有效的7\*7的接受野。**
用三个3\*3卷积取代一个7\*7卷积，将会使得决策函数更具有区分性。同时参数量减少81%。（
$3^2C^2=27^2C^2, 7^2C^2=49C^2$）纳入1×1卷积层（配置C，表1）是增加决策函数的非线性而不影响卷积层的感受野的一种方法。

网络配置如下：

![在这里插入图片描述](https://img-blog.csdnimg.cn/3dadaa05c05546a48148cda15f5224e9.png)

## 3 Classification Framework

### 训练测试细节

训练通常遵循AlexNet训练方法。训练使用mini-batch梯度下降，batchsize256，动量0.9，L2正则化5\*10-4，dropout设置在前两个全连接层后边（dropout rate0.5）。初始学习率10-2，当验证集停止更新是下降1/10，训练总共74epochs。

数据增强：随机水平翻转和随机RGB颜色变换。

训练图片尺度S的探究：固定S=256和S=384.尺度抖动，从[Smin, Smax]中随机采样S，Smin=256，Smax=512.

测试图片尺度Q：Q不必和训练尺度相同。原图片和翻转图片经过模型softmax平均得到最终的分数。


## 4 实验结果

单测试尺度性能

![单测试尺度](https://img-blog.csdnimg.cn/c2354b86d3394134bc760bbbfc1da483.png)

多测试尺度性能

![在这里插入图片描述](https://img-blog.csdnimg.cn/b0c8914da19d4d0b9cb13751cc9cad30.png)

ConvNet评估技术比较

![在这里插入图片描述](https://img-blog.csdnimg.cn/ec2ac7e71ea54013b46bde0f1f0eae21.png)

和其他模型比较

![在这里插入图片描述](https://img-blog.csdnimg.cn/4327ec2bd6284af4909bb5bdc37e5606.png)


[PyTorch-VGG](https://github.com/Lornatang/VGG-PyTorch)
