# Assignment 1

* [assignment1](https://cs231n.github.io/assignments2020/assignment1/)

## 1.0 环境

以下内容为我的实现方式，如果想做的别的选择请参考官网，一手资料：https://cs231n.github.io/setup-instructions/ 

有两种方式来实现该作业，一种是远程在 Google Colaboratory 上做，另一种是在本地机器上做。我采用的方式的后者。

首先阅读 https://cs231n.github.io/setup-instructions/#working-locally-on-your-machine

采用 conda/venv 来建立一个虚拟环境。我选择是是 conda 来创建虚拟环境，如果没有 conda 可以下载一个，也可以采用 venv 来做，具体可参考官网的实现方式。 

采用 conda 创建虚拟环境：`conda create -n cs231n python=3.6`

激活虚拟环境：`conda activate cs231n`

退出虚拟环境：`conda deactivate cs231n`

使用 pip 来下载所需的包。

* `cd assignment1`
* `pip install -r requirements.txt`

## 2.0 下载数据集

```sh
cd cs231n/datasets
./get_datasets.sh
```

在 windows 下，使用 cmd 终端会导致以上命令无法使用。

其实该脚本的内容就是下载一个数据集并解压到 dataset 文件夹中。

我是直接访问链接下载数据集并移动到该位置下，然后采用 git bash 来解压处理。

> 文件下载速度很慢，后续会做一个备份。

然后启动 jupyter : `jupyter notebook`


> 我是用 vscode 做的，其中装一个 jupyter 插件比较好用。

## 3.0 Coding

### 3.1 compute_distances_two_loops.

计算训练集中的每个点合测试集中点之间的距离。

这道题明确输入输出就好了，输入是 500 张图片，一行代表一张图片，所有共 500 行。每一张图片的像素是 32 * 32 ，通道为 3 ，所以共 3072 个维度，也就是 3072 列。

训练集和测试集的数据格式等同。

输出是一个二维数组，每一行是一个测试数据，每一列是一个训练数据，行和列交叉的地方表示训练数据同测试数据之间的欧式距离。

> 卡住的原因是对 numpy 的索引方式不熟悉，习惯了 matlab 的索引。

`dists[i,j] = np.sqrt(np.sum(np.square(X[i] - self.X_train[j]))) `

500 张测试集和 5000 张训练集跑起来太慢了，为方便，我选择缩小 10 倍。

