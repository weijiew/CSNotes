# 主题五：命令行环境

`kill -TERM <PID> ` 结束一个进程，需要直到进程号 PID 。

jobs 会列出当前终端中未完成的全部任务，可以查看 pid 。

ctrl-c 可以停止程序运行。

ctrl-z 可以使得正在运行的进程转到后台运行，然后再输入 bg。

ctrl-c/z 实际上是向操作系统发生了一个信号，操作系统根据信号的执行相应的程序。


创建别名，例如：`alias ll="ls -lh"` `alias gs="git status"` 等。

默认状态下 shell 不会保存别名，如果想长久使用需要保存配置到 .bashrc/.zshrc 中。

SSH 远程连接。

bash 是最通用的 shell 。

zsh 是 bash 的超集，添加了很多方便的功能。

## zsh 资源推荐

1. [安装](https://www.cnblogs.com/dhcn/p/11666845.html)，以这篇文章为主。可以配置完整配置好。
2. [zsh优点](https://blog.csdn.net/rapheler/article/details/51505003) 了解zsh特点，这篇文章为辅。
3. [repo](https://github.com/ohmyzsh/ohmyzsh) oh my zsh 仓库地址。 
4. [命令行美化主题](https://github.com/ohmyzsh/ohmyzsh/wiki/themes) 我采用的主题是 ys
5. [主题参考-知乎](https://www.zhihu.com/question/33277508)

![ys-主题](https://cdn.jsdelivr.net/gh/weijiew/pic@master/images/20200825191554.png)

## dotfile

像 .zshrc 这类以点开头的文件称为配置文件，shell 的很多配置都是通过这类文件完成的。也是隐藏文件。ls 不会显示。

可以将配置文件保存在云端，例如 Github 中。这样做舒服的地方在于当切换电脑时直接将这些配置文件下载到机器中。

不需要做一些繁琐的配置工作了。

可以参考别人的 dotfiles 配置方式进行学习。

SSH 如果是连接服务器的话需要打开 22 端口，否则无法连接。