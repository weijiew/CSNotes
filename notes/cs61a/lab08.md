# Lab 08

## Q1: Insert

```python
    if index == 0:
        link.rest = Link(link.first,link.rest)
        link.first = value
    elif link.rest is Link.empty:
        raise IndexError
    else:
        insert(link.rest, value, index - 1)
```

## Q2: Determining Efficiency

`python3 ok -q wwpd-efficiency -u --local`

答案：[wwpd-efficiency.py](https://github.com/weijiew/cs61a/blob/master/lab/lab08/tests/wwpd-efficiency.py)

## Q3: Subsequences

```python
def insert_into_all(item, nested_list):
    return [[item] + rest for rest in nested_list]

def subseqs(s):
    if not s:
        return [[]]
    else:
        with_in = insert_into_all(s[0],subseqs(s[1:]))
        with_out = subseqs(s[1:])
        return with_in + with_out
```


先暂停了，没有时间，去考研了，考研成功后再更新！😂