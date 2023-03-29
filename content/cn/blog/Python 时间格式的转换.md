---
categories: Python 程序语言
title: Python 时间格式的转换
tags: [ 教程, python]
date: 2023-03-12
lastmod: 2023-03-16
thumbnail: https://s1.vika.cn/space/2023/03/12/7951c28f287743db9f0e65c5739d5f96?attname=20230312222408.png
published: "true"
slug: 20230312220449
---

# 《Python 程序语言》读书笔记之三

Python中时间的三种表达方式：  

## 1 时间戳 (timestamp)  

通常来说，时间戳表示的是从1970年1月1日00:00:00开始按秒计算的偏移量。
返回时间戳方式的函数主要有time( )，clock( )等, 返回的是 float 类型。
  
```python
import time
print(time.time()) #返回当前时间戳
```


## 2 格式化的时间字符串 (Format String) 

```python
import time
print(time.strftime("%Y-%m-%d %H:%M:%S %p")) # 2020-12-18 10:25:16 AM
print(time.strftime("%Y-%m-%d %X")) # 2020-12-18 10:25:16 AM
```

## 3 结构化的时间 (struct_time) 元组  

struct_time 元组共有九个元素  

返回 struct_time 的函数主要有 : localtime( ), gmtime( ), strptime( )  。
元組 (struct_time) 九个元素的含义 :             

| 索引（Index） | 属性（Attribute）         | 值（Values）       |
| ---slug: 20230312220449
------slug: 20230312220449
---- | ---slug: 20230312220449
------slug: 20230312220449
------slug: 20230312220449
------slug: 20230312220449
---- | ---slug: 20230312220449
------slug: 20230312220449
------slug: 20230312220449
--- |
| 0             | tm_year（年）             | 比如2011           |
| 1             | tm_mon（月）              | 1 - 12             |
| 2             | tm_mday（日）             | 1 - 31             |
| 3             | tm_hour（时）             | 0 - 23             |
| 4             | tm_min（分）              | 0 - 59             |
| 5             | tm_sec（秒）              | 0 - 59             |
| 6             | tm_wday（weekday）        | 0 - 6（0表示周日） |
| 7             | tm_yday（一年中的第几天） | 1 - 366            |
| 8             | tm_isdst（是否是夏令时）  | 默认为-1                   |

 
```python
import time
print(time.localtime()) # 本地时区元组(struct_time)
'''输出
time.struct_time(tm_year=2020, tm_mon=12, tm_mday=18, tm_hour=8, tm_min=45, tm_sec=9, tm_wday=4, tm_yday=353, tm_isdst=0)\
'''

#时间元组的取值，一是索引，二是属性
print(time.localtime()[3]) # 8
print(time.localtime().tm_hour) #8
#strptime
time.strptime('2022-1-2','%Y-%m-%d')
'''输出
time.struct_time(tm_year=2022, tm_mon=1, tm_mday=2, tm_hour=0, tm_min=0, tm_sec=0, tm_wday=6, tm_yday=2, tm_isdst=-1)
'''
```

  

## 4 时间格式转换关系

![image.png](https://s1.vika.cn/space/2023/03/12/7951c28f287743db9f0e65c5739d5f96)

格式化的字符串时间与时间戳之间的转换都是以结构化的时间作为中转站来进行操作的。  

>结构化时间与时间戳之间的转化：  
>>time.mktime([结构化时间]) : "struct_time" 转换 "timestamp"  
>>time.localtime([时间戳]) : "timestamp" 转换 "struct_time" 本地时区  
>>time.gmtime([时间戳]) : "timestamp" 转换 "struct_time" UTC时区  
>>time.gmtime([time.time()])  

```python

import time
⛅"struct_time" 转换 "timestamp"
print(time.mktime(time.localtime()))   # 1608259357.0
print(time.mktime(time.gmtime()))      # 1608259357.0 

⛅"timestamp" 转换 "struct_time"
print(time.localtime(456465.4685))  # 返回的是"struct_time"本地时区元组
print(time.localtime(time.time()))  # 里面不填时间戳默认就是当前时间戳"time.time()"
print(time.gmtime(456465.4685))     # 返回的是"struct_time"UTC时区元组
print(time.gmtime(time.time()))
```


>结构化时间与格式化字符串时间之间的转换
>>time.strftime([时间格式],[结构化时间]) : "struct_time" 转换 "format time"
>>time.strptime([格式化的字符串时间],[时间格式]) : "format time" 转换 "struct_time"
