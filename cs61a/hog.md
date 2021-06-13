# Hog

> 已经花了大约 8 个小时

两个人在玩掷骰子的游戏，每次可以选多个骰子但是不能超过 10 。
每轮投掷的分数为骰子的点数累加和，但是如果其中任何一枚骰子出现 1 点，那么该轮的分数就为 1 。
所以骰子选太多虽然能拿很多的分数，但是出现 1 的概率也会非常大。


# Phase 1: Simulator

## Problem 0

问题 0 是让你通过交互式的命令查看源码的，很简单的两个函数。

首先查看 dice.py 文件，理解内容，然后进行测试 `python3 ok -q 00 -u --local` 判断理解情况。

dice.py 里面就两个函数，其实特别简单。一个用于模拟真实的筛子，另外一个则是用于测试的骰子。

答案：[00.py](https://github.com/weijiew/cs61a/blob/master/proj/hog/tests/00.py)

## Problem 1

这个题很简单！目的是实现 Pig Out 功能。

roll_dice 函数就是来统计分数，只要骰子中含有 1 分数就是 1 ，反之要求和。

根据问题 0 的基础，来实现 roll_dice 函数。注意，需要先了解该函数要实现什么功能。（接下来有我的思考，最好先自己想一遍！）

需要先运行 **python3 ok -q 00 -u --local** 来确保自己已经理解 roll_dice 函数的功能。

答案：[01.py](https://github.com/weijiew/cs61a/blob/master/proj/hog/tests/01.py)

以下是该功能的代码实现，这是我最开始的思路：

```python
    num = 0
    for i in range(0, num_rolls):
        if dice() == 1:
            return 1
        num += dice()
    return num
```

下面这个例子过不去！

```python
>>> roll_dice(2, make_test_dice(4, 6, 1))
? 10
```

改进：

```python
    sum, f = 0, 0
    for i in range(0, num_rolls):
        tmp = dice()
        if tmp == 1:
            f = 1
        sum += tmp
    return 1 if f else sum
```

> python3 ok -q 01 --local

## Problem 2

这个函数就是实现一个整数三次方后各个位数交错加减再加一。也就是体重所述的 Free Bacon 功能。

看明白题意后，根据 `python3 ok -q 02 -u --local` 来验证自己的理解。

经过手算这几道题目肯定能够明白代码的实现逻辑。

答案：[02.py](https://github.com/weijiew/cs61a/blob/master/proj/hog/tests/02.py)

`free_bacon()` 的代码实现如下：

```python
    num, sum, i = pow(score, 3), 0, 0
    while num != 0:
        if i%2 == 1:
            sum -= num % 10
        else:
            sum += num % 10
        i += 1
        num //= 10
    finall = abs(sum) + 1
    return finall
```

`python3 ok -q 02 --local`
## Problem 3

这个函数是用来控制 free bacon 规则的触发条件的，也就是选择骰子数为 0 的话则触发，反之不行。

同样先运行：`python3 ok -q 03 -u --local`

答案：[03.py](https://github.com/weijiew/cs61a/blob/master/proj/hog/tests/03.py)

代码实现：

```python
    if num_rolls == 0:
        return free_bacon(opponent_score)
    else:
        return roll_dice(num_rolls,dice)
```

测试：`python3 ok -q 03 --local` 

## Problem 4

这道题是 Swine Swap 规则的实现方式。

先解锁 `python3 ok -q 04 -u --local`

答案：[04.py](https://github.com/weijiew/cs61a/blob/master/proj/hog/tests/04.py)

`python3 ok -q 04 --local` 通过测试。注意如果用 log10 需要在引入 `from math import log10`

```python
    num = pow(3,player_score + opponent_score)
    a = num % 10
    b = num // pow(10,int(log10(num)))
    return (a == b)    
```

准确的来说不能用 log10 ，因为只能在要求范围内写代码。

不过这个逻辑也很简单，log10 之前没有想到，有点启发。

## Problem 5a

`python3 ok -q 05a -u --local`

答案：[05a.py](https://github.com/weijiew/cs61a/blob/master/proj/hog/tests/05a.py)

代码实现：

```python
    # BEGIN PROBLEM 5
    while (score0 < goal) and (score1 < goal):
        if who == 0:
            dice_num = strategy0(score0, score1)
            curr_score = take_turn(dice_num, score1, dice)
            score0 += curr_score

        if who == 1:
            dice_num = strategy1(score1, score0)
            curr_score = take_turn(dice_num, score0, dice)
            score1 += curr_score

        if is_swap(score0, score1):
            score0, score1 = score1, score0
        who = other(who)
    # END PROBLEM 5
```

测试 `python3 ok -q 05a --local` 通过。

## Problem 5b

在上一题的基础上需要添加几条规则。

`python3 ok -q 05b -u --local`

答案：[05b.py](https://github.com/weijiew/cs61a/blob/master/proj/hog/tests/05b.py)

实现代码：[hog.py](https://github.com/weijiew/cs61a/blob/master/proj/hog/hog.py)
# Phase 2: Commentary

## Problem 6 

添加输出，提高交互性。

`python3 ok -q 06 -u --local`

答案：[06.py](https://github.com/weijiew/cs61a/blob/master/proj/hog/tests/06.py)

```python
def silence(score0, score1):
    """Announce nothing (see Phase 2)."""
    print("Player 0 now has", score0, "and Player 1 now has", score1)
    return silence
```

注意缩进！

```python
    # BEGIN PROBLEM 6
        say = say(score0, score1)
    # END PROBLEM 6
```


## Problem 7

`python3 ok -q 07 -u --local`

答案：[07.py](https://github.com/weijiew/cs61a/blob/master/proj/hog/tests/07.py)

```python
    # BEGIN PROBLEM 7
    def say(score0, score1):
        if who:
            curr_point = score1 - prev_score
            if curr_point > prev_high:
                print(str(curr_point)+" point(s)! That's the biggest gain yet for Player "+str(who))
                return announce_highest(who,curr_point, score1)
            else:
                return announce_highest(who,prev_high, score1)

        else :
            curr_point = score0 - prev_score
            if curr_point > prev_high:
                print(str(curr_point)+" point(s)! That's the biggest gain yet for Player "+str(who))
                return announce_highest(who,curr_point, score0)
            else:
                return announce_highest(who,prev_high, score0)
    return say
    # END PROBLEM 7
```

`python3 ok -q 07 --local` 
## Problem 8

`python3 ok -q 08 -u --local`

答案：[08.py](https://github.com/weijiew/cs61a/blob/master/proj/hog/tests/08.py)

`python ok -q 08 --local` 结果

```python
    # BEGIN PROBLEM 8
    def avg(*args):
        i , sum = 0 , 0
        while i < num_samples:
            sum += g(*args)
            i += 1
        return sum/num_samples
    return avg
    # END PROBLEM 8
```

## Problem 9


```python
    # BEGIN PROBLEM 9
    i = 1
    max = float("-inf")
    max_index = 1
    while i <= 10:
        averaged_roll_dice = make_averaged(roll_dice, num_samples)
        curr_value = averaged_roll_dice(i, dice)
        if curr_value > max:
            max_index = i
            max = curr_value
        i += 1
    return max_index
    # END PROBLEM 9
```

`python ok -q 09 -u --local`