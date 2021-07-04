# LAB-2: Bomb Lab

本实验的目的是分析编译好的二进制文件，分析汇编代码，得出要输入字符串。共六个阶段。

其实就是根据二进制进行反汇编，分析汇编代码得到输入的字符串。

首先阅读 wirteup 然后阅读 bomb.c 文件，但是其中函数部分并没有详细给出，通过此文件仅仅是知晓主函数的运行流程。具体到每阶段中的函数代码需要我们进行反汇编，通过汇编代码分析程序意图得到要输入的字符串。

## 1. phase_1

首先进行反汇编 ` objdump -d bomb > tmp.txt` 目的是将汇编代码存入 tmp.txt 中。

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

其他先不看，先看首尾两条指令，这两条指令用于在栈中开辟空间。栈是自上向下生长，所以申请空间需要减去，而函数结束需要将空间释放所以需要加上。

  400ee0:	48 83 ec 08          	sub    $0x8,%rsp    
  ...
  400ef7:	48 83 c4 08           add    $0x8,%rsp 

`sub    $0x8,%rsp` 功能是减法操作，等价于 `$0x8` 减去 `%rsp` 寄存器中内容并将其放入 `%rsp` 中。rsp 寄存器中存放的是栈指针，该语句的目的其实是开辟了 8 字节的内存。我看到有的文章中写道，申请的这块空间并未使用，原因是编译器为了提高效率做了内存对齐处理。

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
weijiew@ubuntu:/mnt/hgfs/CSAPP-Labs/lab2/bomb$ ./bomb
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

可以使用 Ctrl + F 定位，查询关键字 phase_1 即可，但是这个代码没有相对地址。

下面的代码是根据 gdb 的 disas 指令查看得来的。

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

首先看函数的开头结尾，中间部分已经省略，这部分内容用于完成函数的初始化工作，例如开辟栈空间。

=> 0x0000000000400efc <+0>:     push   %rbp
   0x0000000000400efd <+1>:     push   %rbx
   0x0000000000400efe <+2>:     sub    $0x28,%rsp
   0x0000000000400f02 <+6>:     mov    %rsp,%rsi
   ....................
   0x0000000000400f3c <+64>:    add    $0x28,%rsp
   0x0000000000400f40 <+68>:    pop    %rbx
   0x0000000000400f41 <+69>:    pop    %rbp


由函数名可知，输入的是 6 个整数。

   0x0000000000400f05 <+9>:     callq  0x40145c <read_six_numbers>

比较 rsp 的内存地址中的值是否为 1 。如果是就跳转到 52 行，反之爆炸。据此输入的第一个字符是 1 .

   0x0000000000400f0a <+14>:    cmpl   $0x1,(%rsp)
   0x0000000000400f0e <+18>:    je     0x400f30 <phase_2+52>
   0x0000000000400f10 <+20>:    callq  0x40143a <explode_bomb>

把

   0x0000000000400f30 <+52>:    lea    0x4(%rsp),%rbx       
   0x0000000000400f35 <+57>:    lea    0x18(%rsp),%rbp      
   0x0000000000400f3a <+62>:    jmp    0x400f17 <phase_2+27>


## 参考

1. [CSAPP实验-BombLab](https://blog.csdn.net/aawoe/article/details/107214522)
2. [csapp字符驱动实验_超精讲-逐例分析 CSAPP：Lab2-Bomb!(上)](https://blog.csdn.net/weixin_42363874/article/details/113053235)
3. [【读厚 CSAPP】II Bomb Lab](https://wdxtub.com/csapp/thick-csapp-lab-2/2016/04/16/)
