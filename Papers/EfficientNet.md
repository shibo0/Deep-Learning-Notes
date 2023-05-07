### EfficientNet: Rethinking Model Scaling for Convolutional Neural Networks

> Reference : Mingxing Tan, Quoc V. Le. EfficientNet: Rethinking Model Scaling for Convolutional Neural Networks. Proceedings of ICML, 2019.

### 1 Highlights

在本文中，作者研究了模型缩放，并认为仔细平衡网络的深度、宽度和分辨率可以获得更好的性能。

结果表明，这种平衡可以通过定比例缩放来实现。基于这一观察，他们提出了一个简单的复合缩放方法，相当于一个简单的神经结构搜索。

### 2 Methods

作者考虑以下三个神经网络元素：

- 深度：层数
- 宽度：每层神经元数量（和特征图feature maps）
- 分辨率：输入图片尺寸

他们承认这些变量以某种方式相互关联(例如在一个 ConvNet 中，提高分辨率会影响深度) ，因此提出了下面的复合系数 φ

![](https://vitalab.github.io/article/images/efficientnet/sc03.jpg)

这导致了他们简单的神经结构搜索算法，即:

![](https://vitalab.github.io/article/images/efficientnet/sc04.jpg)

baseline network:

![](https://vitalab.github.io/article/images/efficientnet/sc06.jpg)

### Results

![](https://vitalab.github.io/article/images/efficientnet/sc01.jpg)

![](https://vitalab.github.io/article/images/efficientnet/sc05.jpg)

### References

参考：https://vitalab.github.io/article/2020/11/05/EfficientNet.html

代码：https://github.com/tensorflow/tpu/tree/master/models/official/efficientnet
