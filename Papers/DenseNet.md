### Densely Connected Convolutional Networks

> G. Huang, Z. Liu, L. Van Der Maaten and K. Q. Weinberger, "Densely Connected Convolutional Networks," 2017 IEEE Conference on Computer Vision and Pattern Recognition (CVPR), Honolulu, HI, USA, 2017, pp. 2261-2269, doi: 10.1109/CVPR.2017.243.

**摘要**

最近的工作表明，如果卷积网络在靠近输入层和靠近输出层之间包含较短的连接，那么卷积网络可以大大加深，更准确，更高效地训练。文章应用这一原理，引入DenseNet。DenseNet有几个引人注目的优点：它们缓解了梯度消失的问题，加强了特征传播，鼓励特征重用，并大大减少了参数的数量。

**设计理念**

相比ResNet，DenseNet提出了一个更激进的密集连接机制：即互相连接所有的层，具体来说就是每个层都会接受其前面所有层作为其额外的输入。图1为ResNet网络的连接机制，作为对比，图2为DenseNet的密集连接机制。可以看到，ResNet是每个层与前面的某层（一般是2~3层）短路连接在一起，连接方式是通过元素级相加。而在DenseNet中，每个层都会与前面所有层在channel维度上连接（concat）在一起（这里各个层的特征图大小是相同的，后面会有说明），并作为下一层的输入。对于一个L层的网络，DenseNet共包含L(L+1)/2个连接，相比ResNet，这是一种密集连接。而且DenseNet是直接concat来自不同层的特征图，这可以实现特征重用，提升效率，这一特点是DenseNet与ResNet最主要的区别。

![图1 ResNet网络的短路连接机制](https://pic3.zhimg.com/80/v2-862e1c2dcb24f10d264544190ad38142_720w.webp)

![图2 DenseNet网络的密集连接机制](https://pic3.zhimg.com/80/v2-2cb01c1c9a217e56c72f4c24096fe3fe_720w.webp)


## 1. DenseNets

文章提到ResNet的相加机制有可能阻碍信息流。

**DenseNet**:为了提高层与层之间的信息流，文中提出一个新的连接模式（图1）.

图1中每一层定义为复合函数，包含Conv-ReLU-BN。多个复合函数合并成一个Dense模块。每个模块包含一个生长率(Growth rate)，例如图1的生长率为4，它决定了Dense模块的大小。

![在这里插入图片描述](https://img-blog.csdnimg.cn/0e7b35554bfb4f239288cd0f29d8bdf5.png)

![在这里插入图片描述](https://img-blog.csdnimg.cn/d4fc35a7550c4da9bd9cc2fdb53fc9a2.png)
