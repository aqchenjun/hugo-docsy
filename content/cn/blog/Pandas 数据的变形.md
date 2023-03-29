---
categories: Pandas
title: Pandas 数据的变形
tags: [ 教程, python]
date: 2023-03-27
lastmod: 2023-03-28 
slug: 20230327222108
thumbnail: 
published: "true"
---


pandas 数据的变形，实际上是行、列数据的转换，既有简单的行、列交换，也有复杂的数据聚合。

## 1 transpose 方法，简单的行、列交换

```python
DataFrame.transpose(*args, copy=False)
DataFrame.T
```

## 2 stack 和 unstack 方法

```python
DataFrame.stack(level=- 1, dropna=True)
DataFrame.unstack(level=- 1, fill_value=None)
```

## 3 长表变宽表

### 3.1 pivot 方法

`DataFrame.pivot(index=None, columns=None, values=None)`

unstack 方法的结果也是长表变宽表。

### 3.2 pivot_table 方法

pivot_table 和 pivot 方法的区别在于，pivot_table 方法可以处理 index 有重复的数值型 columns。

```python
DataFrame.pivot_table(values=None, 
                      index=None, 
                      columns=None, 
                      aggfunc='mean', 
                      fill_value=None, 
                      margins=False, 
                      dropna=True,                      
                      margins_name='All', 
                      observed=False, 
                      sort=True)
```

参数：

- index，columns：取值范围包括列名标签、Grouper对象、 array、及他们的列表
- aggfunc：函数、函数列表、字典。

index 和 columns 取值 Grouper 对象时，常用于时间日期的分组，示例：

```python
df = pd.DataFrame(
   {
       "Publish date": [
            pd.Timestamp("2000-01-02"),
            pd.Timestamp("2000-01-02"),
            pd.Timestamp("2000-01-09"),
            pd.Timestamp("2000-01-16")
        ],
        "ID": [0, 1, 2, 3],
        "Price": [10, 20, 30, 40]
    }
)
df
```

结果：

![image.png|200](https://s1.vika.cn/space/2023/03/28/a0448529d1744e038e2dcdd44760653c)

```python
df.pivot_table(index=(pd.Grouper(key='Publish date',freq='1W')),columns='ID',values='Price')
```

结果：

![image.png|300](https://s1.vika.cn/space/2023/03/28/b757e704dc5b4da4a01d13a4e59c421c)

### 3.3 groupby 方法

```python
DataFrame.groupby(by=None, 
                  axis=0, 
                  level=None, 
                  as_index=True, 
                  sort=True, 
                  group_keys=True, 
                  dropna=True)
df.groupby(by=[pd.Grouper(key='Publish date',freq='1W'),'ID'])['Price'].mean().unstack()
```

groupby 方法比上面的 pivot_table 方法速度快，两者比较

### 3.4 get_dummies 方法

```python
pandas.get_dummies(data, 
                   prefix=None, 
                   prefix_sep='_', 
                   dummy_na=False, 
                   columns=None, 
                   sparse=False, 
                   drop_first=False, 
                   dtype=None)
```

## 4 宽表变长表

### 4.1 melt 方法

```python
DataFrame.melt(id_vars=None, 
               value_vars=None, 
               var_name=None, 
               value_name='value', 
               col_level=None, 
               ignore_index=True)
```

### 4.2 wide_to_long 方法

```python
pd.wide_to_long(df, 
                stubnames,       #要变成长表格式的宽表变量的前端部分
                i,               #作为数据标识的列名
                j,               #要变成长表格式的宽表变量的后端部分在新DataFrame中列的名称
                sep='',          #要变成长表格式的宽表变量的前、后端部分分隔符
                suffix='\\d+')   #要变成长表格式的宽表变量的后端部分的正则表达式
```

### 4.3 多级索引列的宽表变长表

#### 4.3.1 melt 方法

第一步，重置索引

由于列是多级索引，必然存在索引行，melt 方法中的 id_vars参数不能为索引，因此要重置

第二步，melt

melt 方法中，仅设置 id_vars 参数，不设置value_vars 参数，此时会得到一个DataFrame长表，一级索引是一列，二级索引是一列，数据是一列

第三步，pivot

将一级索引列或者二级索引列进行旋转。

```python
index_c=pd.MultiIndex.from_product([['Chinese','Math'],['Mid','Final']])
index = pd.MultiIndex.from_tuples([(1,'San Zhang'),(2,'Si Li')],name=['Class','Name'])
df=pd.DataFrame(data=[[80,90,80,90],[75,85,75,85]],columns=index_c,index=index)

df=df.reset_index()
df.melt(id_vars=['Class','Name']).pivot(index=['Class','Name','variable_1'],columns=['variable_0'],values=['value'])
```


#### 4.3.2 wide_to_long方法

由于 wide_to_long 方法需要参数 stubnames、i、sep、和 suffix，因此需要对多级索引进行处理，将索引层级修改为1级。修改方法：

`df.columns = [i[0]+'_'+i[1] for i in df.columns]`

df.columns.values 是一个列表，列表元素是元祖，(一级索引,二级索引)。

```python
df=df.reset_index()
pd.wide_to_long(df,
                stubnames=['Chinese','Math'],i=['Class','Name'],j='type',sep = '_',suffix= '\w+',)
```

## 5 其他

```python
DataFrame.explode(column, 
                  ignore_index=False)
```

该方法可以对指定的column进行纵向展开，参数 column 标识列名，数据形式包括 lists, tuples, sets, Series, and np.ndarray。

`df= pd.DataFrame({'A': [[1, 2],'my_str',{1, 2}],'B': 1})`

结果：
![Untitled 2.png](https://s1.vika.cn/space/2023/03/27/13e5035e1a3549bda1007ed82c72a0c3)


`df.explode('A')`

结果：

![Untitled 3.png](https://s1.vika.cn/space/2023/03/27/8f4db31934f64f71892163ee4dc4e6ae)

## 6 实例应用

### 6.1 长表变宽表

现有一份关于美国非法药物的数据集，其中 SubstanceName, DrugReports 分别指药物名称和报告数量：

```python
df = pd.read_csv('drugs.csv')
df.head()
```

结果：

![image.png|500](https://s1.vika.cn/space/2023/03/28/6ce9c1b84c5e4b1a97edb3810b6a554a)


将数据转为如下的形式：

![image.png](https://s1.vika.cn/space/2023/03/28/4f19d625744d4d8d99b21252f5078a55)

#### 6.1.1 方法一：pivot

```python
df_wide = df.pivot(index=['State','COUNTY','SubstanceName'],columns=['YYYY'],values='DrugReports')
df_wide = df_wide.reset_index().rename_axis(columns={'YYYY':''})
```

#### 6.1.2 方法二：unstack

```python
df_wide = df.set_index(['YYYY','State','COUNTY','SubstanceName']).unstack(0)
df_wide = df_wide.droplevel(0,axis=1).reset_index().rename_axis(columns={'YYYY':''})
```

### 6.2 宽表变长表

将上面得到的 df_wide 转换为原表。

#### 6.2.1 方法一：melt

```python
df_long = df_wide.melt(id_vars=['State','COUNTY','SubstanceName'],value_vars=range(2010,2030),var_name='YYYY',value_name='DrugReports').dropna(subset='DrugReports')
df_long = df_long.reset_index().drop('index',axis=1)
```

#### 6.2.2 方法二：stack

```python
df_long = df_wide.set_index(['State','COUNTY','SubstanceName']).stack()
df_long = df_long.reset_index().rename(columns={'':'YYYY',0:'DrugReports'}).sort_values('YYYY')
```

#### 6.2.3 方法三：wide_to_long

需要转换的列名加上前缀，如原列名‘2010’，加上前缀后为“ DrugReports_2010”，实际上这就是列数据的含义。该前缀“DrugReports”就是新长表中的数据列的列名，由参数 “stubname” 设置。后缀2010、2011、2012等就是新长表中的年份列的数据，该年份列的列名由参数 “j” 设置。

```python
df_temp = df_wide.rename(columns={i:'DrugReports_'+str(i) for i in df_wide.columns[3:]})
pd.wide_to_long(df_temp,stubnames='DrugReports',i=['State','COUNTY','SubstanceName'],j='YYYY',sep= '_',suffix = '.+').dropna(subset='DrugReports').reset_index()
```

### 6.3 数据透视表

按 State 分别统计每年的报告数量总和，其中 State, YYYY 分别为列索引和行索引。

#### 6.3.1 方法一：pivot_table

`df.pivot_table(index='YYYY',columns='State',values='DrugReports',aggfunc='sum')`

#### 6.3.2 方法二：groupby+unstack

`df.groupby(['YYYY','State'])['DrugReports'].sum().unstack(1)`

### 6.4 用 pivot 方法实现 get_dummies 并用 melt 方法逆转换

#### 6.4.1 数据集

```python
df = pd.DataFrame({"key": list("bbacab"), "data1": range(6)})
df
```

结果：

![image.png|200](https://s1.vika.cn/space/2023/03/28/07e1177eddaa4b1c8057a2b5b601b32c)

#### 6.4.2 get_dummies 方法

```python
df1=pd.get_dummies(df,columns=['key'])
df1
```

结果：

![image.png|300](https://s1.vika.cn/space/2023/03/28/8a92404591804f27a5e156b52ae95779)

#### 6.4.3 pivot方法

get_dummies 方法实质上是长表变宽表，只是其数据只能为 1 或 0，这需要单独处理。

- 增加一列

`df['values']=np.ones(6).astype(int)`

- pivot 变换

`df2 = df.pivot(index='data1',columns='key',values='values')`

- 填充 NaN值等后续处理

`df2.fillna(0).astype(int).rename_axis(columns={'key':''}).reset_index()`

结果：

![image.png|200](https://s1.vika.cn/space/2023/03/28/f070bdfa330148289f14cbee898cb430)


#### 6.4.4 melt方法逆转换

- melt

`df3=df1.melt(id_vars='data1',value_vars=['key_a','key_b','key_c'])`
结果：

![image.png|300](https://s1.vika.cn/space/2023/03/28/db085b3cc58a4709987cc5be3de0c25b)

- 将 value=0的行过滤掉，再删除 value 列

`df4=df3[df3['value']!=0].drop(labels='value',axis=1)`

结果：

![image.png|200](https://s1.vika.cn/space/2023/03/28/80171fe9016f4d62a04c87b019a2ce53)

- 去除 ‘key_’字符串等后续处理

```python
df4.replace('key_','',regex=True).sort_values('data1').reset_index().drop('index',axis=1).rename(columns={'variable':'key'})
```

结果：

![image.png|200](https://s1.vika.cn/space/2023/03/28/86c13b79c1b74f2da0a0934fa2e55a6b)


