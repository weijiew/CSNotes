# 主题一：shell

下图是 一个常见的命令行：

![命令行](https://cdn.jsdelivr.net/gh/weijiew/pic@master/images/20200824153454.png)

* ~ 表示当前位于 home 目录下。
* $ 符号表示您现在的身份不是 root 用户。

shell 是命令行解释器，用于解释命令。例如输入 date 会输出当前日期。

![date](https://cdn.jsdelivr.net/gh/weijiew/pic@master/images/20200824153826.png)

shell 采用空格来分割命令进行解析。

例如 echo 命令，以空格分割。
```sh
weijiew@LAPTOP-PJ3DJQFQ:~$ echo Hello
Hello
```

如果空格作为命令的一部分，需要采用转义符号 \ 进行转义。

如果执行的命令不是 shell 中内置的关键字，shell 会去咨询环境变量 $PATH 。

> 例如配置的 Java 环境变量之前输入 Java 命令无效，配置环境变量后就可以解释了。

## 在 shell 中导航

在 Linux/macOS 中路径以斜杠 / 分割，根目录是 / 代表系统所有文件都在该路径之下。

在 windows 中路径以反斜杠 \ 分割。分为多个盘，例如 （C/D/E/F）等，每个盘都有一个根目录。

例如：`C:\` 

绝对路径表示以根目录起始的路径。

相对路径则是相对于当前工作目录的路径。绝对路径之外的都是相对路径。

例如：使用 pwd 命令可以查看当前的工作目录。

```sh
weijiew@LAPTOP-PJ3DJQFQ:~$ pwd
/home/weijiew
```

可以用 cd 命令来切换目录。其中 .. 表示退回到上级目录。

```sh
weijiew@LAPTOP-PJ3DJQFQ:/home$ cd ..
weijiew@LAPTOP-PJ3DJQFQ:/$
```

一个点 . 表示当前工作目录。

```sh
weijiew@LAPTOP-PJ3DJQFQ:/$ cd ./home
weijiew@LAPTOP-PJ3DJQFQ:/home$
```

波浪线(~)表示当前用户的主目录，反斜杠(/)表示根目录，所有文件的起始，二者是不同的。

如果是 root 用户，cd ~ 会切换到 /root 目录下。

```sh
root@LAPTOP-PJ3DJQFQ:/# pwd
/
root@LAPTOP-PJ3DJQFQ:/# cd ~
root@LAPTOP-PJ3DJQFQ:~# pwd
/root
```

如果不是 root 用户，会切换的到当前用户的主目录下。也就是 /home/{username} 目录中。

例如我的用户名是 weijiew ，那么 cd ~ 将会进入到 /home/weijiew 中。

```sh
weijiew@LAPTOP-PJ3DJQFQ:/$ pwd
/
weijiew@LAPTOP-PJ3DJQFQ:/$ cd ~
weijiew@LAPTOP-PJ3DJQFQ:~$ pwd
/home/weijiew
```

撤销目录切换。当切换到其他目录后如果想回退到切换前的目录中，可以采用 `cd -` 来实现。

```sh
weijiew@LAPTOP-PJ3DJQFQ:~$ pwd
/home/weijiew
weijiew@LAPTOP-PJ3DJQFQ:~$ cd /
weijiew@LAPTOP-PJ3DJQFQ:/$ cd -
/home/weijiew
weijiew@LAPTOP-PJ3DJQFQ:~$ pwd
/home/weijiew
```

ls 命令用于查看当前目录下的文件。

```sh
weijiew@LAPTOP-PJ3DJQFQ:/home$ ls
weijiew
```

ls 命令对应很多参数，如果忘记参数含义可以采用 `ls --help` 进行查看。

例如 ls -l 可以更详细的打印文件或文件信息。

```sh
weijiew@LAPTOP-PJ3DJQFQ:~$ ls -l /home
total 0
drwxr-xr-x 1 weijiew weijiew 4096 Aug 24 14:47 weijiew
```

第三行中开头的 d 表示 /home 是一个目录。

接下来的 9 个字符每三个字符一组。

假如用括号分割的话：d(rwx)(r-x)(r-x) 。

这三组分别代表了**文件所有者**，**用户组**和**其他所有人**对这个文件夹所拥有的权限。
* r (read) 代表可读。
* w (write) 代表可写也就是具有修改权。
* x 代表可执行。
* \- 则代表该用户不具备相应的权限。

> 注意，/bin目录下的程序在最后一组，即表示所有人的用户组中，均包含x权限，也就是说任何人都可以执行这些程序。

* mv 重命名或移动文件。
* cp 拷贝文件。
* mkdir 新建文件夹。

如果忘记命令的相关内容，可以使用 man 来查看该命令的用户手册。

例如 man ls ,注意按 q 可以退出查看界面。

man 可以获得比 help 更多的信息。

## 在程序间创建连接

信息的流动称为**流**，程序中存在两个流。

流入程序的称为**输入流**，流出的则称为**输出流**。

程序读取信息时会从输入流中进行读取，相反打印信息时则是输出到输出流中。

例如重定向 > 可以将程序的输入流和输出流分别重定向到文件中。

> 最简单的重定向是 < file 和 > file。这两个命令可以将程序的输入输出流分别重定向到文件：

```sh
weijiew@LAPTOP-PJ3DJQFQ:~$ echo hello > hello.txt
weijiew@LAPTOP-PJ3DJQFQ:~$ cat hello.txt
hello
weijiew@LAPTOP-PJ3DJQFQ:~$ cat < hello.txt > hello2.txt
weijiew@LAPTOP-PJ3DJQFQ:~$ cat hello2.txt
hello
```

\>> 表示追加内容。

```sh
weijiew@LAPTOP-PJ3DJQFQ:~$ cat hello.txt
hello
weijiew@LAPTOP-PJ3DJQFQ:~$ echo hello >> hello.txt
weijiew@LAPTOP-PJ3DJQFQ:~$ cat hello.txt
hello
hello
```

> 使用管道（ pipes ），我们能够更好的利用文件重定向。 |操作符允许我们将一个程序的输出和另外一个程序的输入连接起来。

管道线的用法实例。 

ls -l / 表示查看根目录 (/) 下所有文件的详细信息。

```sh
weijiew@LAPTOP-PJ3DJQFQ:~$ ls -l /
total 580
lrwxrwxrwx  1 root root      7 Apr 23 14:40 bin -> usr/bin
drwxr-xr-x  1 root root   4096 Apr 23 14:49 boot
drwxr-xr-x  1 root root   4096 Aug 24 15:37 dev
drwxr-xr-x  1 root root   4096 Aug 24 15:34 etc
drwxr-xr-x  1 root root   4096 May 29 21:45 home
-rwxr-xr-x  1 root root 591344 Jan  1  1970 init
-rwxrwxrwx  1 root root     11 Aug 24 17:08 last-modified.txt
lrwxrwxrwx  1 root root      7 Apr 23 14:40 lib -> usr/lib
lrwxrwxrwx  1 root root      9 Apr 23 14:40 lib32 -> usr/lib32
lrwxrwxrwx  1 root root      9 Apr 23 14:40 lib64 -> usr/lib64
lrwxrwxrwx  1 root root     10 Apr 23 14:40 libx32 -> usr/libx32
drwxr-xr-x  1 root root   4096 Apr 23 14:40 media
drwxr-xr-x  1 root root   4096 May 29 21:44 mnt
drwxr-xr-x  1 root root   4096 Apr 23 14:40 opt
dr-xr-xr-x  9 root root      0 Aug 24 15:34 proc
drwx------  1 root root   4096 Aug 24 17:08 root
drwxr-xr-x  1 root root   4096 Aug 24 15:34 run
lrwxrwxrwx  1 root root      8 Apr 23 14:40 sbin -> usr/sbin
drwxr-xr-x  1 root root   4096 Apr 10 22:57 snap
drwxr-xr-x  1 root root   4096 Apr 23 14:40 srv
dr-xr-xr-x 12 root root      0 Aug 24 15:34 sys
drwxrwxrwt  1 root root   4096 Aug 24 16:23 tmp
drwxr-xr-x  1 root root   4096 Apr 23 14:41 usr
drwxr-xr-x  1 root root   4096 Apr 23 14:43 var
```

如果我只想查看最后一个文件夹的相信信息，可以采用管道线进行**过滤**。

其中 tail 命令表示从尾部开始查看文件，参数 n 后面的数字表示具体要查看几行。

```sh
weijiew@LAPTOP-PJ3DJQFQ:~$ ls -l / | tail -n1
drwxr-xr-x  1 root root   4096 Apr 23 14:43 var
```

也可以将信息输出流 (>) 写入文本中。

```sh
weijiew@LAPTOP-PJ3DJQFQ:~$ ls -l / | tail -n1 > ls.txt
weijiew@LAPTOP-PJ3DJQFQ:~$ cat ls.txt
drwxr-xr-x  1 root root   4096 Apr 23 14:43 var
```

## 一个功能全面又强大的工具

根用户在类 Unix 系统中是非常强大的，拥有整个系统的所有权限。

通过 su (super user 缩写) 命令进行切换到根用户，也称为超级用户或 root 用户。

因为根用户权限比较高，
所以通常不建议处于根用户的状态避免误操作。
但是执行某些命令是权限不够，sudo 命令用于解决这个问题，也就是可以以 su 的身份来 do 一些事情。

普通用户的命令行前面的符号是 $ 。例如：`weijiew@LAPTOP-PJ3DJQFQ:~$` 。

root 用户的命令行前面的符号则是 # 。例如：`root@LAPTOP-PJ3DJQFQ:/home/weijiew#`

> 有一件事情是您必须作为根用户才能做的，那就是向sysfs 文件写入内容。系统被挂载在/sys下， sysfs 文件则暴露了一些内核（kernel）参数。 因此，您不需要借助任何专用的工具，就可以轻松地在运行期间配置系统内核。注意 Windows or macOS没有这个文件

## 课后练习

1. 在 /tmp 下新建一个名为 missing 的文件夹。

```sh
weijiew@LAPTOP-PJ3DJQFQ:/tmp$ cd /
weijiew@LAPTOP-PJ3DJQFQ:/$ cd /tmp
weijiew@LAPTOP-PJ3DJQFQ:/tmp$ mkdir missing
```

2. 用 man 查看程序 touch 的使用手册。

`weijiew@LAPTOP-PJ3DJQFQ:/tmp$ man touch`


3. 用 touch 在 missing 文件夹中新建一个叫 semester 的文件。

```sh
weijiew@LAPTOP-PJ3DJQFQ:/tmp$ cd missing
weijiew@LAPTOP-PJ3DJQFQ:/tmp/missing$ touch semester
```

4. 将以下内容一行一行地写入 semester 文件：

```sh
#!/bin/sh
curl --head --silent https://missing.csail.mit.edu
```
第一行可能有点棘手， # 在Bash中表示注释，而 ! 即使被双引号（"）包裹也具有特殊的含义。 
单引号（'）则不一样，此处利用这一点解决输入问题。更多信息请参考 [Bash quoting手册](https://www.gnu.org/software/bash/manual/html_node/Quoting.html)


双引号对于 "$" "`" "\" （启用历史扩展时） "!" 进行翻译。[参考](https://www.gnu.org/software/bash/manual/html_node/Double-Quotes.html#Double-Quotes)。

单引号会保留字符中的所有信息，

> Enclosing characters in single quotes (‘'’) preserves the literal value of each character within the quotes. A single quote may not occur between single quotes, even when preceded by a backslash.

```sh
weijiew@LAPTOP-PJ3DJQFQ:/tmp/missing$ echo '#!/bin/sh' > semester
weijiew@LAPTOP-PJ3DJQFQ:/tmp/missing$ cat semester
#!/bin/sh
weijiew@LAPTOP-PJ3DJQFQ:/tmp/missing$ echo 'curl --head --silent https://missing.csail.mit.edu' >> semester
weijiew@LAPTOP-PJ3DJQFQ:/tmp/missing$ cat semester
#!/bin/sh
curl --head --silent https://missing.csail.mit.edu
```

5. 尝试执行这个文件。例如，将该脚本的路径（./semester）输入到您的shell中并回车。
如果程序无法执行，请使用 ls命令来获取信息并理解其不能执行的原因。

无法执行，ls 查看后发现该文件不具备执行权限。

```sh
weijiew@LAPTOP-PJ3DJQFQ:/tmp/missing$ ./semester
-bash: ./semester: Permission denied
weijiew@LAPTOP-PJ3DJQFQ:/tmp/missing$ ls -l semester
-rw-rw-rw- 1 weijiew weijiew 61 Aug 24 16:45 semester
```

6. 查看 chmod 的手册(例如，使用man chmod命令)

`weijiew@LAPTOP-PJ3DJQFQ:/tmp/missing$ man chmod` 浏览后按 q 退出。

7. 使用 chmod 命令改变权限，使 ./semester 能够成功执行，不要使用sh semester来执行该程序。您的
shell是如何知晓这个文件需要使用sh来解析呢？更多信息请参考：[shebang](https://en.wikipedia.org/wiki/Shebang_(Unix))

```sh
weijiew@LAPTOP-PJ3DJQFQ:/tmp/missing$ chmod +x semester
weijiew@LAPTOP-PJ3DJQFQ:/tmp/missing$ ls -l semester
-rwxrwxrwx 1 weijiew weijiew 61 Aug 24 16:45 semester
weijiew@LAPTOP-PJ3DJQFQ:/tmp/missing$ ./semester
```

8. 使用 | 和 > ，将 semester 文件输出的最后更改日期信息，写入根目录下的 last-modified.txt 的文
件中

最后更改时间：

```sh
weijiew@LAPTOP-PJ3DJQFQ:/tmp/missing$ stat -c %Y semester
1598259525
```

先切换到根目录下，在根目录下创建并修改文件权限。

需要切换到 su 用户下才能在根目录下创建文件。

777 代表三个用户类型都具有可读可写可执行的权限。

```sh
weijiew@LAPTOP-PJ3DJQFQ:/tmp/missing$ cd /
weijiew@LAPTOP-PJ3DJQFQ:/$ su
root@LAPTOP-PJ3DJQFQ:/# touch  last-modified.txt
root@LAPTOP-PJ3DJQFQ:/# ls -l last-modified.txt
-rw-r--r-- 1 root root 0 Aug 24 17:04 last-modified.txt
root@LAPTOP-PJ3DJQFQ:/# chmod 777 last-modified.txt
root@LAPTOP-PJ3DJQFQ:/# ls -l last-modified.txt
root@LAPTOP-PJ3DJQFQ:/# stat -c %Y /tmp/missing/semester > /last-modified.txt
-rwxrwxrwx 1 root root 0 Aug 24 17:04 last-modified.txt
root@LAPTOP-PJ3DJQFQ:/# cat last-modified.txt
1598259525
```

9. 写一段命令来从 /sys 中获取笔记本的电量信息，或者台式机CPU的温度。注意：macOS并没有sysfs，
所以mac用户可以跳过这一题。

路径的 `/sys/class/power_supply` 目录结构如下，笔记本的硬件信息位于此处。

```sh
.
├── ac
│   ├── online
│   └── type
├── battery
│   ├── capacity
│   ├── charge_counter
│   ├── current_now
│   ├── health
│   ├── present
│   ├── status
│   ├── technology
│   ├── temp
│   ├── type
│   └── voltage_now
└── usb
    ├── online
    └── type
```

```sh
root@LAPTOP-PJ3DJQFQ:/# cat /sys/class/power_supply/battery/capacity
96
root@LAPTOP-PJ3DJQFQ:/# cat /sys/class/power_supply/battery/health
Good
root@LAPTOP-PJ3DJQFQ:/# cat /sys/class/power_supply/battery/status
Not charging
root@LAPTOP-PJ3DJQFQ:/# cat /sys/class/power_supply/battery/type
Battery
```
