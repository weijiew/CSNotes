# Lab 0：网络热身

简介：使用 C++ 写一个获取网页的程序，这个程序可以实现网络关键的抽象（内存中），大概消耗 2 - 6 小时。后续的实验要在前置 lab 的基础上。

* 先浏览整个实验。
* 实验与实验直接是存在联系的，不建议跳，跳的话可能写不下去。
* lab 不是完美的，可能存在问题。

## 0. 协作政策

* 除了课程提供的部分辅助代码外，其他代码必须是自己完成的，不允许 copy 。如果你参考了网上的代码，提交代码之时需要将链接作为注释写在源代码上。
* 自己不能看别人的代码，自己的代码也不能展示给别人，不允许看往届的答案。可以和同学讨论作业，但是不能 copy 任何人的代码。如果和同学讨论了，在提交代码之时需要将同谁讨论作业的同学名字作为注释写在相关的代码部分。handout 中有更多的细节。

## 1. 系统安装 

CS144 的作业需要在 GNU/Linux 系统之上进行并且要有 C++ 2017 版本的编译器。

安装 Linux 系统，官方提供的三种安装方式，分别是安装官方提供的镜像，其次是自己安装 Ubuntu 18.04 LTS 版本然后运行网站中所提供的工具安装脚本，最后是自己摸索。

> 我最初采用的是安装官方镜像，后来装 IDE 的时候出现了一些问题，于是改为了 WSL 。如果你的系统是 WIN10 采用 WSL 来做耗时相对耗时较少。以下是配置步骤。其实就是[第二种安装](https://stanford.edu/class/cs144/vm_howto/vm-howto-iso.html)方式。只不过把前面的系统安装替换为了 WSL 。

### 1.1 WSL


1. 打开微软商店，搜索 WSL ，安装 Ubuntu 20.04 LTS ，虽然官方的环境是 18.04 LTS 经过我的测试 20.04 也是可用的。wsl 下载很快，但是配置的时候有点慢，需要等十几分钟。
2. 如果没有 Windows Terminal 的话建议安装 Windows Terminal。同样是在微软商店。
3. 然后下载安装脚本： `wget https://web.stanford.edu/class/cs144/vm_howto/setup_dev_env.sh `
4. 记得换源，不然后续执行会非常慢！
5. 执行脚本：`bash ./setup_dev_env.sh` （即使换源这个花费时间也比较长，大约 10-20 分钟。不换的话可想而知！）
6. IDE 采用 vscode 。vscode 需要安装 remote wsl 插件。
7. 在 WSL 中输入 code 。（表示在 WSL 中安装 vscode）
8. 切换到后续下载代码的位置，输入命令 `code .` 即可打开。（表示在当前文件夹中打开 vscode）

与 WSL 相关的更多内容可参考 [dowww](https://github.com/spencerwooo/dowww) 。上诉方法是目前为止我认为耗时最少的。

### 1.2 镜像安装

> 以下是第一种环境配置的摸索，此方法耗时部分在于镜像下载。

我最初选择的是第一种，也就是下载官方所提供的镜像。也就是按照这一页的内容来：https://stanford.edu/class/cs144/vm_howto/vm-howto-image.html

注意：Windows 系统的话需要先下载虚拟机（VirtualBox），页面中有写到，按照步骤来就可以了。

可以直接默认，也可以根据你的电脑配置来修改。

我将内存（RAM）改为了 4G 。CPU 核心数改为了 2 。

![image-1](https://cdn.jsdelivr.net/gh/weijiew/pic@master/images/image.6dmk7hpxcmo0.png)

首次登陆会强制修改密码，用户名和密码均为 **cs144** 。

登陆进去后默认是文本中断，如果想要图形界面需要以下命令：

安装图形界面：`sudo apt-get install --reinstall lightdm`

启动图形界面：`sudo systemctl start lightdm`

终端访问：我的系统本身是 win10 ，采用的终端是 WSL ，访问命令：`ssh -p 2222 cs144@localhost` 。（这一步可以不选，我觉得用 WSL 访问比较便捷，windows 下你可以采用 XShell 或 Putty 等终端）

该系统其实就是 Ubuntu 18.04 LTS

![image-2](https://cdn.jsdelivr.net/gh/weijiew/pic@master/images/image.41e5ycv81yy0.png)

最后记得换源！

## 2. 手动联网

有两个任务，像浏览器一样读取网页，其次像邮件客户端一样发送一个邮件消息。

以上的两个任务都依赖可靠双向字节流的网络抽象。可靠双向字节流指在本地中断中输入一串消息，该消息一相同的顺序发送给另一台计算机，另一台计算机收到该消息后会回复一条消息表示收到。

### 2.1 捕获一个网页

1. 这一节内容是告诉我们如何在浏览器中看到网页访问所得到的信息。

首先在浏览器中查看 http://cs144.keithw.org/hello 可以看到 `Hello, CS144!` 。


2. 那么在终端中，按照文中所给步骤也是可以看到该内容的。

![image-3](https://cdn.jsdelivr.net/gh/weijiew/pic@master/images/image.5070rpwtdig0.png)


3. 修改 GET 请求的路径即可，改成自己的 ID 能拿到属于自己的 X-Your-Code-Is 。

![image-4](https://cdn.jsdelivr.net/gh/weijiew/pic@master/images/image.70o0sngyzc40.png)


### 2.2 给自己发邮件

这个需要处于 stanford 的网络才能做，跳过了。

### 2.3 监听和连接

要分别开两个终端，分别代表服务端和客户端。

`netcat -v -l -p 9090` 表示监听 9090 端口，当作服务端，一旦受到消息就显示出来。

`telnet localhost 9090` 表示访问 9090 端口，当作客户端，在其中输入内容会在服务端中显示出来。

服务端和客户端都可实现交互，注意是在一个虚拟机中。

## 3. 使用系统套接字编写程序

通过数据交付之时产生的一系列问题介绍了 TCP 产生的原因。本次实验比较简单，简单使用系统内置的 TCP 。后续的实验就需要自己的实现 TCP 啦！

git clone https://github.com/cs144/sponge

### 3.1 构建起步代码

1. `git clone https://github.com/cs144/sponge` 下载
2. 不允许公开！
3. `cd sponge` 进入该文件夹中。
4. `mkdir build` 创建 build 文件夹，用于存放编译后的文件。
5. `cd build` 进入 build 文件中。
6. `cmake ..` 构建编译方式。
7. `make` 编译代码
8. 需要提交实验报告 `sponge/writeups/lab0.md` 是要提交实验报告的位置，这个可以略过！

### 3.2 现代 C++

介绍了一些使用 C++ 一些要避免以及要注意的地方！

使用 const 传参可以避免变量中所存储的值被修改。如果是传值的话需要拷贝副本消耗空间，而传址可能会导致变量被修改所以加上 const 来保护。

为了方便 debug ，git 提交时尽可能频繁 commit ，并注明每次 commit 的信息。

### 3.3 阅读 Sponge 文档

1. 阅读：https://cs144.github.io/doc/lab0/


### 3.4 编写 webget 程序

文件位于 `../apps/webget.cc` 。首先阅读代码，建议先看 main 函数。

1. 第 24 行代码：`int main(int argc, char *argv[])` 。其中 argc 表示命令行中输入参数的个数，而 argv 则是一个字符数组，其中 argv[0] 存的是文件名，后续存的则是参数。
2. 第 27 行代码：`abort()` 函数属于 stdlib.h 头文件中的函数，表示终止当前进程，直接从调用的地方跳出。argc 表示无参数，此时程序终止。
3. `const string &host` 采用 const 修饰形参是为了避免新参值被修改，& 则表示取地址，也就是传值。

其实就是输入 host 和 path 两个参数，然后调用 get_URL 这个函数。

```cpp
    TCPSocket sock;
    sock.connect(Address(host,"http"));
    // 这串内容是仿照着 2.1 的内容而写，也就是将手动输入的拼接成字符串
    // 在 Http 协议中 \r\n 表示换行！
    sock.write("GET " + path + " HTTP/1.1\r\nHost: " + host + "\r\n\r\n");
    sock.shutdown(SHUT_WR);
    while(!sock.eof()){
        cout << sock.read();
    }
    sock.close();
    return;
```

* 首先在 build 目录下执行 make 命令对代码进行重新编译，因为修改代码了。
* 然后判断程序是否能够正确运行 `./apps/webget cs144.keithw.org /hello` 
* 最后采用测试用例判断 `make check_webget`

## 4. 基于内存的可靠字节流

* [byte_stream.hh](https://github.com/weijiew/TCP/blob/main/libsponge/byte_stream.hh)
* [byte_stream.cc](https://github.com/weijiew/TCP/blob/main/libsponge/byte_stream.cc)

## 5. 参考

1. [C语言中 int main(int argc,char *argv[])的两个参数详解](https://blog.csdn.net/weixin_40539125/article/details/82585792)
