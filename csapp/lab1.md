# LAB-1: Data Lab

建议先阅读 CS:APP 第二章。

## 1. 阅读

如果你是教师的话，先阅读这个 [README-datalab.txt](https://github.com/weijiew/CSAPP-Labs/blob/main/lab1/README-datalab.txt)，里面包含了整个 lab 是怎么制作的一些信息。

如果你是学生并且想直接做实验，阅读教师的指导会浪费时间，建议直接阅读这个内容：[datalab-handout/README](https://github.com/weijiew/CSAPP-Labs/blob/main/lab1/datalab-handout/README) 。

此时就已经知晓整个实验的流程，以及文件中一些工具的使用。

## 2. 内容总结

总结：

1. `make btest` 编译
2. `./btest` 运行

这个错误不用理会。

```
gcc -O -Wall -m32 -lm -o btest bits.c btest.c decl.c tests.c
btest.c: In function 'test_function':
btest.c:332:23: warning: 'arg_test_range[1]' may be used uninitialized in this function [-Wmaybe-uninitialized]
  332 |     if (arg_test_range[1] < 1)
      |         ~~~~~~~~~~~~~~^~~
```

## 3. 问题

## 3.1. bitXor

题意为通过按位与(&)和取反操作(~)实现异或运算(^)。

异或运算： 1^0 = 1, 0^1 = 1 其他 0^0 = 0, 1^1 = 0均是 0 。

& 是按位与运算符，也就是两个 1 才是 1 ，其他都是 0 。

~ 是取反操作，也就是 NOT 0 变 1 ，1 变 0 。

```c
int bitXor(int x, int y) {
  return ~(~x&~y)&~(x&y);
}
```

## 3.2 Tmin

求补码的最小值，补码最高位是符号位，0 表示正数，1 表示负数。

int 共 32 位，1 左移 31 位即为最大值。

首先第 32 位是 1 表示是负数，而后续位数不能出现 1 因为出现即意味着加法，这将会使得值变大。

```c
int tmin(void) {
  return 1 << 31;
}
```

## 3.3 isTmax

判断输入的 x 在补码下是否是最大值，如果是返回 1 反之返回 0 .

补码下的最大值即首位是 0 ，后续的 31 位均是 1 .

假设输入的 x 是最大值。二进制表示则是：`01111111...`（共 31 个 1）

`i = x + 1` 的二进制表示则是 `100000...` 共 31 个 0 . 此时在补码下 i 为最小值。

`x + i` 则变为全是 `111111111...` 共 32 个 1 . 在补码下表示 -1 。

`x ~= x` 对 x 进行取反，此时 x 全是 0 .

以上情况基于输入的 x 是最大值，输出是 0 . 其他输入最终得到的结果均是 1 ，但是有个特例 -1 .

-1 的补码表示比较特殊， `1111...` 也就是 32 个 1 .

假设输入的 x 是 -1 . 那么 `i = x + 1 ` 因为 x 32 全是 1 那么加一后会导致溢出，第 33 位是 1 后续的 32 位是 0 .进而导致截断，也就是 32 的 0 .最终结果是 0 .

而如果不针对 x = -1 处理的话则会输出 1 ，所以要排除 x = -1 的情况。

`i = !i` 中 ! 是逻辑运算符而非位运算符，也就是 i 是 True 那么 !i 则是 False . 正常情况下 i 是 `1000` 31 个 0 , 也就是 1 .逻辑取反后变成了 0 。那么 `x = x + i` 将不会对 x 产生任何作用。而当输入的 x 是 -1 时，i 将变为 `00000....` 31 个零。此时逻辑取反后将变为 1 ，而 `x = x + i`  将会对使得 x 不再是 0 .

因为题目中要求输入是最大值时输出 1 ，而此时 x 为 0 ，那么再逻辑取反即可。

```c
int isTmax(int x) {
  int i = x + 1; // x:01111111 i:10000000
  x += i;       //  x:11111111
  x = ~x;       //  x:00000000
  i = !i;       // 如果 x = -1 此时 i 将从 0 变为 1 . 排除 -1 的情况 ，-1 的补码也是 11111111 
  x = x + i;    // 因为 i 不再是 0 所以对 x 产生了影响。
  return !x;
}
```

总结：判断输入的 x 是否为补码的最大值。

1. 先变加一使得变为补码的最小值。
2. 然后补码的最小值和最大值相加变成了 32 位的 1 。
3. 此时再按位取变为全是 0 。
4. 以上操作无法避免 -1 ，所以需要增加最后两行措施。


## 3.4 allOddBits

题意：判断所有的奇数位是否都等于 1 是的话 return 1 .

构造一个奇数位全是 1 的掩码（mask），然后和 x 进行按位与，使得 x 的偶数位全部置为零。

之后再同 mask 进行异或运算，也就是逐位比对，最终全部置零，再逻辑取反即可。

```c
int allOddBits(int x) {
  int mask = 0xAA+(0xAA<<8);
  mask=mask+(mask<<16);
  return !((mask&x)^mask);
}
```

OXAA 中 A 是十六进制，十进制下是 10 而二进制表示则为 1010 .所以前两行的目的在于构造 32 位的 奇数全是 1 的二进制数。

## 3.5 negate

返回输入值的负数。

```c
int negate(int x) {
  return ~x + 1;
}
```

按位取反再加一即可。

## 3.6 isAsciiDigit

判断输入的 x 是否是 Ascii 码中 0-9 的值。

其实就是判断一个数字是否在范围 [0x30, 0x39] 内。0x30 的二进制是 48 ，而 0x39 则是 58 .

其实就是和最大值最小值做差，然后判断符号位即可。

```c
int isAsciiDigit(int x) {
  return (!((x+(~48+1))>>31))&!!((x+(~58+1))>>31);
}
```

## 3.7 conditional

用位运算来实现 `x ? y : z` 操作。

```c
int conditional(int x, int y, int z) {
  x = !!x; // 将输入的数字转换位 0、1 
  x = ~x + 1; // 取负数
  return (x&y)|(~x&z);
}
```

如果输入的 x 是 0 那么 !!x 依旧是 0 ，反之非零数字均转换为 1 .目的在于区分 x 是否为 0 .

之后拿到 x 的负数，因为如果 x 是 0 其负数依旧是 0 ，反之 1 的负数是 -1 也就是，在补码下 32 全是 1 .得到的结果 x 要么 32 全是 0 .要么 32 全是 1 .

上一步的目的是为了将 x 构造出来全是 0 或全是 1 的二进制，在提取 y 和 z 即可，如果 x 全是 1 那么结果就是 y 反之则是 z .

## 3.8 isLessOrEqual

题意：通过位运算实现 <= 操作。

```c
int isLessOrEqual(int x, int y) {
    int i = !((y + (~x + 1)) >> 31); //  计算 y - x 如果 y < x 则 i = 0 反之 i = 1 
    int xsign = x >> 31; // 拿到 x 的符号位 
    int ysign = y >> 31; // 拿到 y 的符号位
    int xbool = !xsign; // 取反，若 x 是负数，则 xbool = 0 反之 xbool = 1
    int ybool = !ysign; // 取反，若 y 是负数，则 ybool = 0 反之 ybool = 1
    int result = (!(xbool ^ ybool)) & i; // 判断两个符号位是否相同，若相同则证明输出 i 即可，反之 i 为 0
    return result | (ybool & !xbool); // 当符号位不同的时候， i 为 0 ，接下来判断 x 的符号位即可。
}
```

根据 x，y是否异号可以分为两种情况，同号的话直接输出结果即可。异号输出 x 的符号位即可，因为异号要么 x < 0 要么 y < 0 .符号位就已经代表了 x y 大小。

[bitwise-less-than-or-equal-to](https://stackoverflow.com/questions/41948852/bitwise-less-than-or-equal-to)

## 3.9 logicalNeg

```c
int logicalNeg(int x) {
  return ((x|(~x+1))>>31)+1;
}
```

## 3.10 howManyBits

判断输入的数字在二进制下占几位。



## 3.11 floatScale2

## 3.12 floatFloat2Int

## 3.13 floatPower2

# 参考

1. [CSAPP 之 DataLab详解，没有比这更详细的了](https://zhuanlan.zhihu.com/p/59534845)