# LAB-2: Bomb Lab

本实验的目的是分析编译好的二进制文件，分析汇编代码，得出要输入字符串。共六个阶段。

## 总结

首先阅读 wirteup 然后阅读 bomb.c 文件，但是其中函数部分并没有详细给出，通过此文件仅仅是知晓主函数的运行流程。具体到每阶段中的函数代码需要我们进行反汇编，通过汇编代码进行分析。

首先进行反汇编 ` objdump -d bomb > tmp.txt` 目的是将汇编代码存入 tmp.txt 中。

该文件共 1742 行，其中 phase_1 阶段的代码从 346 行开始。


```
0000000000400ee0 <phase_1>:
  400ee0:	48 83 ec 08          	sub    $0x8,%rsp
  400ee4:	be 00 24 40 00       	mov    $0x402400,%esi
  400ee9:	e8 4a 04 00 00       	callq  401338 <strings_not_equal>
  400eee:	85 c0                	test   %eax,%eax
  400ef0:	74 05                	je     400ef7 <phase_1+0x17>
  400ef2:	e8 43 05 00 00       	callq  40143a <explode_bomb>
  400ef7:	48 83 c4 08          	add    $0x8,%rsp
  400efb:	c3                   	retq   
```
