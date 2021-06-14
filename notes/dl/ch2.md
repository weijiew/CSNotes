# 概述

# 导入库

先将所需要库导进来

```python
from keras.datasets import imdb
from keras import models
from keras import layers
from keras import metrics
from keras import losses
from keras import optimizers
from keras.datasets import imdb

import numpy as np
```

# 导入数据

```python
(train_data, train_labels), (test_data, test_labels) = imdb.load_data(num_words=10000)
```

* 首先导入数据集并加载，加载数据的时候选择的是前 10000 个最频繁出现的单词，也就是对所有评论中的单词统计频数，排个序，排在前 10000 留下，之后的不要。这样做是为了减小数据量，我认为出现次数多的单词中学到的知识（权重）相对较少，得不偿失。而且数据量太大，电脑也扛不住。

```python
print(train_data[0]) # [1, 14, ···, 1]
print(train_labels[1]) # 0 
```

* 通过这些代码调试，可以很清晰的看到训练集中储存的是数字。而不是评论本身，这些其实是字母对应的索引，根据这些索引去查表得到字母，然后字母组成句子就是评论。
* 查看标签数据可以看到数据 0 / 1 。 前者代表负面评价，后者则是正面评价。

## 准备数据

将数据填入网络之前需要将数据整理好。如果不整理加载进来的数据集很不规整，就像排队一样，左边长右边短的，不仅看起来难受，用起来也不方便，所以需要让他们排列整齐。有两种转换方式。这里采用的是 one-hot。

* one-hot ：准备一个长度为 10000 的序列，里面填满了零，将对应存在评论的索引改为 1 即可。

```python
def vectorize_sequences(sequences, dimension=10000):
    np.zeros(sequences, dimension)
    for i, sequence in enumerate(sequences):
        results[i, sequence] = 1
    return results
```python
* np 是 numpy ，调用 zeros 函数，创建了一个零矩阵，矩阵的行数是 sequences 的长度，也就是评论的个数，矩阵的列数是定值 10000，因为设定过了，索引不会超过 10000。
* 遍历每一条评论，enumerate 返回两个值，i 是顺序，从零开始， sequence 对应评论的下标。
* results 实现了将每一条评论的索引进行了映射，一共 10000 个位置。

enumerate 作用：

```python
>>>seasons = ['Spring', 'Summer', 'Fall', 'Winter']
>>> list(enumerate(seasons))
[(0, 'Spring'), (1, 'Summer'), (2, 'Fall'), (3, 'Winter')]
>>> list(enumerate(seasons, start=1))       # 下标从 1 开始
[(1, 'Spring'), (2, 'Summer'), (3, 'Fall'), (4, 'Winter')]
```

格式变换如下。

```python
x_train = vectorize_sequences(train_data)
y_train = vectorize_sequences(test_data)

x_label = np.array(train_labels).astype('float32')
y_label = np.array(test_labels).astype('float32')
```

* 将以上封装成一个函数 `np.zeros(sequences, dimension)` 其实是将其转换成了一个二维数组。第一维的长度维 10000。 就相当于每一条评论占了一个坑，坑里面有 10000 个小位置。根据这些评论的索引将零数组对应的下标改为 1 即可。

* 数组改完后还需要将标签向量化。

# 构建网络

* 构建网络，还是采用线性模型。一共三层，对应参数如下。

```python
model = models.Sequential()
model.add(layers.Dense(16, activation='relu', input_shape=(10000,)))
model.add(layers.Dense(16, activation='relu'))
model.add(layers.Dense(1, activation='sigmoid'))
```

* 编译网络，配置优化器 rmsprop 


```python
model.compile(optimizer='rmsprop',
            loss='binary_crossentropy',
            metrics=['accuracy'])
```
# 训练

```python
model.fit(x_train, y_train, epochs=4, batch_size=512)
results = model.evaluate(x_test, y_test)
print(results)
```
将训练数据导入，设置批次以及每一批的大小。训练完成后导入测试数据，输出测试结果。