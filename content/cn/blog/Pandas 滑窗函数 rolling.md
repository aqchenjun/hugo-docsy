---
source: Pandas
title: Pandas 滑窗函数 rolling
tags: [ 教程, python]
date: 2023-03-17
lastmod: 2023-03-21
thumbnail: https://s1.vika.cn/space/2023/03/25/5d40f115ac02485db5c50e59aca0e40d?attname=fall-7863868_960_720.webp
published: "true"
slug: 20230317175253
---


## 1 滑窗函数 rolling 常见参数：

```python
DataFrame.rolling(window, min_periods=None,center=False, on=None, axis=0)
```

## 2 window：

表示时间窗的大小，注意有两种形式（int or offset）。如果使用int，则数值表示计算统计量的观测值的数量即向前几个数据。如果是offset类型，表示时间窗的大小，如'30D'，表示30天。

## 3 min_periods：

最少需要有值的观测点的数量，对于int类型，默认与window相等。对于offset类型，默认为1。

## 4 center：

是否使用window的中间值作为label，默认为false。只能在window是int时使用。

## 5 on：

对于DataFrame如果不使用index（索引）作为rolling的列，那么用on来指定使用哪列。

## 6 axis：

方向（轴），一般都是0。

```python
s= pd.Series([100, 90, 110, 150, 110, 130, 80, 90, 100, 150])
s.rolling(3)
```

结果示意如下：能够使用相应的聚合函数进行计算，还支持使用 apply 传入自定义函数，其传入值是对应窗口的 Series。

![image.png](https://s1.vika.cn/space/2023/03/17/f841caee0afd4a10bfe18be5a80b4ee4)


```python
s.rolling(3).mean()
#下面语句等效
s.rolling(3).apply(lambda x:x.mean())
```

rolling函数是向下滑行，如果需要向上滑行，则需要将序列倒序。

shift, diff, pct_change 是一组类滑窗函数，它们的公共参数为periods=n ，默认为1，分别表示取向前第n 个元素的值、与向前第 n 个元素做差（与 Numpy 中不同，后者表示 n 阶差分）、与向前第 n 个元素相比计算增长率。这里的 n 可以为负，表示反方向的类似操作。
