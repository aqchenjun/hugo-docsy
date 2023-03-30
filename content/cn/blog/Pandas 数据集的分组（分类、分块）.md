---
categories: Pandas
title: Pandas 数据集的分组（分类、分块）
tags: [ 教程, python]
date: 2023-03-18
lastmod: 2023-03-22 
thumbnail: https://s1.vika.cn/space/2023/03/25/b9c5dfff93044b72ada1cc731189ce19?attname=magnets-2488264_960_720.jpg
published: "true"
slug: q359088
---

pandas 的分组（分类、分块）涉及到的方法有 groupby(by=None)、value_counts(bins=None)、cut(bins=)。  

## 1 groupby 方法  

```python
DataFrame.groupby(by=None,
                  axis=0,
                  level=None,
                  as_index=True,
                  sort=True,
                  group_keys=True,                  squeeze=NoDefault.no_default,
                  observed=False,
                  dropna=True)
```  

参数 ：
by的取值：
- 函数，调用的参数是 index 。
- labels或者其表达式。labels表达式功能相当于 python 中的 key 参数。如 by = employees['Name'].str.len()，表示以 Name 字段的字符长度为分组依据。
- 与分组轴向长度一致的列表。相当于增加了一列，然后对该列分组。
- Grouper 对象。  

## 2 value_counts方法  

```python
Series.value_counts(normalize=False,    #显示各分项占总数的比例
                    sort=True,
                    ascending=False,
                    bins=None, 
                    dropna=True)
                    
DataFrame.value_counts(subset=None,
                        normalize=False,
                        sort=True,
                        ascending=False,
                        bins=None, 
                        dropna=True)
```  

参数 bins 只对数组型有效，可以是列表，也可以是一个 int。  

1. 列表。表示指定区间。如 bins=[0, 200, 400, 600, 800, 1000, 1200, 1400]。系统会将 Series 按7个区间进行统计，这 7 个区间是[0,200]、(200,400]、(400,600]、(600,800]、(800,1000]、(1000,1200]、(1200,1400]。小括号表示不包括，中括号表示包括。
2. int 。该 int表示将 Series 分割成的区间。具体区间范围由系统根据 Series 数据的最小值和最大值以及该 int 值自动计算得到。  

## 3 cut 方法  

```python
pd.cut(x,
       bins,
       right=True,
       labels=None,
       retbins=False,
       precision=3,
       include_lowest=False,
       duplicates='raise',
       ordered=True)
```  

## 4 根据分位数结果进行分组  

对一列数据，分成三组，前10%、后10%及其他。每组进行统计。  

```python
se = pd.Series(data=np.random.randint(1,10,5))
res = se.quantile([0.1,0.9])

#结果是一个Series，索引是 [0.1,0.9] ，res[0.1] 即第前10%的值，res[0.9] 即 第前90%的值。
def q():
  se1 = se.transform(lambda x:'first' if x<quantile_list[0.1] else ('last' if x>quantile_list[0.9] else 'mid'))

#此处的 transform 可以 用 map
return se1
  se.groupby(q())['se'].mean()
```()
```