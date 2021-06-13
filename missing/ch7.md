# 主题七：调试及性能分析

> “最有效的 debug 工具就是细致的分析，配合恰当位置的打印语句” — Brian Kernighan, Unix 新手入门。

修改日志的颜色，安装错误的级别进行分类。红，黄，白，级别逐渐降低可以迅速定位问题出现的位置。

建议自己尝试一遍 debug 。

ubuntu 自带 python 2.7 ，这个不能卸载，因为 Ubuntu 系统依赖 python 2.7 环境。建议另外安装 python3 环境。

我单独安装了一个 python 3.8 的环境，采用 pip3 下载的 [ipdb](https://pypi.org/project/ipdb/) 进行调试。

虽然这个代码很容易的能后看出第五行数组越界，但是还是建议自己跟踪一遍，学习一下相关命令。以便于后续解决同类问题。

```python
def bubble_sort(arr):
    n = len(arr)
    for i in range(n):
        for j in range(n):
            if arr[j] > arr[j+1]:
                arr[j] = arr[j+1]
                arr[j+1] = arr[j]
    return arr

print(bubble_sort([4, 2, 1, 8, 7, 6]))
```

运行结果是数组越界问题：

```python
$ python /mnt/f/bubble.py
Traceback (most recent call last):
  File "/mnt/f/bubble.py", line 10, in <module>
    print(bubble_sort([4,2,1,8,7,6]))
  File "/mnt/f/bubble.py", line 5, in bubble_sort
    if arr[j] > arr[j+1]:
IndexError: list index out of range
```

开始调试：`$ python3 -m ipdb /mnt/f/bubble.py`

* l(ist) - 显示当前行附近的11行或继续执行之前的显示；
* s(tep) - 执行当前行，并在第一个可能的地方停止
* n(ext) - 继续执行直到当前函数的下一条语句或者 return 语句；
* b(reak) - 设置断点（基于传入对参数）；
* p(rint) - 在当前上下文对表达式求值并打印结果。还有一个命令是pp ，它使用 pprint 打印；
* r(eturn) - 继续执行直到当前函数返回；
* q(uit) - 退出调试器。

风格检查工具：[pylint](https://www.pylint.org/) / [pep8](https://pypi.org/project/pep8/) 。

