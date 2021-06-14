# Lab 04

## Q1: Skip Add

```python
    if n <= 0:
        return 0
    return n + skip_add(n - 2)
```


## Q2: Summation

```python
    if (n == 1):
        return term(1)
    return term(n) + summation(n - 1,term)
```

## Q3: GCD

```python
    return gcd(b, a%b) if b > 0 else a 
```

# Tree Recursion

## Q4: Insect Combinatorics

```python
    if (m <= 0 and n <= 0):
        return 0
    if (m == 1 and n == 1):
        return 1
    return paths(m-1,n) + paths(m,n-1)
```

## Q5: Maximum Subsequence

```python
    if n == 0 or l == 0:
        return 0
    a = 10 * max_subseq(n//10,l-1) + (n%10)
    b = max_subseq(n//10, l)
    return max(a,b)
```