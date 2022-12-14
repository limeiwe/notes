# 2.5 CNN网络实战技巧

## 学习目标

- 目标
  - 了解迁移学习以及技巧
- 应用
  - 无

我们来看一个个问题如果我们要做一个具体场景的计算机视觉任务，那么从头开始训练一个网络是合适的选择吗？怎么样才能避免浪费过多的计算时间？

### 2.5.1 迁移学习(Transfer Learning)

#### 2.5.1.1 介绍

* 定义
  * 迁移学习就是**利用数据、任务或模型之间的相似性，将在旧的领域学习过或训练好的模型，应用于新的领域这样的一个过程。**
  * 两个任务的输入属于同一性质：要么同是图像、要么同是语音或其他

迁移学习到底在什么情况下使用呢？有两个方面需要我们考虑的

- 1、当我们有海量的数据资源时，可以不需要迁移学习，**机器学习系统很容易从海量数据中学习到一个鲁棒性很强的模型。**但通常情况下，我们需要研究的领域可获得的数据极为有限，在少量的训练样本上精度极高，但是泛化效果极差。
- 2、训练成本，很少去从头开始训练一整个深度卷积网络，从头开始训练一个卷积网络通常需要较长时间且依赖于强大的 GPU 计算资源。

#### 2.5.1.2 方法

* 最常见的称呼叫做fine tuning,即微调
  * 已训练好的模型，称之为Pre-trained model

通常我们需要加载以训练好的模型，这些可以是一些机构或者公司在ImageNet等类似比赛上进行训练过的模型。TensorFlow同样也提供了相关模型地址：https://github.com/tensorflow/models/tree/master/research/slim

下图是其中包含的一些模型：

![](../images/预训练模型.png)

#### 2.5.1.3 过程

这里我们举一个例子，假设有两个任务A和B，任务 A 拥有海量的数据资源且已训练好，但并不是我们的目标任务，任务 B 是我们的目标任务。下面的网络模型假设是已训练好的1000个类别模型

![](../images/迁移过程1.png)

而B任务假设是某个具体场景如250个类别的食物识别，那么该怎么去做

* 1、建立自己的网络，在A的基础上，修改最后输出结构，并加载A的模型参数
* 2、根据数据大小调整
  * 如果B任务数据量小，那么我们可以选择将A模型的所有的层进行freeze(可以通过Tensorflow的trainable=False参数实现)，而剩下的输出层部分可以选择调整参数训练
  * 如果B任务的数据量大，那么我们可以将A中一半或者大部分的层进行freeze,而剩下部分的layer可以进行新任务数据基础上的微调

### 

