# Lab-0: 环境搭建

我采用的是虚拟机，宿主机是 win10 ，虚拟机采用的是 VMware 。

首先安装 Ubuntu20.04 ，注意一定不能安装别的版本，这个版本省事。

接下来就说配置环境，我参照的信息均来源于此页面： https://pdos.csail.mit.edu/6.828/2020/tools.html 。

## 1. 安装包

安装 `sudo apt-get install git build-essential gdb-multiarch qemu-system-misc gcc-riscv64-linux-gnu binutils-riscv64-linux-gnu` 

该版本的包存在一个问题 `sudo apt-get remove qemu-system-misc` 需要卸载安装指定版本。

安装指定版本 `sudo apt-get install qemu-system-misc=1:4.2-3ubuntu6` 。

## 2. 测试环境是否配置成功

据此命令 `riscv64-unknown-elf-gcc --version` 出现如下内容：

    weijiew@ubuntu:~$ riscv64-unknown-elf-gcc --version

    Command 'riscv64-unknown-elf-gcc' not found, but can be installed with:

    sudo apt install gcc-riscv64-unknown-elf

意思是缺少该包，按照提示安装即可：`sudo apt install gcc-riscv64-unknown-elf`

安装后输入下面的两条命令判断 `riscv64-unknown-elf-gcc --version`

    weijiew@ubuntu:~$ riscv64-unknown-elf-gcc --version
    riscv64-unknown-elf-gcc () 9.3.0
    Copyright (C) 2019 Free Software Foundation, Inc.
    This is free software; see the source for copying conditions.  There is NO
    warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.

`qemu-system-riscv64 --version`

    weijiew@ubuntu:~$ qemu-system-riscv64 --version
    QEMU emulator version 4.2.0 (Debian 1:4.2-3ubuntu6)
    Copyright (c) 2003-2019 Fabrice Bellard and the QEMU Project developers

到此处本实验其实已经结束了。

## 最后

下面的内容其实是下一个实验的，当然来本实验的最后其实也提到了。目的是将 xv6 的代码拉取下来看能否编译成功，据此判断环境是否搭建成功。

我将代码放到了共享文件夹中，关于共享文件夹的设置方法可以参照[此处](https://zhuanlan.zhihu.com/p/42203768)。

下载 xv6 代码并编译判断

    git clone git://g.csail.mit.edu/xv6-labs-2020
    cd xv6-labs-2020
    git checkout util

然后输入 `sudo make qemu` 编译代码。

当出现 xv6 kernel is booting 表示着源码编译搭建成功。

进入 qemu 后可以输入一些命令来查看，例如 `ls` 等。

退出 qemu ： 先输入 ctrl + a 抬起后再输入 x 。

# 参考

[1. MIT 6.S081: Lab 0 搭建环境](https://zhuanlan.zhihu.com/p/343655412)
[2. qemu 的退出方法](https://blog.csdn.net/zsj1126/article/details/104054913)