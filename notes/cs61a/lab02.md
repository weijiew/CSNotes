# Lab 02

## Q1: WWPD: Lambda the Free

`python3 ok -q lambda -u --local`

[lambda.py](https://github.com/weijiew/cs61a/blob/master/lab/lab02/tests/lambda.py)

记住如果觉得有问题就输入 `Error` :)

## Q2: WWPD: Higher Order Functions

`python3 ok -q hof-wwpd -u --local`

[hof-wwpd.py](https://github.com/weijiew/cs61a/blob/master/lab/lab02/tests/hof-wwpd.py)

## Q3: Lambdas and Currying

```python
def lambda_curry2(func):
    return lambda x: lambda y: func(x, y)
```

测试：`python3 ok -q lambda_curry2 --local`

## Q4: Count van Count

首先返回值是一个函数，所以需要在内部重新定义一个函数用于返回。

```python
def count_cond(condition):
    def count(n):
        i, count = 1, 0
        while i <= n:
            if condition(n, i):
                count += 1
            i += 1
        return count
    return count
```

`python3 ok -q count_cond --local`
## Q5: Both Paths

```python
def both_paths(sofar="S"):
    print(sofar)
    def left():
        return both_paths(sofar+'L')
    def right():
        return both_paths(sofar+'R')
    return left,right
```

## Q8: Composite Identity Function

```python
def composite_identity(f, g):
    return lambda x : f(g(x)) == g(f(x))
```

## Q9: I Heard You Liked Functions...

```python
def cycle(f1, f2, f3):
    def ret_fn(n):
        def a(x):
            i = 0
            while (i < n):
                if (i % 3 == 0):
                    x = f1(x)
                elif (i % 3 == 1):
                    x = f2(x)
                elif (i % 3 == 2):
                    x = f3(x)
                i += 1
            return x
        return a
    return ret_fn 
```
