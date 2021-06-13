# Lab 01

[地址：](https://inst.eecs.berkeley.edu/~cs61a/sp20/lab/lab01/)

# Quick Logistics Review

之前使用过命令行来测试程序，但是很多参数其实没有介绍，这里提供了介绍。

以下是一些我的总结，想要查看更多的信息还请查看网站获取。

* `python3 -i` 在命令行中创建一个python3 的一个交互使用方式。

lab 提供提供了测试用例，ok 程序就是用来判定程序是否正确。

* `python3 ok -q <specified function>` 指定一个函数可以调用其注释部分的 doctests 来测试。

默认情况下是只显示错误的哪个用例没有通过，加上参数 -v 后可以查看所有的情况，例如成功通过的例子。

* `python3 ok -v`

# What Would Python Display? (Part 1)

## Q1: WWPD: Control

> python3 ok -q control -u --local 

代码太长了，去厂库看吧：https://github.com/weijiew/cs61a/blob/master/lab/lab01/tests/control.py

## Q2: WWPD: Veritasiness

> python3 ok -q short-circuit -u --local  

同上：https://github.com/weijiew/cs61a/blob/master/lab/lab01/tests/short-circuit.py

## Q3: Debugging Quiz!

debugg 方面的知识，一共 12 道题！

> python3 ok -q debugging-quiz -u --local

答案：https://github.com/weijiew/cs61a/blob/master/lab/lab01/tests/debugging-quiz.py

## Q4: Fix the Bug

> python3 ok -q both_positive --local

```python
def both_positive(a, b):
    return (a > 0) and (b > 0) # You can replace this line!
```

## Q5: Sum Digits

题意就是将一个非负整数的每一位拆分然后累加。

> 

```python
def sum_digits(x):
    sum = 0
    while x != 0:
        sum += int(x % 10)
        x /= int(10)
    return sum
```

> python3 ok -q sum_digits --local

# Optional Questions

以下是选作的题目：
# What Would Python Display? (Part 2)

## Q6: WWPD: What If?

其实考的就是一点，函数是原样返回值，而 print 则是带有转义。如下：

```python
k = "aaa"

def a(k):
    return k # "aaa"

print(k) # aaa
```
## Q7: Falling Factorial

```python
def falling(n, k):
    sum = 1
    while k:
        sum *= n
        n -= 1
        k -= 1
    return sum
```

> python3 ok -q falling --local

## Q8: Double Eights


```python
def double_eights(n):
    return '88' in str(n)
```

这个写法有点取巧了，下面有个循环的写法。

```python
def double_eights(n):
    while n != 0:
        if n % 100 == 88:
            return True
        n //= 10
    return False
```

> python3 ok -q double_eights --local