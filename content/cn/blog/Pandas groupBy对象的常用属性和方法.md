---
categories: Pandas
title: Pandas groupBy对象的常用属性和方法
tags: [ 教程, python]
date: 2023-03-13
lastmod: 2023-03-24 
thumbnail: https://s1.vika.cn/space/2023/03/25/79df5448cf434bb7b7d9461a802f7436?attname=flowers-6790357__340.jpg
published: "true"
slug: biegtlu
---


## 1  数据集  

```python
food_data = {
 "Item": ["Banana", "Cucumber", "Orange", "Tomato", "Watermelon"],
 "Type": ["Fruit", "Vegetable", "Fruit", "Vegetable", "Fruit"],
 "Price": [0.99, 1.25, 0.25, 0.33, 3.00]
 }
supermarket = pd.DataFrame(data = food_data)
supermarket
```

结果：
  
![image.png](https://s1.vika.cn/space/2023/03/13/aca1d1bd53684db6a8769a9de4c7a008)
  
```python
grouper = supermarket.groupby('Type')
```

## 2  获取组名  

```python
grouper.groups
```
  
结果：
  
![image.png](https://s1.vika.cn/space/2023/03/13/bbc9344f897e441e8e7ebdc933c2b976)

上述结果是一个字典，字典的 key 是组名，value 是索引列表。  

```python
#组名列表
list(grouper.groups.keys())
#组内容遍历
for k,v in grouper:
    print(k)
    print(v)
#组名遍历
for k in grouper.groups:
    print(k)
#值遍历
for v in grouper.groups.values():
    print(v)
#字典遍历
for k,v in grouper:
    print(k,v)
```

## 3  单独取出组  

```python
grouper.get_group('Vegetable')
```

## 4 first、last、head、tail  

```python
grouper.first()    #每组的第一个元素
grouper.last()
grouper.head(n=5)    #每组的头 n 个元素
grouper.tail(n=5)
```

## 5 apply  

```python
grouper.apply(func, *args, **kwargs)
```

参数：
- func：函数，参数是分组后的 DataFrame。
- args, kwargs：tuple and dict，传递给 func 的其他参数。  

```python
grouper.apply(lambda x: x / x.sum())        #分组后，每个数据与该组和的比值
grouper['item'].apply(lambda x:x.str.contains('e').sum())    #lambda x:x.str.contains('e') 表示当item 中包含 e 时，返回True，对齐求和，可以得到True的数量。该语句表示分组后，item 中包含 e 的数据的个数
```

## 6 count 和 size  

```python
grouper.count()    #组内各统计元素的数量,不包括 NaN
grouper.size()     #每组元素的数量，包括 NaN
```

  

## 7  组的数量(长度)  

```python
len(grouper)
```

## 8  分组后的排序  

```python
grouped = DataFrame.groupby(by=None)
grouped.mean().nlargest(n, columns, keep='first')
grouped.mean().nsmallest(n,columns,keep='first')

#利用分组前的排序结果
grouped.first()
grouped.last()
grouped.head(n=5)
grouped.tail(n=5)

#分组前排序
grouped = DataFrame.sort_values().groupby(by=None)
```

## 9  索引和列的混合分组  

使用 Grouper 对象。  

```python
df.set_index('B').groupby([len,pd.Grouper(key='A')])
df.set_index('B').groupby([pd.Grouper(level=0),pd.Grouper(key='A')])
```

>上述语句，首先将列 'B' 设置为索引。
>
第一句groupby 方法中的 by 参数列表中，第一个元素是函数，索引的字符串长度；第二个元素是Grouper对象。
>
第二句 groupby 方法中的 by 参数列表元素均为 Grouper 对象。

## 10  聚合  

### 10.1 直接定义在groupby对象的聚合函数  

| Function                                                                        | Description                                |     |     |
| ---slug: 20230313223812
------slug: 20230313223812
------slug: 20230313223812
------slug: 20230313223812
------slug: 20230313223812
------slug: 20230313223812
------slug: 20230313223812
------slug: 20230313223812
------slug: 20230313223812
------slug: 20230313223812
------slug: 20230313223812
------slug: 20230313223812
------slug: 20230313223812
---- | ---slug: 20230313223812
------slug: 20230313223812
------slug: 20230313223812
------slug: 20230313223812
------slug: 20230313223812
------slug: 20230313223812
------slug: 20230313223812
--- | --- | slug: 20230313223812
--- |
| mean()、median()、sum()、any、all、idxmax、idxmin、mad、nunique、skew、quantile |                                            |     |     |
| size()                                                                          | Compute group sizes                        |     |     |
| count()                                                                         | Compute count of group                     |     |     |
| std()、var()、sem()                                                             |                                            |     |     |
| describe()                                                                      | Generates descriptive statistics           |     |     |
| first()、last()                                                                 | Compute first or last of group values      |     |     |
| nth()                                                                           | Take nth value, or a subset if n is a list |     |     |
| head(n=5)、tail(n=5)                                                            | Return first or last n rows of each group  |     |     |
| min()、max()                                                                    | Compute min or max of group values         |     |     |                                                                               |                                            |     |     |

  

### 10.2 agg函数  

```python
DataFrameGroupBy.agg(func=None, *args, **kwargs)
```

## 11  分组后的筛选过滤  

### 11.1  数据集  

```python
DataFrameGroupBy.filter(func,          #参数是分组后的 subframe
                        dropna=True)    #不满足条件的数据不显示，如果 False，不满足条件的数据显示 NaN。
#示例数据
df = pd.DataFrame(
    {
	"A": ["foo", "bar", "foo", "bar", "foo", "bar", "foo", "foo"],
	"B": ["one", "one", "two", "three", "two", "two", "one", "three"],
	"C": np.random.randn(8),
	"D": np.random.randn(8),
    }
)
df
```

>结果：

|     | A   | B     | C         | D        |
| --- | slug: 20230313223812
--- | ----- | slug: 20230313223812
------slug: 20230313223812
--- | ---slug: 20230313223812
----- |
| 0   | foo | one   | 0.329390  | 1.590274 |
| 1   | bar | one   | -0.643742 | 0.174269 |
| 2   | foo | two   | -0.164038 | 0.622962 |
| 3   | bar | three | -0.235379 | 0.719191 |    |     |       |           |          |
| 4 | foo | two | 0.244213 | -0.478606 |
| 5 | bar | two | 0.251741 | 0.398766 |
| 6 | foo | one | 1.546374 | 0.294665 |
| 7 | foo | three | 0.025296 | -1.474683 |

### 11.2  符合条件的组保留显示  

```python
df.groupby('A').filter(lambda x:x['C'].sum()>1)
```
  
DataFrame.filter 返回subframe，结果只保留了x['C']分组和大于 1 的df['A'] 中的 'foo' 分组。  

|     | A   | B   | C   | D   |
| --- | slug: 20230313223812
--- | --- | slug: 20230313223812
--- | --- |
| 0 | foo | one | 0.329390 | 1.590274 |
| 2 | foo | two | -0.164038 | 0.622962 |
| 4 | foo | two | 0.244213 | -0.478606 |
| 6 | foo | one | 1.546374 | 0.294665 |
| 7 | foo | three | 0.025296 | -1.474683 |

```python
df.groupby('A')['C'].filter(lambda x:x.sum()>1)
#结果
0    0.329390
2   -0.164038
4    0.244213
6    1.546374
7    0.025296
Name: C, dtype: float64
```

Series.filter 返回 Series，结果只保留符合条件的分组，显示其 index 和 df['C'] 值。  

## 12  转换 transform  

```python
#不分组
DataFrame.transform(func,
                    axis=0,
                    *args,
                    **kwargs)
#分组
DataFrameGroupBy.transform(func,
                           *args,
                           **kwargs)
```

参数：
>func 的取值范围：
>- 函数
>- 函数名
>- 上述两种的列表，如 [np.exp, 'sqrt']
>- 字典  

>返回值：
>新的 DataFrame 或 Series  

不分组示例：  
```python
df = pd.DataFrame([[1, 2, 3],
                    [4, 5, 6],
                    [7, 8, 9],
                    [np.nan, np.nan, np.nan]],
                   columns=['A', 'B', 'C'])
df
```

数据：  

|     | A   | B   | C   |
| slug: 20230313223812
--- | --- | slug: 20230313223812
--- | --- |
| 0 | 1.0 | 2.0 | 3.0 |
| 1 | 4.0 | 5.0 | 6.0 |
| 2 | 7.0 | 8.0 | 9.0 |
| 3 | NaN | NaN | NaN |

```python
df.transform(lambda x:x+1)
df.transform([np.exp, 'sqrt'])
df[['A','B']].transform('sqrt')
df.transform({'A':'sqrt','B':np.exp})
```

分组情况下的 transform ，多了聚合函数的使用，导致可能出现单一计算结果，此时会广播到相同shape。其他与不分组下的 transform 用法基本一致。  

## 13  pipe 方法  

```python
DataFrame.pipe(func, *args, **kwargs)
```

## 14  分组后的跨列分组计算  

```python
food_data = {
 "Item": ["Banana", "Cucumber", "Orange", "Tomato", "Watermelon"],
 "Type": ["Fruit", "Vegetable", "Fruit", "Vegetable", "Fruit"],
 "Price": [0.99, 1.25, 0.25, 0.33, 3.00],
 "Quantity":[12,21,23,10,9]
 }
supermarket = pd.DataFrame(data = food_data)
supermarket
```

数据集：  

|     | Item | Type | Price | Quantity |
| slug: 20230313223812
--- | ---- | slug: 20230313223812
---- | ----- | slug: 20230313223812
-------- |
| 0 | Banana | Fruit | 0.99 | 12 |
| 1 | Cucumber | Vegetable | 1.25 | 21 |
| 2 | Orange | Fruit | 0.25 | 23 |
| 3 | Tomato | Vegetable | 0.33 | 10 |
| 4 | Watermelon | Fruit | 3.00 | 9 |

按 Type 列分组后，计算每组的商品价值总和，即 price * quantity 的和。  

```python
grouper = supermarket.groupby('Type')
grouper.apply(lambda x:(x['Price']*x['Quantity']).sum())
```

>注意
>- 此处不能使用 pipe 函数，因为 pipe 函数中的 x['Price'] 和 x['Quantity'] 是 SeriesGroupby 对象，聚合后才能使用。�用。