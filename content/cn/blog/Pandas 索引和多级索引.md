---
categories: Pandas
title: Pandas 索引和多级索引
tags: [ 教程, python]
date: 2023-03-17 
lastmod: 2023-03-21 
thumbnail: https://s1.vika.cn/space/2023/03/25/216d459520914627b21fd672e906871e?attname=archive-7548483__340.jpg
published: "true"
slug: 8kdp5el
---


## 1 索引

### 1.1 `[ ]`

`DataFrame[ column ]`

`Series [ index ]`

### 1.2 df.loc`[行,列]`

如果包括全部列，则只需要行参数，‘，’及后面的列参数均可省略。如果包括全部行，则需要用‘:’表示行。有效的输入参数包括：

1.   a single label，如 ‘a’。
2.  a list of labels，如 ['a', 'b', 'c']，注意要加上 方括号
3.  A slice object with labels，如 'a':'f'。注意没有方括号
4.  A boolean array
5. 函数，函数的参数是 df 本身，返回值是上述4项之一。

函数实例：

```python
df = pd.DataFrame(np.random.randn(6, 4),
                  index=list('abcdef'),
                  columns=list('ABCD'))
def fu(x):
    return x['A']>x['B']    #返回一个boolean array
df.loc[fu]
#相当于
df[df['A']>df['B']]
```

### 1.3 df.iloc`[行号,列号]`

### 1.4 标签索引和位置索引的组合

```python
df.loc[df.index[[1,2]],'A']
df.iloc[[1,2],df.columns.to_list().index('A')]
```

### 1.5 at 和iat

标签索引：`Series.at()、DataFrame.at()`

位置索引：`Series.iat()、DataFrame.iat()`

at 和 iat 方法定位单个元素，比使用  `[ ]` 方法要快。

### 1.6 isin

```python
Series.isin(value)
DataFrame.index.isin(values, level=None)     #level 表示多级索引的级数
```

### 1.7 索引的设置和重置

```python
df.set_index(keys,
             drop = True,
             append = False,)
#下面的 reset_index 方法是 set_index 方法的逆方法，即从行到列，与droplevel 方法不同
df.reset_index(level = None,    #要重置的索引
                drop = False,   #新索引在 column 中的位置为默认值 
                col_level = 0,    #新索引的级数
                col_fill='')      #多级索引中，空白级的填充字符
DataFrame.reindex(index=None, 
                  columns=None, 
                  method=None, 
                  fill_value=nan)
```

### 1.8 索引排序和重命名

```python
DataFrame.sort_index(axis=0, 
                     level=None, 
                     ascending=True,
                     na_position='last', 
                     sort_remaining=True, 
                     ignore_index=False, 
                     key=None)
DataFrame.rename_axis(index=None, 
                      columns=None,)
DataFrame.rename(index=None, 
                 columns=None,                  
                 level=None, 
                 errors='ignore')
```

## 2 多级索引

### 2.1 构造

```python
pd.MultiIndex.from_arrays(arrays, 
                          sortorder=None, 
                          names=NoDefault.no_default)
pd.MultiIndex.from_tuples(tuples, 
                          sortorder=None, 
                          names=None)
pd.MultiIndex.from_product(iterables, 
                           sortorder=None, 
                           names=NoDefault.no_default)
pd.MultiIndex.from_frame(df, 
                         sortorder=None, 
                         names=None)
```

### 2.2 删除

`DataFrame.droplevel(level, axis=0)`

### 2.3 交换

```python
DataFrame.reorder_levels(order,    #新层级顺序的列表，可以是 int，也可以是 label 
                        axis=0)
DataFrame.swaplevel(i=,    #欲交换的索引层级，int or str
                    j=,    #欲交换的索引层级，int or str
                    axis=0)
#该方法只能两两交换
```

### 2.4 定位

### 2.5 `df.loc[行,列]`和 `df.iloc[行,列]`

由于多级索引中的单个元素以元组为单位，因此单级索引的 loc 和 iloc 方法完全可以照搬，只需把标量的位置替换成对应的元组。

`df.loc[[(),()],列]`   
参数是元祖列表，每个元祖表示一个多级索引

`df.loc[([],[]),列]`  
参数是列表元祖，列表顺序对应索引的层级，列表元素可以是label，也可以是函数。如果包括该层级的全部索引，则表示为 slice(None)

实例：

```python
L1,L2 = ['A','B','C'],['a','b','c']
mul_index1 = pd.MultiIndex.from_product([L1,L2],names=('Upper', 'Lower'))
L3,L4 = ['D','E','F'],['d','e','f']
mul_index2 = pd.MultiIndex.from_product([L3,L4],names=('Big', 'Small'))
df= pd.DataFrame(np.random.randint(-9,10,(9,9)),
                    index=mul_index1,
                    columns=mul_index2)
df
```

结果：
![image.png|300](https://s1.vika.cn/space/2023/03/17/a793a33d88ec413a81975067b07d386d)

```python
#选取行('A','b')和('B','a')，选取所有第二层列的 'd'
df.loc[[('A','b'),('B','a')],(slice(None),['d'])]
#选取 ('D','e') 列大于 0，第二层索引是 'a','c'的所有行，列名是('D','f')
df.loc[(df[('D','e')]>0,['a','c']),[('D','f')]]
```

### 2.6 xs 方法

```python
DataFrame.xs(key,    #label or tuple of label 
             axis=0, 
             level=None, #与key对应，取值为 int、label or list 
             drop_level=True)
```

### 2.7 IndexSlice对象

```python
idx = pd.IndexSlice
df.loc[idx[level_0,level_1,],] #level_0 表示 level_0 索引的label、list及其切片
```

### 2.8 时间索引的定位

当索引为 DatetimeIndex 时，除了常规的 index 定位方法外，有些特殊用法。

>使用部分索引字符串：

```python
df=pd.DataFrame(
    np.random.randn(10, 1),
    columns=["A"],
    index=pd.date_range("20130801", periods=10, freq="M")    
)
df
```

数据集：

![image.png|200](https://s1.vika.cn/space/2023/03/17/e09ec8d9d82f4cfe99b095d2f3a5b2bd)

```python
df.loc['2014']
df.loc['2014-2']
#部分字符串必须从头开始，如果需要中间某个日期，则用下面方法
```

>任意日期时间的定位：

`df[df.index.day==31]`

>包含 DatetimeIndex 的多级索引的定位：

```python
df=pd.DataFrame(
    np.random.randn(10, 1),
    columns=["A"],    index=pd.MultiIndex.from_product([["a", "b"],pd.date_range("20131001", periods=5, freq="M") ])
)
df
```

![image.png|200](https://s1.vika.cn/space/2023/03/17/491889a682b344e2b38258df12ed9003)

```python
df.loc[(slice(None),'2013'),:]        #所有2013年的数据
#提取所有1月份的数据。思路：由于 df.index 是元组列表，可以对 df.index 进行 map 映射，找出每个日期数据的月份
m = df.index.map(lambda x:x[1].month)
df[m==1]
```

## 3 多级索引和单级索引的互相转换

### 3.1 数据

```python
arrays = [
    ["bar", "bar", "baz", "baz", "foo", "foo", "qux", "qux"],
    ["one", "two", "one", "two", "one", "two", "one", "two"],
]
tuples = list(zip(*arrays))
columns = pd.MultiIndex.from_tuples(tuples, names=["first", "second"])
index = pd.MultiIndex.from_tuples([("A",'a'),("A",'d'), ("B",'c'),('B','d')],names=['upper','lower'])
df = pd.DataFrame(np.random.randn(4, 8), index=index, columns=columns)
df
```

结果：

![image.png](https://s1.vika.cn/space/2023/03/17/c2049ab053f74bf5992cd840eeb35fc4)

### 3.2 多级索引 → 单级

以前缀或者后缀的形式将多级索引连接成单级索引。

```python
#df.columns=[i[0]+'_'+i[1] for i in df.columns]
df.columns=df.columns.map(lambda x:x[0]+'_'+x[1])
```

结果：

![image.png](https://s1.vika.cn/space/2023/03/17/595c4b01f19c4efe818aca14a676737f)

### 3.3 单级索引 → 多级

转换的前提是单级索引的名称中，有多级索引的分割符。以上述转换的结果为例。

```python
df.columns=df.columns.str.split('_').map(lambda x:(x[0],x[1]))
```

df.columns 列表元素是字符串，字符串分割后为列表，map 方法将列表转换为元组，即多级索引。�引。