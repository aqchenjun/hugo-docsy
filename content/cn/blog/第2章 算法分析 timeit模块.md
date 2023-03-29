---
categories: Python 数据结构与算法分析(第2版)
title: 第2章 算法分析 timeit模块
tags: [ 书籍, python]
date: 2023-03-13
lastmod: 2023-03-22
thumbnail:  
published: "true"
slug: 20230313163912
---


## 1 概述
timeit 模块提供了测量 Python 小段代码执行时间的方法。

1. timeit 模块定义了接受两个参数的 Timer 类，stmt 和 setup。两个参数都是字符串。stmt 是你要计时的语句或者函数，setup 是为stmt 参数语句构建环境的导入语句。
2. 一旦有了 Timer 对象，下面就是调用 timeit()，它接受一个参数，为每个测试中调用被计时语句的次数，默认为一百万次，返回所耗费的秒数。

```python
t = timeit.Timer(stmt='pass', setup='pass')
t.timeit(number=1000000)
```

## 2 实例

```python
import timeit
#测试函数
def test():
    l = []
    for i in range(1000):
        l = l.append(i)
t = timeit.Timer(stmt = 'test()',setup = 'from __main__ import test')
print(t.timeit(number=1000),'milliseconds')
# t.timeit()默认为一百万次，返回所耗费的秒数。当运行次数调整为1000次，返回的即为毫秒。表示函数test()运行1000次的时间

#测试语句
t = timeit.Timer(stmt = 'lst[100]',setup='from __main__ import lst')
result = []
for i in range(10000,1000001,10000):
    lst = list(range(i))        #给出100组列表
    result.append(t.timeit(number = 1000))    #针对每组列表统计运行时间
print(result)

#传递参数
import random
#方法一
t = timeit.Timer(stmt = 'lst[n]',setup='from __main__ import lst,n')    #从 __main__ 中导入变量 lst 和 n
result = []
for i in range(100000,10000001,100000):
    lst = list(range(i))
    n = random.randint(1, i)
    result.append(t.timeit(number = 1))

#方法二，用另外一个函数包装
def foo(lst,n):
    def _foo():
        lst[n]
    return _foo
result = []
for i in range(100000,10000001,100000):
    lst = list(range(i))
    n = random.randint(1, i)
    t = timeit.Timer(stmt = foo(lst,n),setup='from __main__ import foo')    #lst 和 n 赋值应在该语句之前
    result.append(t.timeit(number = 1))
```
