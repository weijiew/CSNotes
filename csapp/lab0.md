# LAB-0: 搭建实验环境

我是在 win10 系统上做实验的，而实验环境要求 linux 平台，所以有以下三种解决方案。

1. 在 docker 中做实验。
2. WSL 
3. 虚拟机

三种方式我都尝试过，win10 下 WSL 最快，docker 次之，而虚拟机虽然搭建略微费事，但限制最少。

建议安装 docker 。除此之外 win10 可以选择使用 WSL ，这也完全可行，虚拟机次之。

我最初采用的是 docker ，所以下面记录了当时的内容。但是后来笔记本出了一些问题最后还是用虚拟机了。其实影响不大，只是入口存在差异，本质上都是安装了一个 Ubuntu 系统。

## 1. 在 docker 中做实验

为什么使用 docker ？

首先虚拟机笨重，docker 是单进程，轻量级。其次如果你不想在终端中编程的话，虚拟机需要重新安装 IDE ，例如 clion 或 vscode 之类的。而使用 docker 可以直接在 windows 下编程，使用 docker 编译。总之 docker 具备轻量和便捷的特点。

所以不论你是什么系统，安装好 docker 后，后续步骤都相同。

### 1.0 安装 docker 

建议先安装 docker

* [Windows Docker 安装](https://www.runoob.com/docker/windows-docker-install.html)
* [Mac下安装docker的三种方法](https://zhuanlan.zhihu.com/p/91116621)

### 1.1 使用 docker 并拉去配置好的镜像

docker 安装完毕后打开 CMD 输入如下命令。

下面这个镜像是我已经打包好的镜像，你可以直接拉取。或者选择跳过这一步选择自己制作镜像。 

```sh
docker pull weijiew/csapp_datalab
```

根据镜像构建容器（csapp_datalab）:

```
docker container run -it --name=csapp_datalab weijiew/csapp_datalab /bin/bash
```

或者和共享本地的文件夹，我的代码在路径 `E:/Repo/CSAPP-Labs` 下。

```
docker run -it -v E:/Repo/CSAPP-Labs:/csapp --name=csapp_env weijiew/csapp_datalab /bin/bash
```

实验文件都在 `/csapp/data_lab` 中。

如果你不想在终端中敲代码，可以使用 vscode attach 这个容器就可以开始写代码了。（注意安装 docker 插件）

### 1.2 自己搭建实验环境

1.  拉取一个 ubuntu:18.04 的镜像。

```
docker pull ubuntu:18.04
```

2. 根据镜像构建容器并设置共享文件夹。

```
docker container run -it -v E:/Repo/CSAPP-Labs:/csapp --name=csapp_env ubuntu:18.04 /bin/bash
```

`E:/Repo/CSAPP-Labs:/csapp` 中 `E:/Repo/CSAPP-Labs` 是本地路径，而 `/csapp` 则是映射到容器中的路径，设置好后二者的内容都是同步的。可以在`E:/Repo/CSAPP-Labs:/csapp`中编辑，在`/csapp`中编译。

查看共享情况：

```
root@ec3dec38c3a9:/# ls
bin  boot  csapp  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
root@ec3dec38c3a9:/# cd csapp/
```

### 1.3 配置容器

1. 更新

更新软件并安装 sudo ，提权操作。
```
apt-get update
apt-get install sudo
``` 

2. 换源

> 这步可以跳过，如果在国内还是建议换一下，速度更快。

安装 vim ，文本编辑工具，为下一步的换源做准备，[vim 使用参考](https://www.runoob.com/linux/linux-vim.html)。

```
apt-get install vim
```

备份并编辑：

```
sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak
sudo vim /etc/apt/sources.list
```

删除或注释原始内容，并将如下内容贴入：

```
deb http://mirrors.aliyun.com/ubuntu/ focal main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ focal-security main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ focal-updates main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ focal-proposed main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ focal-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ focal main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ focal-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ focal-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ focal-proposed main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ focal-backports main restricted universe multiverse
```

更新

```
sudo apt-get update
```

3. 安装 c++ 编译环境

```
sudo apt-get install build-essential
sudo apt-get install gcc-multilib
```

### 1.4 退出并启动容器

* `exit` 退出容器。
* `docker ps -a` 找到 csapp_env 的容器 ID 。
* `docker start 容器id` 启动容器。
* `docker exec -it 容器id /bin/bash` 进入容器。

# 2.0 WSL

1. 换源。
2. 安装编译环境。

与 WSL 相关的内容可以参考这篇[文档](https://dowww.spencerwoo.com/)讲的很详细。

# 3. 虚拟机

虚拟机存在两种 VMware 和 VirtualBox ，前者收费但是功能强大（虽然收费但是破解起来很方便），反之后者开源免费但是功能不如前者。

安装 Ubuntu 20.04

换源并安装编译环境。

```
sudo apt-get install build-essential
sudo apt-get install gcc-multilib
```

可以设置一个共享文件夹，这样就能够在宿主机中编辑，在虚拟机中编译。仅仅把虚拟机当作一个编译器来用，不需要再另外搭建一套 IDE 。

设置[共享文件夹](https://zhuanlan.zhihu.com/p/42203768)，这样可以在 windows 下编辑，然后在虚拟机中编译。

可以[参考这篇文章](https://blog.csdn.net/hhhuua/article/details/80003620)设置 ssh 在本地访问虚拟机。

# 5. 参考

1. [windows](https://zhuanlan.zhihu.com/p/340283308) 下环境配置。