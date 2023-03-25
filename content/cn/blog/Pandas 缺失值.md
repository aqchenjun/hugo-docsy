---
source: Pandas
title: Pandas 缺失值
tags: [ 教程, python]
date: 2023-03-14
lastmod: 2023-03-21 
thumbnail: https://s1.vika.cn/space/2023/03/25/2cde404f0152484fa6d28bfc6bfaeab0?attname=QJ6272362943.jpg
published: "true"
---


## 1 筛选过滤  

```python
Series.isna()
DataFrame.isna()
```  

## 2 填充  

```python
DataFrame.fillna(value=None,
                 method=None,
                 axis=None,
                 limit=None,
                 downcast=None)
```  

参数 value 的取值范围： 

	1. 单个值。用该值填充所有的缺失值。
	2. 字典。字典的 key 表示 column，字典的 value 用来填充该列的缺失值。
	3. Series。对应 index 进行填充
	4. DataFrame。对应 column 进行填充。  

参数 method：
>{‘backfill’, ‘bfill’, ‘pad’, ‘ffill’, None}, default None  

## 3 删除  

```python
DataFrame.dropna(axis=0,    #删除方向，0 表示删除有缺失值的行，1 表示删除有缺失值的列
                how='any',     #删除方式，‘any’表示存在任意一个缺失值即删除，‘all’表示该行/列所有值都缺失时才删除,

                thresh=None,    

                subset=None,)

Series.dropna(axis=0, inplace=False, how=None)
```  

参数 thresh 表示非空值的最小数量。


	1. 当删除方向为行时，保留非空值数量不小于 thresh的行。
	3. 当删除方向为列时，保留非空值数量不小于 thresh 的列。  

### 3.1 数据集  

```python
df = pd.DataFrame([[2,5, 7, 4,np.nan],
                  [3, np.nan, np.nan,np.nan, 1],
                 [2,1, np.nan, np.nan, 5],
                [4,2, np.nan, np.nan, 4]],                 columns=list('ABCDE'))  
```  

df:

<img src="https://s1.vika.cn/space/2023/03/14/d35251b19dba48e599d506b863278222" width=200px>  

### 3.2 删除方向为行

代码：  

`df.dropna(thresh=2,axis=0)`  

结果：

<img src="https://s1.vika.cn/space/2023/03/14/7601cd2bc67f40aa98da95901759a6da" width=200px>

代码：  

`df.dropna(thresh=3,axis=0)`  

结果：

<img src="https://s1.vika.cn/space/2023/03/14/acfb3cd8bf23403a9336f526905da31f" width=200px>

代码：  

`df.dropna(thresh=4,axis=0)`  

结果：

<img src="https://s1.vika.cn/space/2023/03/14/59df39693c57433fb34d20564fb9a7ec" width=200px>

### 3.3 删除方向为列  

代码：  

`df.dropna(thresh=1,axis=1)`  

结果：

<img src="https://s1.vika.cn/space/2023/03/14/625ef71dfa7c460db17a6cd8c286dffb" width=200px>

代码：  

```python
df.dropna(thresh=2,axis=1)
df.dropna(thresh=3,axis=1)
```  

结果：

<img src="https://s1.vika.cn/space/2023/03/14/ee9c1ec2d623447e8da4450e907a39ad" width=200px>

代码：  

`df.dropna(thresh=4,axis=1)`  

结果：

<img src="https://s1.vika.cn/space/2023/03/14/94d2fa9ee64c4685b43f7664ac946bce" width=80px>
