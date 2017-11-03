### pythoncode书摘 （Be a better man） python 3

#### 第一章 数据结构和算法

```
>>> items = [1, 10, 7, 4, 5, 9]
>>> head, *tail = items
>>> head
1
>>> tail
[10, 7, 4, 5, 9]
```

> return head + sum(tail) if tail else head  # if tail is True then return head+sum(tail), 
