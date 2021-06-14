# Lab 00

[lab00 实验地址](https://inst.eecs.berkeley.edu/~cs61a/sp20/lab/lab00/) 。

这个实验主要是环境搭建和 Python 的基本语法学习。

如果没有学过 Python 略微有些吃力。其实没什么问题，需要的前置知识并不多。 Just do it！

安装提示首先下载 lab00.zip 文件，然后解压，开始做实验！

解压 lab00.zip 后，其中文件内容如下:

    .
    ├── lab00.ok
    ├── lab00.py
    ├── ok
    └── tests
        ├── __init__.py
        └── python-basics.py

实验内容目前来看分为两种类型，一种是在命令行中根据题目交互式写答案，另一种则是写代码然后在命令行中输入命令进行测试。

对于此例而言，前者是通过调用 `test/python-basics.py` 来实现，后者则是在 `lab00.py` 文件中编写代码来实现。

而其中 ok 程序则是用来判断程序正确与否的文件，其本质是已经编译好的二进制文件。

例如第一个作业：

![image-1](https://cdn.jsdelivr.net/gh/weijiew/pic@master/images/image.1jy3hrqj6jmo.png)

对于前者而言，如果答对，文件会解锁，也就是通过验证后如下内容会显示正确的答案，而非目前所见的乱码。其实目前所见的就是正确答案的加密字符。

![image-2](https://cdn.jsdelivr.net/gh/weijiew/pic@master/images/image.5sit9pwx0i80.png)

做完之后会提示输入邮箱，不用理会，ctrl + c 中断即可。

![image-3](https://cdn.jsdelivr.net/gh/weijiew/pic@master/images/image.4opv6uv6r560.png)

如果嫌提示输入邮箱烦人，可以在命令后加上 `--local` 参数，表示本地测试。建议加上 `--local` 。

通过测试后，文件解锁，加密内容变为正确答案，注意如果不加 `--local` 参数，即使全部答对题目，但因为 ctrl + c 没有正常结束程序，所以加密内容不会还原。

![image-4](https://cdn.jsdelivr.net/gh/weijiew/pic@master/images/image.5s0lhznb7m40.png)

我是从 Doing the assignment 开始的，之前的内容为环境配置以及一些 Linux 命令学习。

## Doing the assignment

下面的几道测试题，用于判断文中提到的 python 语法。

在下载好的 lab00 文件的工作目录中执行命令。

直接在当前的工作空间中输入命令即可：`python3 ok -q python-basics -u`

![image-5](https://cdn.jsdelivr.net/gh/weijiew/pic@master/images/image.5d8hx2d9wc40.png)

交互式输入答案即可。

我已经通过了这个文件，为避免篇幅过长，具体可查看此处: [weijiew/cs61a](https://github.com/weijiew/cs61a/blob/master/lab/lab00/tests/python-basics.py) 。

## coding

通过给定的操作符，最终结果等于 2020 即可。

```python
def twenty_twenty():
    """Come up with the most creative expression that evaluates to 2020,
    using only numbers and the +, *, and - operators.

    >>> twenty_twenty()
    2020
    """
    return 2019 + 1
```

通过键入命令 `python3 ok --local` 来对代码进行测试，测试结果如下。没有失败的测试用例则表示通过。

![image-6](https://cdn.jsdelivr.net/gh/weijiew/pic@master/images/image.6rndwletqjk0.png)

## 总结

该实验主要是熟悉环境以及如何测试，python 的一些简单语法。