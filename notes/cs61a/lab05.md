# Lab 05

## Q1: List Indexing

简单的数组索引，如果越界直接填 `error` 即可。

`python3 ok -q list-indexing -u --local`

答案：[list-indexing.py](https://github.com/weijiew/cs61a/blob/master/lab/lab05/tests/list-indexing.py)

## Q2: Couple

```python
    a = []
    for i in range(len(lst1)):
        a.append([lst1[i], lst2[i]])
    return a
```

# City Data Abstraction

## Q3: Distance

```python
    x1, y1 = get_lat(city1), get_lon(city1)
    x2, y2 = get_lat(city2), get_lon(city2)

    return sqrt((x1 - x2)**2 + (y1 - y2)**2)
```

## Q4: Closer city

```python
    tmp = make_city('tmp', lat, lon)
    dis1 = distance(city1, tmp)
    dis2 = distance(city2, tmp)

    if dis1 > dis2:
        return get_name(city2)
    else:
        return get_name(city1)
```

## Q5: Don't violate the abstraction barrier!

修复之前的代码。

# Trees

## Q6: Nut Finder

注意，需要借助 ADT。

```python
    if label(t) == 'nut':
        return True
    for node in branches(t):
        if nut_finder(node):
            return True
    return False
```

## Q7: Sprout leaves

```python
    if is_leaf(t):
        return tree(label(t),[tree(v) for v in values])
    return tree(label(t),[sprout_leaves(b, values) for b in branches(t)])
```