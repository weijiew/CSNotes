# Lab 06

## Q1: Make Adder Increasing

```python
def make_adder_inc(n):
    def myadd(num):
        nonlocal n
        t = n + num
        n += 1
        return t
    return myadd
```

`python3 ok -q make_adder_inc --local`

## Q2: Next Fibonacci

```python
def make_fib():
    n = 0
    m = 1
    index = -1
    def fib():
        nonlocal n,m,index
        index += 1
        if index == 0:
            return n
        if index == 1:
            return m

        res = m + n
        n = m
        m = res
        return res
    return fib
```

## Q3: Scale


```python
def scale(it, multiplier):
    for item in it:
        yield item * multiplier
```

`python3 ok -q scale --local`

## Q4: Hailstone

```python
def hailstone(n):
    yield n
    if n != 1:
        if n & 1:
            yield from hailstone(3 * n + 1)
        else:
            yield from hailstone(n//2)
```
