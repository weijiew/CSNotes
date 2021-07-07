# LAB-2: Bomb Lab

建议先阅读 CS:APP 第三章。

> 今天又发现了一个写代码比较好用的方式，用 vscode remote SSH 插件访问虚拟机，可以直接在 vscode coding 。
> 具体可以参考这篇文档：https://code.visualstudio.com/docs/remote/ssh#_getting-started
> vscode 可以安装 x86 and x86_64 Assembly 插件针对 .asm 文件实现语法高亮，方便阅读。

本实验的目的是分析编译好的二进制文件，分析汇编代码，得出要输入字符串。共六个阶段。

其实就是根据二进制进行反汇编，分析汇编代码得到输入的字符串。

首先阅读 wirteup 然后阅读 bomb.c 文件，但是其中函数部分并没有详细给出，通过此文件仅仅是知晓主函数的运行流程。具体到每阶段中的函数代码需要我们进行反汇编，通过汇编代码分析程序意图得到要输入的字符串。

下面这张图很重要，需要经常查阅。

![image](https://raw.githubusercontent.com/weijiew/pic/master/images/image.5ibn2o8lfv40.png)

## 1. phase_1

首先进行反汇编 ` objdump -d bomb > tmp.asm` 目的是将汇编代码存入 tmp.asm 中。

可以使用 Ctrl + F 定位，查询关键字 phase_1 即可。

该文件共 1742 行，其中 phase_1 阶段的代码从 346 行开始。

$ 前缀表示立即数，立即数就是字面值。

% 前缀表示寄存器， 0x 前缀表示 16 进制。

```
0000000000400ee0 <phase_1>:
  400ee0:	48 83 ec 08          	sub    $0x8,%rsp    
  400ee4:	be 00 24 40 00       	mov    $0x402400,%esi 
  400ee9:	e8 4a 04 00 00       	callq  401338 <strings_not_equal>
  400eee:	85 c0                	test   %eax,%eax
  400ef0:	74 05                	je     400ef7 <phase_1+0x17>
  400ef2:	e8 43 05 00 00       	callq  40143a <explode_bomb> 
  400ef7:	48 83 c4 08           add    $0x8,%rsp 
  400efb:	c3                   	retq   
```

大致流程是将位于内存地址 0x402400 处的字符串作为参数传入，调用函数 strings_not_equal 比对。字符串相等返回 1 不等返回 0 结果存入 eax 中。比对 eax 结果进行跳转。下面逐行分析：

其他先不看，先看首尾两条指令，这两条指令用于在栈中开辟空间。

  400ee0:	48 83 ec 08          	sub    $0x8,%rsp    
  ...
  400ef7:	48 83 c4 08           add    $0x8,%rsp 

`sub    $0x8,%rsp` 功能是减法操作，等价于 `$0x8` 减去 `%rsp` 寄存器中内容并将其放入 `%rsp` 中。rsp 寄存器中存放的内容表示栈指针，该语句的目的其实是开辟了 8 字节的内存。我看到有的文章中写道，申请的这块空间并未使用，原因是编译器为了提高效率做了内存对齐处理。

在内存中栈是自上向下生长，“上边”地址属于高字节，“下边”地址属于低字节。而 rsp 寄存器中存放的是栈顶指针。所以栈空时 rsp 中存放的值最大，栈满时 rsp 中的值最小。也就是当向栈中存放内容时 rsp 的值减小。这部分的具体内容可以参考 3.4.4 节。

![image-1](https://cdn.jsdelivr.net/gh/weijiew/pic@master/images/image.12ui0c2ctrfk.png)

`mov    $0x402400,%esi` 这条指令的意思是将立即数 0x402400 存入寄存器 esi 中。根据书中（P132页）可知，寄存器 %esi 中存储的是函数的第二个参数。

由 bomb.c 文件可知第一个参数是我们需要寻找的字符串。函数签名如下：

```c
   input = read_line();
   phase_1(input);
   phase_defused();      
```

`callq  401338 <strings_not_equal>` 意思是调用 strings_not_equal 函数用于判断字符串是否相等。

	test   %eax,%eax
  je     400ef7 <phase_1+0x17>

je 指令用于判断寄存器中的值是否等于 0 。如果是 0 跳转反之爆炸。

%eax 中存放的是函数返回值。

所以其实就是输入的字符串和存储在 $0x402400 地址中的字符串进行比对。那么直接用 gdb 查询即可。

```sh
weijiew@ubuntu:/mnt/hgfs/CSAPP-Labs/lab2/bomb$ gdb bomb

...
(gdb) x/s 0x402400
0x402400:       "Border relations with Canada have never been better."
```

测试：

```sh
$ ./bomb
Welcome to my fiendish little bomb. You have 6 phases with
which to blow yourself up. Have a nice day!
Border relations with Canada have never been better.
Phase 1 defused. How about the next one?
```

好了 phase_1 搞定！

当然也可以用断点。建议熟悉一下 gdb 调试。

先进入 GDB：

$ gdb bomb

打上两个断点，目的是使得程序停在断点处。

  (gdb) break phase_1 
  if      task    thread
  (gdb) break explode_bomb 
  if      task    thread

然后开始调试程序，随便输入个字符 aaa 先让程序跑起来。

  (gdb) run
  Welcome to my fiendish little bomb. You have 6 phases with
  which to blow yourself up. Have a nice day!
  aaa

程序停在了第二个断点处，为什么不是第一个断点，因为第一个断点在第二个断点之后才执行。

然后输入 disas 查看当前代码， 箭头 => 表示执行当前代码所处位置：

  Breakpoint 2, 0x0000000000400ee0 in phase_1 ()
  (gdb) disas
  Dump of assembler code for function phase_1:
  => 0x0000000000400ee0 <+0>:     sub    $0x8,%rsp
    0x0000000000400ee4 <+4>:     mov    $0x402400,%esi
    0x0000000000400ee9 <+9>:     callq  0x401338 <strings_not_equal>
    0x0000000000400eee <+14>:    test   %eax,%eax
    0x0000000000400ef0 <+16>:    je     0x400ef7 <phase_1+23>
    0x0000000000400ef2 <+18>:    callq  0x40143a <explode_bomb>
    0x0000000000400ef7 <+23>:    add    $0x8,%rsp
    0x0000000000400efb <+27>:    retq
  End of assembler dump.

stepi 表示执行下一条指令。也就是单步调试。用 disas 查看可以看到箭头前进了一步。

  (gdb) stepi
  0x0000000000400ee4 in phase_1 ()
  (gdb) disas
  Dump of assembler code for function phase_1:
    0x0000000000400ee0 <+0>:     sub    $0x8,%rsp
  => 0x0000000000400ee4 <+4>:     mov    $0x402400,%esi
    0x0000000000400ee9 <+9>:     callq  0x401338 <strings_not_equal>
    0x0000000000400eee <+14>:    test   %eax,%eax
    0x0000000000400ef0 <+16>:    je     0x400ef7 <phase_1+23>
    0x0000000000400ef2 <+18>:    callq  0x40143a <explode_bomb>
    0x0000000000400ef7 <+23>:    add    $0x8,%rsp
    0x0000000000400efb <+27>:    retq
  End of assembler dump.

执行到 mov 处，此时已经把数据加载到寄存器 esi 中。所以查看 esi 中的内容即可。

  (gdb) disas
  Dump of assembler code for function phase_1:
    0x0000000000400ee0 <+0>:     sub    $0x8,%rsp
    0x0000000000400ee4 <+4>:     mov    $0x402400,%esi
  => 0x0000000000400ee9 <+9>:     callq  0x401338 <strings_not_equal>
    0x0000000000400eee <+14>:    test   %eax,%eax
    0x0000000000400ef0 <+16>:    je     0x400ef7 <phase_1+23>
    0x0000000000400ef2 <+18>:    callq  0x40143a <explode_bomb>
    0x0000000000400ef7 <+23>:    add    $0x8,%rsp
    0x0000000000400efb <+27>:    retq
  End of assembler dump.
  (gdb) x/s $esi
  0x402400:       "Border relations with Canada have never been better."

## 2. phase_2

> 这一节考察的是循环

下面的代码是根据 gdb 的 disas 指令查看得来的。区别是能看到相对地址，而反汇编得到的没有相对地址。只是方便阅读。



```sh
(gdb) disas
Dump of assembler code for function phase_2:
=> 0x0000000000400efc <+0>:     push   %rbp
   0x0000000000400efd <+1>:     push   %rbx
   0x0000000000400efe <+2>:     sub    $0x28,%rsp
   0x0000000000400f02 <+6>:     mov    %rsp,%rsi
   0x0000000000400f05 <+9>:     callq  0x40145c <read_six_numbers>
   0x0000000000400f0a <+14>:    cmpl   $0x1,(%rsp)
   0x0000000000400f0e <+18>:    je     0x400f30 <phase_2+52>
   0x0000000000400f10 <+20>:    callq  0x40143a <explode_bomb>
   0x0000000000400f15 <+25>:    jmp    0x400f30 <phase_2+52>
   0x0000000000400f17 <+27>:    mov    -0x4(%rbx),%eax
   0x0000000000400f1a <+30>:    add    %eax,%eax
   0x0000000000400f1c <+32>:    cmp    %eax,(%rbx)
   0x0000000000400f1e <+34>:    je     0x400f25 <phase_2+41>
   0x0000000000400f20 <+36>:    callq  0x40143a <explode_bomb>
   0x0000000000400f25 <+41>:    add    $0x4,%rbx
   0x0000000000400f29 <+45>:    cmp    %rbp,%rbx
   0x0000000000400f2c <+48>:    jne    0x400f17 <phase_2+27>
   0x0000000000400f2e <+50>:    jmp    0x400f3c <phase_2+64>
   0x0000000000400f30 <+52>:    lea    0x4(%rsp),%rbx       
   0x0000000000400f35 <+57>:    lea    0x18(%rsp),%rbp      
   0x0000000000400f3a <+62>:    jmp    0x400f17 <phase_2+27>
   0x0000000000400f3c <+64>:    add    $0x28,%rsp
   0x0000000000400f40 <+68>:    pop    %rbx
   0x0000000000400f41 <+69>:    pop    %rbp
   0x0000000000400f42 <+70>:    retq
```

下面来逐行解释，

首先看函数的开头结尾，中间部分已经省略，这部分内容用于完成函数的初始化工作。

rbp, rbx 属于通用寄存器。因为接下来的函数调用中需要使用这两个寄存器，但是其中存有之前的数据，所以使用之前需要将其中的数据保存到栈中，当函数结束后再将其复原。

rsp 之前已经解释过了，表示存放栈顶的指针。

=> 0x0000000000400efc <+0>:     push   %rbp
   0x0000000000400efd <+1>:     push   %rbx
   0x0000000000400efe <+2>:     sub    $0x28,%rsp
   0x0000000000400f02 <+6>:     mov    %rsp,%rsi
   ....................
   0x0000000000400f3c <+64>:    add    $0x28,%rsp
   0x0000000000400f40 <+68>:    pop    %rbx
   0x0000000000400f41 <+69>:    pop    %rbp


   0x0000000000400f05 <+9>:     callq  0x40145c <read_six_numbers>

看接下来的代码，由函数名可知，输入的是 6 个整数。

   0x0000000000400f0a <+14>:    cmpl   $0x1,(%rsp)
   0x0000000000400f0e <+18>:    je     0x400f30 <phase_2+52>
   0x0000000000400f10 <+20>:    callq  0x40143a <explode_bomb>

比较存放在 rsp 内存地址中所存储的值是否为 1 。如果是就跳转到 <+52> 处，反之爆炸（<+52> 是当前函数中的相对地址）。

据此输入的第一个字符是 1 . je 指令表示相等或等零时才进行跳转。

   0x0000000000400f30 <+52>:    lea    0x4(%rsp),%rbx       
   0x0000000000400f35 <+57>:    lea    0x18(%rsp),%rbp      
   0x0000000000400f3a <+62>:    jmp    0x400f17 <phase_2+27>

> 先简要介绍一下 lea 指令。lea 做地址计算，但不会访问内存。(由上面公式算出地址值)。lea指令只有leaq,没有其它变种，它并不是从地址中读取值，而是将算出的地址值写入。它是用来进行地址计算的，所以也不修改条件码。

0x4(%rsp) 表示间接引用，也就是寄存器值加上 0x4 处的内存地址的值。

其实就是计算内存地址。第一个操作数是一个表达式，表示计算得到的内存地址并存入第二个操作数的寄存器中。注意和 mov 不同的时 lea 是针对地址的字面值，而 mov 是针对该地址中所存储的内容。也就是 rsp + 0x4 的地址存入 rbx 中。 rsp + 0x18 地址存入 rbp 中。然后跳转到 <+27> 中。


   0x0000000000400f17 <+27>:    mov    -0x4(%rbx),%eax
   0x0000000000400f1a <+30>:    add    %eax,%eax
   0x0000000000400f1c <+32>:    cmp    %eax,(%rbx)
   0x0000000000400f1e <+34>:    je     0x400f25 <phase_2+41>
   0x0000000000400f20 <+36>:    callq  0x40143a <explode_bomb>

此时将 rbx - 4 的地址存入 eax 中，注意 rbx 是由 rsp + 4 得来，所以 eax 本质上存的其实是 rsp 。

然后是 eax += eax ，也就是乘二。紧接着判断 eax 和 rbx 中存储的值是否相等。如果相等就跳转至 <+41> 反之爆炸。

共 6 个数字，rbx 中存的是当前数字，而 eax 中存储的是上一个数字。然后比对上一个数字是否是当前数字的二倍。

   0x0000000000400f25 <+41>:    add    $0x4,%rbx
   0x0000000000400f29 <+45>:    cmp    %rbp,%rbx
   0x0000000000400f2c <+48>:    jne    0x400f17 <phase_2+27>
   0x0000000000400f2e <+50>:    jmp    0x400f3c <phase_2+64>

跳转到 41 处后 rbx += 4 ，然后比对 rbp 和 rbx 是否相等，不相等就跳入 27 循环，反之循环结束跳入 64 。

综上可知，rbp 中存放的是循环结束的值。rbx 表示当前值，而循环过程中都会将 rbx 中的值存入 eax 中，在 eax 中和其本身相加（乘二）然后和 rbx + 4 的位置进行比较。而共 6 个数字，首项为 1 ，显然结果为 `1 2 4 8 16 32` .

## 3. phase_3 

> 这一节考察的是 条件与分支

依旧是找到 phase_3 的代码然后分析，内容如下：

  0000000000400f43 <phase_3>:
    400f43:	48 83 ec 18          	sub    $0x18,%rsp
    400f47:	48 8d 4c 24 0c       	lea    0xc(%rsp),%rcx
    400f4c:	48 8d 54 24 08       	lea    0x8(%rsp),%rdx
    400f51:	be cf 25 40 00       	mov    $0x4025cf,%esi
    400f56:	b8 00 00 00 00       	mov    $0x0,%eax
    400f5b:	e8 90 fc ff ff       	callq  400bf0 <__isoc99_sscanf@plt>
    400f60:	83 f8 01             	cmp    $0x1,%eax
    400f63:	7f 05                	jg     400f6a <phase_3+0x27>
    400f65:	e8 d0 04 00 00       	callq  40143a <explode_bomb>
    400f6a:	83 7c 24 08 07       	cmpl   $0x7,0x8(%rsp)
    400f6f:	77 3c                	ja     400fad <phase_3+0x6a>
    400f71:	8b 44 24 08          	mov    0x8(%rsp),%eax
    400f75:	ff 24 c5 70 24 40 00 	jmpq   *0x402470(,%rax,8)
    400f7c:	b8 cf 00 00 00       	mov    $0xcf,%eax
    400f81:	eb 3b                	jmp    400fbe <phase_3+0x7b>
    400f83:	b8 c3 02 00 00       	mov    $0x2c3,%eax
    400f88:	eb 34                	jmp    400fbe <phase_3+0x7b>
    400f8a:	b8 00 01 00 00       	mov    $0x100,%eax
    400f8f:	eb 2d                	jmp    400fbe <phase_3+0x7b>
    400f91:	b8 85 01 00 00       	mov    $0x185,%eax
    400f96:	eb 26                	jmp    400fbe <phase_3+0x7b>
    400f98:	b8 ce 00 00 00       	mov    $0xce,%eax
    400f9d:	eb 1f                	jmp    400fbe <phase_3+0x7b>
    400f9f:	b8 aa 02 00 00       	mov    $0x2aa,%eax
    400fa4:	eb 18                	jmp    400fbe <phase_3+0x7b>
    400fa6:	b8 47 01 00 00       	mov    $0x147,%eax
    400fab:	eb 11                	jmp    400fbe <phase_3+0x7b>
    400fad:	e8 88 04 00 00       	callq  40143a <explode_bomb>
    400fb2:	b8 00 00 00 00       	mov    $0x0,%eax
    400fb7:	eb 05                	jmp    400fbe <phase_3+0x7b>
    400fb9:	b8 37 01 00 00       	mov    $0x137,%eax
    400fbe:	3b 44 24 0c          	cmp    0xc(%rsp),%eax
    400fc2:	74 05                	je     400fc9 <phase_3+0x86>
    400fc4:	e8 71 04 00 00       	callq  40143a <explode_bomb>
    400fc9:	48 83 c4 18          	add    $0x18,%rsp
    400fcd:	c3                   	retq   

第四行的 mov    $0x4025cf,%esi 很突兀，用 gdb 把内存地址 $0x4025cf 打印出来看看，结果如下：

  (gdb) x/sb 0x4025cf
  0x4025cf:       "%d %d"

由此可见输入的个数为两个整数。然后看接下来的代码

    400f51:	be cf 25 40 00       	mov    $0x4025cf,%esi
    400f56:	b8 00 00 00 00       	mov    $0x0,%eax
    400f5b:	e8 90 fc ff ff       	callq  400bf0 <__isoc99_sscanf@plt>
    400f60:	83 f8 01             	cmp    $0x1,%eax
    400f63:	7f 05                	jg     400f6a <phase_3+0x27>
    400f65:	e8 d0 04 00 00       	callq  40143a <explode_bomb>
    400f6a:	83 7c 24 08 07       	cmpl   $0x7,0x8(%rsp)

将 eax 中的值改为 0 ，然后调用 sscanf 函数，eax 寄存器中存放的是函数的返回值。

然后比对 eax 中的值是否大于 1 表示 sscanf 调用成功进而跳入 400f6a ，反之爆炸。

    400f6a:	83 7c 24 08 07       	cmpl   $0x7,0x8(%rsp)
    400f6f:	77 3c                	ja     400fad <phase_3+0x6a>
    400f71:	8b 44 24 08          	mov    0x8(%rsp),%eax

第一个数如果大于 7 则爆炸。反之进入分支，下面是 0 - 7 共八个分支。

    400f75:	ff 24 c5 70 24 40 00 	jmpq   *0x402470(,%rax,8)
    400f7c:	b8 cf 00 00 00       	mov    $0xcf,%eax
    400f81:	eb 3b                	jmp    400fbe <phase_3+0x7b>
    400f83:	b8 c3 02 00 00       	mov    $0x2c3,%eax
    400f88:	eb 34                	jmp    400fbe <phase_3+0x7b>
    400f8a:	b8 00 01 00 00       	mov    $0x100,%eax
    400f8f:	eb 2d                	jmp    400fbe <phase_3+0x7b>
    400f91:	b8 85 01 00 00       	mov    $0x185,%eax
    400f96:	eb 26                	jmp    400fbe <phase_3+0x7b>
    400f98:	b8 ce 00 00 00       	mov    $0xce,%eax
    400f9d:	eb 1f                	jmp    400fbe <phase_3+0x7b>
    400f9f:	b8 aa 02 00 00       	mov    $0x2aa,%eax
    400fa4:	eb 18                	jmp    400fbe <phase_3+0x7b>
    400fa6:	b8 47 01 00 00       	mov    $0x147,%eax
    400fab:	eb 11                	jmp    400fbe <phase_3+0x7b>
    400fad:	e8 88 04 00 00       	callq  40143a <explode_bomb>
    400fb2:	b8 00 00 00 00       	mov    $0x0,%eax
    400fb7:	eb 05                	jmp    400fbe <phase_3+0x7b>
    400fb9:	b8 37 01 00 00       	mov    $0x137,%eax

符合条件进入后，将某个值存入 eax 中。

    400fbe:	3b 44 24 0c          	cmp    0xc(%rsp),%eax
    400fc2:	74 05                	je     400fc9 <phase_3+0x86>
    400fc4:	e8 71 04 00 00       	callq  40143a <explode_bomb>
    400fc9:	48 83 c4 18          	add    $0x18,%rsp
    400fcd:	c3                   	retq   

然后用输入的数字同 eax 存储的内容进行比对。也就是第一个数字进入分支，第二个数字是必须和分支内的数字等同。而代码中的第二个数字是立即数为十六进制，转为十进制即可，所以问题转换为 8 个数字匹配，gdb 单步调试即可，下面给出一个答案，共有八个答案。

  4 389

## 4. phase_4

依旧是查看代码：

  000000000040100c <phase_4>:
    40100c:	48 83 ec 18          	sub    $0x18,%rsp
    401010:	48 8d 4c 24 0c       	lea    0xc(%rsp),%rcx
    401015:	48 8d 54 24 08       	lea    0x8(%rsp),%rdx
    40101a:	be cf 25 40 00       	mov    $0x4025cf,%esi
    40101f:	b8 00 00 00 00       	mov    $0x0,%eax
    401024:	e8 c7 fb ff ff       	callq  400bf0 <__isoc99_sscanf@plt>
    401029:	83 f8 02             	cmp    $0x2,%eax
    40102c:	75 07                	jne    401035 <phase_4+0x29>
    40102e:	83 7c 24 08 0e       	cmpl   $0xe,0x8(%rsp)
    401033:	76 05                	jbe    40103a <phase_4+0x2e>
    401035:	e8 00 04 00 00       	callq  40143a <explode_bomb>
    40103a:	ba 0e 00 00 00       	mov    $0xe,%edx
    40103f:	be 00 00 00 00       	mov    $0x0,%esi
    401044:	8b 7c 24 08          	mov    0x8(%rsp),%edi
    401048:	e8 81 ff ff ff       	callq  400fce <func4>
    40104d:	85 c0                	test   %eax,%eax
    40104f:	75 07                	jne    401058 <phase_4+0x4c>
    401051:	83 7c 24 0c 00       	cmpl   $0x0,0xc(%rsp)
    401056:	74 05                	je     40105d <phase_4+0x51>
    401058:	e8 dd 03 00 00       	callq  40143a <explode_bomb>
    40105d:	48 83 c4 18          	add    $0x18,%rsp
    401061:	c3                   	retq   

## 参考

1. [CSAPP实验-BombLab](https://blog.csdn.net/aawoe/article/details/107214522)
2. [csapp字符驱动实验_超精讲-逐例分析 CSAPP：Lab2-Bomb!(上)](https://blog.csdn.net/weixin_42363874/article/details/113053235)
3. [【读厚 CSAPP】II Bomb Lab](https://wdxtub.com/csapp/thick-csapp-lab-2/2016/04/16/)
4. [《深入理解计算机系统》Bomb Lab实验解析](https://earthaa.github.io/2020/01/12/CSAPP-Bomblab/)



