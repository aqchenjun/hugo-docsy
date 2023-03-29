---
categories: Pandas
title: Pandas 几个常用方法
tags: [ 教程, python]
date: 2023-03-17
lastmod: 2023-03-20
thumbnail: https://s1.vika.cn/space/2023/03/25/14eaeca109544b5081324a1bf11f9534?attname=autumn-3937289_960_720.jpg 
published: "true"
slug: 20230317193947
---


## 1 Series.unique()  

返回 Series 中不重复的值  

## 2 DataFrame.drop_duplicates  

去除重复值  

```python
DataFrame.drop_duplicates(subset=None,
                          keep='first',
                          inplace=False,
                          ignore_index=False)
```  

## 3 DataFrame.nunique()  

统计数量  

```python
DataFrame.nunique(axis=0, dropna=True)
```  

## 4 DataFrame.count() 

统计非空数据的数量  

```python
DataFrame.count(axis=0, level=None, numeric_only=False)
```  

## 5 DataFrame.value_counts()  

统计各数据值出现的次数  

```python
DataFrame.value_counts(subset=None,
                       normalize=False,    # 显示次数，如果为 True，显示比例
                       sort=True,
                       ascending=False,
                       dropna=True)
```  

## 6 DataFrame.size  

返回元素的数量  

```python
DataFrame.size
```  

## 7 reindex()  

重新索引并得到一个新的DataFrame对象。reindex()方法不仅可以重新索引DataFrame，也可以同时实现排序、过滤功能。  

常用参数：  

`DataFrame.reindex(index=None, columns=None, method=None, fill_value=nan)`

index：用作索引的新序列，如果与原索引完全重合，即是排序，如果是原索引的子集，即是过滤筛选，如果出现新的索引，则其列的值根据 method 和 fill_value 的设置来确定。
columns：用作列名的新序列，与 index 类似。 
method：插值填充方式 
	- ffill 或 pad ：前向填充值（用前一个索引的数据作为自己的数据）
	- bfill 或 backfill：后向填充值（用后一哥索引的数据作为自己的数据）
	- nearset：从最近的索引值填充
fill_value：引入缺失值时使用的替代值（所有的缺失值都会用这个值）  

## 8 set_index()  

主要是用DataFrame中的某一列的值设置index，也可以设置全新的index。set_index() 一个重要参数 append，如果为True，表示保留原来的索引，直接把新设定的添加到原索引的内层，如果为默认值False，则不保留原来的索引。
当设置全新的 index 时，要采用 pd.Index() 方法。  

`df.set_index(pd.Index(range(1,4)))`