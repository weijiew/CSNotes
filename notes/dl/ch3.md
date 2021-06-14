# 新闻分类问题

新闻的主题有很多种，每一条数据是以篇新闻，根据新闻内容判断主题，这里的数据一共有 46 种主题。

# coding

## 导入库

下面是用到的库，提前导入。
```python
from keras.datasets import reuters
from keras.utils.np_utils import to_categorical
from keras import models
from keras import layers

import matplotlib.pyplot as plt
import numpy as np
```

## 加载数据
`(train_data, train_labels), (test_data, test_labels) = reuters.load_data(num_words=10000)`

将数据导入进来。

## 数据标准化
设定数据的形状，和上一篇一样。
```python
def vectorize_sequences(sequences, dimension=10000):
    results = np.zeros((len(sequences), dimension))
    for i, sequence in enumerate(sequences):
        results[i, sequence] = 1
    return results

x_train = vectorize_sequences(train_data)
x_test = vectorize_sequences(test_data)

```
`to_categorical` 函数是 keras 里面的函数， one-hot 编码。本质就是下面的函数。和向量化的函数几乎一样。

```python
def to_one_hot(labels,dimension=46):
    results = np.zeros((len(labels,dimension)))
    for i , label in enumerate(labels):
        results[i,label] = 1
    return results
```

```python
one_hot_train_labels = to_categorical(train_labels)
one_hot_test_labels = to_categorical(test_labels)
```
## 搭建网络

还是线性网络。
```python
model = models.Sequential()
model.add(layers.Dense(64, activation='relu', input_shape=(10000,)))
model.add(layers.Dense(4, activation='relu'))
model.add(layers.Dense(46, activation='softmax'))
```
## 编译网络

设置优化器，损失函数，准确值。

```python
model.compile(optimizer='rmsprop',
            loss='binary_crossentropy',
            metrics=['accuracy'])
```

## 数据分批

用切片将数据分批，`[:1000]` 表示前 1000 条数据。对应的标签也应分批。

```python
x_val = x_train[:1000]
partial_x_train = x_train[1000:]

y_val = one_hot_train_labels[:1000]
partial_y_train = one_hot_train_labels[1000:]
```

## 查看训练损失和验证损失

```python
history = model.fit(partial_x_train, partial_y_train, epochs=20, batch_size=512, validation_data=(x_val, y_val))
loss = history.history['loss']
val_loss = history.history['val_loss']
epochs = range(1, len(loss) + 1)
plt.plot(epochs, loss, 'bo', label='Training loss')
plt.plot(epochs, val_loss, 'b', label='Validation loss')
plt.title('Training and validation loss')
plt.xlabel('Epochs')
plt.ylabel('Loss')
plt.legend()
plt.show()
```

## 查看训练精度和验证精度

可以看出存在过拟合现象。

