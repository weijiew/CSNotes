# 概述

手写数字这个项目在深度学习就像 HelloWorld 一样的存在。大部分人都是从这个项目开始做起的。

在这个项目中我们需要构建一个模型，用数据来训练这个模型，不断提升准确率。

以上的大致流程可以分为四部分： 加载数据 - 构建网络 - 训练网络 - 测试网络。

# 过程

## 引入库

以下是这次项目运行所需要到的所有模块。

Keras 是深度学习中很重要的一个库，里面封装了很多功能。具体的详细功能可以查阅[Keras 文档](https://keras-zh.readthedocs.io/)

```python
from keras import layers 
```

* 数据处理模块。可以看成数据过滤器。 

```python
from keras import models 
```

* models 则是构建一个模块 ，用 layer 来实例化这个模块。

```python
from keras.datasets import mnist 
```

* mnist 是一个数据集，导入数据

```python
from  keras.utils import to_categorical 
```
* 向量矩阵转换为二进制的函数。


## 导入数据

```python
(train_images , train_labels) , (test_images , test_labels) = mnist.load_data()
```

导入数据后我们需要对数据进行分类，区分。分别是训练数据和对应的标签，训练数据则是图像转换成的 numpy 数组矩阵，而标签则是对应的 0-9 数字。

## 构建网络

```python
network = models.Sequential()
```

* 这是是用 model 模块构建了一个线性模型。就像数据蒸馏，每一层拿到了数据中的一些信息反馈给网络，然后网络调节参数。
* 还有不是线性的堆叠，多分支，多头网络，Inception模块，以后会介绍到。

```python
network.add(layers.Dense(512 , activation='relu', input_shape=(28*28,)))
network.add(layers.Dense(10 , activation='softmax'))
```

* 这是向模型中添加 Dense 层，全连接层。里面的参数则是对张量进行运算的设定。这些参数就是网络学到的`知识`。
* relu 是一个公式: output = relu(dot(W,input) + b) w 和 b 都是张量，也就是属性。
* 一个 10 路的 softmax 层。返回了 10 个总和为 1 的概率值组成的数组。

```python
network.compile(optimizer='rmsprop',
                loss='categorical_crossentropy',
                metrics=['accuracy'])
```
* 这一行是编译网络，loss 配置对应的损失函数 `categorical_crossentropy` 每一训练完后都能得到反馈。采用的是随机梯度下降，具体的方法则是第一个参数给出的。

## 准备数据

```python
train_images = train_images.reshape((60000 , 28 * 28 ))
train_images = train_images.astype('float') / 255

test_images = test_images.reshape((10000 , 28 * 28 ))
test_images = test_images.astype('float32') / 255
```

* 调整数据格式调整程规范的要求，本来数据的取值范围是 [0,255] ，而分母是 255 则意味着数据范围变成了 [0,1] 。
* 

```python
train_labels = to_categorical(train_labels)
test_labels = to_categorical(test_labels)
```
* 对标签进行分类编码。

## 训练

```python
network.fit(test_images , test_labels , epochs= 5 , batch_size= 128)
```

网络开始训练，填入训练数据和测试数据，一共迭代 5 次，每个批量包含了 128 个样本。训练的时候模型不会一次性把数据全吞下去，而是分成了很多批量，参数设置成了 128 意思就是每一个批量有 128 个样本。

在所有数据上迭代依次称为 `轮次` 。 也就是迭代 5 轮次，每次迭代根据损失值都相应的更新了权重。

```python
test_loss, test_acc = network.evaluate(test_images, test_labels)

print('test_acc:' , test_acc)
```

## 训练结果
> test_acc: 0.9750999808311462

这个模型识别的准确率达到了 97.5%

# 总结
做完这个对深度学习会有一个大致的了解，但是还有很多知识性的东西没有详细解释。知道的越多不会的也就越多。