# 0. 概述

主要记录学习过程中通过查询学到的知识，个人理解与总结。

作者的代码：[pythonvm](https://gitee.com/hinus/pythonvm/) 。

我的代码：[my-pyvm](https://github.com/weijiew/my-pyvm) 。

# 环境

* WSL2 
* Ubuntu 20.04 LTS 
* CLion 2020.3

如果不想用 WSL ，完全可以采用 vmware + Ubuntu 20.04 LTS 来代替。

书中代码采用的是 Python 2.7 ， Ubuntu20.04 内置了一个 python2.7

# 加载 CodeObject

1. .cpp 文件是 Cpp 代码的实现， .h 头文件仅包含定义，而 .hpp 则是二者的混合，不仅包含定义而且包含了实现。.hpp 是 Header Plus Plus 的简写。

2. #ifndef 是"if not defined"的简写，是预处理功能（宏定义、文件包含、条件编译）当中的条件编译，可以根据是否已经定义了一个变量来进行分支选择，其作用是：

　　1、防止头文件的重复包含和编译；

　　2、便于程序的调试和移植；

## 编译并运行

python 源码的后缀为 py 。然而 python 虚拟机并非直接是直接运行 py 文件，而是先生成二进制形式的 pyc 文件，该文件再由 python 虚拟机加载运行。

而 pyc 实际上是 PyCodeObject 对象的二进制形式。

编译生成 pyc 文件：

1. `python -m compileall hello.py` 编译生成 pyc 文件，注意 python 表示以 2.7 版本运行，python3 表示以 3.8 运行！此处以 python 2.7 运行。

2. `hexdump hello.pyc` 以二进制的方式查看 pyc 文件。

![1-1](https://cdn.jsdelivr.net/gh/weijiew/pic@master/images/image.5dnk9amojps0.png)

我第一次看书之时看到这里非常头大，其实慢慢分析即可。以下是经过检索后，以上内容的实际含义。

该文件是以 16 进制的形式显示，16 进制的前缀是 0x 。

> 例如 0x63 是 16 进制的形式，转成 10 进制就是 99 ，映射到 ASKII 码表中就是小写字母 c 。

* pyc 文件头部
  * [1-4]：f303 0a0d 也就是前四个字节，表示 python 的版本。也被称为 magic number 。
  * [5-8]：8a86 5fe0 表示文件的创建时间。
* PyCodeObject 对象二进制编译结果
  * [9]: 此处一定是 0x63 表示字符 c ，该字符的意思为接下来一定是 CodeObject 。
* PyCodeObject 对象，全局参数
* PyCodeObject 对象，code block （代码块）
  * 

写好代码后，运行如下命令：

2. `g++ -o pyvm -g main.cpp` 将 main.cpp 编译生成 pyvm 文件（该文件没有后缀，pyvm 可自定义）。其中 -o 后的参数为输出文件名， -g 表示编译时附带调试信息。
3. `./pyvm hello.pyc` 运行 pyvm 文件。其中 ./ 表示在当前目录下，hello.pyc 作为输入的第一个参数。

![1-2](https://cdn.jsdelivr.net/gh/weijiew/pic@master/images/image.4om4hp8mxf20.png)

GDB 调试

* `list` 表示输出查看源程序及其行号，简写 `l` 等效 。
* 



# 参考

1. [Python逆向（二）—— pyc文件结构分析](https://www.cnblogs.com/blili/p/11799483.html)
2. [详解 & 0xff 的作用](https://blog.csdn.net/i6223671/article/details/88924481)
3. [hpp头文件与h头文件的区别](https://blog.csdn.net/follow_blast/article/details/81706698)
4. [#ifndef详解](https://www.cnblogs.com/codingmengmeng/p/7221295.html)
5. [C/C++文件读写操作——FILE*、fstream、windowsAPI](https://blog.csdn.net/qq_15821725/article/details/78929344)


