# 主题二：Shell 脚本

> 在这节课中，会展示bash作为脚本语言的一些基础操作，以及几种最常用的shell工具。

Terminal，shell，bash的区别？ 

Terminal 是终端，也就是命令窗口。

shell 是命令行解释器，用于解释命令，根据命令执行相应的程序。

bash 是 shell 的一种，还有 zsh 等等。

shell 也是一门语言，脚本语言，用于创建命令，执行流程文件保存等工作。

赋值 `foo=bar` 语法。

注意不能使用空格分割，bash 会以空格为单位进行解释。
如果以空格进行分割，例如： `foo = bar` bash 会将调用 foo 程序将 = 和 bar 作为参数。

访问变量 `$foo`

单引号和双引号不同，单引号内部的内容会原样输出。双引号则会进行相应的替换。

例如：

```bash
root@LAPTOP-PJ3DJQFQ:/home/weijiew# foo=bar
root@LAPTOP-PJ3DJQFQ:/home/weijiew# echo "$foo"
bar
root@LAPTOP-PJ3DJQFQ:/home/weijiew# echo '$foo'
$foo
```

> 和其他大多数的编程语言一样，bash也支持if, case, while 和 for 这些控制流关键字。

bash 支持函数。

```bash
mcd () {
 mkdir -p "$1"
 cd "$1"
}
```

* $0 - 脚本名
* $1 到 $9 - 脚本到参数。 $1 是第一个参数，依此类推。
* $@ - 所有参数
* $# - 参数个数
* $? - 前一个命令到返回值
* \$$ - 当前脚本到进程识别码
* !! - 完整到上一条命令，包括参数。常见应用：当你因为权限不足执行命令失败时，可以使用 sudo !!
* 再尝试一次。
* $_ - 上一条命令的最

STDOUT 返回输出值。
STDERR 返回错误及错误码。

返回值 0 表示正常执行，非零则不正常。根据返回值可以判断程序是否正常执行。

* 当您通过 $( CMD ) 这样的方式来执行CMD 这个命令时，然后它的输出结果会替换掉 $( CMD ) 。

```bash
root@LAPTOP-PJ3DJQFQ:/home/weijiew# echo "Starting program at $(date)"
Starting program at Mon Aug 24 21:01:58 CST 2020
```

这个例子是使用 grep 搜索字符串 foobar，如果没有找到就将其最为注释追加到文件中。

```bash
#!/bin/bash

echo "Starting program at $(date)" # date会被替换成日期和时间

echo "Running program $0 with $# arguments with pid $$"

for file in $@; do
    grep foobar $file > /dev/null 2> /dev/null
    if [[ $? -ne 0 ]]; then
        echo "File $file does not have any foobar, adding one"
        echo "# foobar" >> "$file"
    fi
done
```

判断语句尽量使用双方括号 [[]]，为了降低犯错的几率。

$? 是上一个命令的返回值，用于判断命令的执行情况，if 中的内容则是判断返回值是否等 0 。

ne 代表 no equal ，也就是不相等。

执行脚本的时候需要提供相应的参数，脚本会匹配相应的参数，这叫做 shell 的通配。

花括号： {} 用于匹配文件的特殊部分。

```bash
convert image.{png,jpg}
# 会展开为
convert image.png image.jpg
```

通配符 * 和 ? 前者匹配任意个，后者匹配一个。

脚本不一定非得用 shell 写，也可以采用 python 写脚本。
但是需要在文件的头部写上 [shebang](https://en.wikipedia.org/wiki/Shebang_(Unix)) `#!/usr/local/bin/python`
用于告诉 shell 通过 python 来执行后续内容。

## 查找文件

类 Unix 系统一般都包含命令 find 用于文件查找。


```bash
# 查找所有名称为src的文件夹
find . -name src -type d
# 查找所有文件夹路径中包含test的python文件
find . -path '**/test/**/*.py' -type f
# 查找前一天修改的所有文件
find . -mtime -1
# 查找所有大小在500k至10M的tar.gz文件
find . -size +500k -size -10M -name '*.tar.gz'
```

对文件进行操作：

```bash
# Delete all files with .tmp extension
find . -name '*.tmp' -exec rm {} \;
# Find all PNG files and convert them to JPG
find . -name '*.png' -exec convert {} {.}.jpg \;
```

## 查找代码

grep 
  -C: 获取查找结果的上下文。(Context)
  -v：对结果进行反选，即输出不匹配的结果。 (Invert)
  -C 5 ： 输出匹配结果的前后 5 行 。
  -R： 递归进入子目录并搜索所有文本文件。

```bash
# 查找所有使用了 requests 库的文件
rg -t py 'import requests'
# 查找所有没有写 shebang 的文件（包含隐藏文件）
rg -u --files-without-match "^#!"
# 查找所有的foo字符串，并打印其之后的5行
rg foo -A 5
# 打印匹配的统计信息（匹配的行和文件的数量）
rg --stats PATTERN
```

## 查找 shell 命令

使用 history 查找输入命令的历史。

和 grep 结合，`。 history | grep find` 查找包含 find 子串的命令。

在命令的开头加上空格，该命令就不会被记录，使用 history 查不到。类似于浏览器不记录浏览记录的功能。


## 文件夹导航

Fasd 根据使用频率和时效对文件进行排序。

## 课后练习


1. 这道题不是要求你去网上搜索每一问该怎么写，而是根据命令帮助去查阅每一个参数的含义，根据含义来操作。管中窥豹，其他的命令也是通过这样学习的。英文很重要！
   1. ls -a 列出所有文件包含隐藏文件，参数 a 表示 all 。
   2. ls -lh 参数 h 表示  human-readable ，易于人阅读的方式输出。
   3. ls -lt 参数 t 表示，安装最后的访问时间排序。
   4. ls --color=auto 设置颜色。

建议使用别名来简化操作。例如： `alias ll='ls -aGhlt'` 。

2. marco.sh 内容如下，通过 source march.sh 加载函数。

```shell
#!/bin/bash
marco(){
        foo=$(pwd)
        export MARCO=$foo
}
polo(){
        cd "$MARCO" || echo "cd error"
}
```

4.`fd -e html -0 | xargs -0 zip output.zip`

5. `fd . -0 -t f | xargs -0 stat -f '%m%t%Sm %N' | sort -n | cut -f2- | tail -n 1` 
