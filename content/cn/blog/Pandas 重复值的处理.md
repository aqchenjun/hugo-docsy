---
categories: Pandas
title: Pandas 重复值的处理
tags: [ 教程, python]
date: 2023-03-17
lastmod: 2023-03-23 
thumbnail: https://s1.vika.cn/space/2023/03/25/3aacbe6a765b458dbc352a0edde2c5be?attname=pattern-3109720__340.jpg
published: "true"
slug: 5w5k38o
---


## 1 duplicated() 函数  

找出重复值。  

```python
df.duplicated(
    subset = None,
    keep = 'first'
```  

subset：列名或其列表 
keep：处理重复值的方法。 
	- 'first'：第一个重复值以后的重复值，标记为 True。
	- 'last'：除去最后一个重复值，其余均标记为 True。
	- 'False'：所有重复值均标记为 True。 
返回的是一个 Boolean series。  

## 2 drop_duplicates() 函数  

去除重复值。  

```python
df.drop_duplicates(
    subset = None,
    keep = 'first',
    inplace = False,
    ignore_index = False,
```  

subset：列名或其列表
keep：处理重复值的方法。
	- 'first'：保留第一个重复值，以后的均去除。
	- 'last'：保留最后一个重复值，其余均去除。
	- 'False'：去除所有重复值。
返回一个没有重复值的 DataFrame
Frame
