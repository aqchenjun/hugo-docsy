---
source: 第2章 算法分析
title: 第2章 算法分析 Python 数据结构的性能
tags:
  - 书籍
  - python
date: '2023-03-13 16:39'
published: 'true'
abbrlink: 2c6b4f15
cover:
thumbnail:
---

# [[第2章 算法分析]]读书笔记详情之三

## 1 列表

### 1.1 Python 列表操作的大O效率

![列表操作的大O效率](https://thumbsnap.com/i/2DDgfhqi.png)

### 1.2 验证试验：以 pop()和pop(0)为例

```python
import timeit
import matplotlib.pyplot as plt

popend = timeit.Timer(stmt='lst.pop()',setup='from __main__ import lst')
popzero = timeit.Timer(stmt='lst.pop(0)',setup = 'from __main__ import lst')
result_end = []
result_zero = []
for i in range(10000,1000001,10000):
    lst = list(range(i))
    result_end.append(popend.timeit(number=1)*1000)
    result_zero.append(popzero.timeit(number=1)*1000)
plt.figure(figsize=(10,6))
plt.scatter(range(10000,1000001,10000),result_end,label='pop()')
plt.scatter(range(10000,1000001,10000),result_zero,label='pop(0)')
plt.xlabel('列表长度',fontdict={'family':'SimSun','size':10})
plt.xticks(range(10000,1000001,100000))
plt.ylabel('执行时间(单位：毫秒)',fontdict={'family':'SimSun','size':10})
plt.legend(loc='upper left')
plt.show()
```
![](https://thumbsnap.com/i/EfVBorYt.png)

>从上面的图形可以看出，pop()操作在各个列表长度下，耗时稳定，大O效率为O(1)。pop(0)操作，随列表长度的增加，其耗时也相应增加，基本成线性关系，大O效率为O(n)。 

## 2 字典 Python 字典操作的大O效率
![](https://thumbsnap.com/i/Bnu8Xwgo.png)

