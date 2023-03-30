---
categories: Pandas
title: Pandas 增改删查
tags: [ 教程, python]
date: 2023-03-17
lastmod: 2023-03-23 
thumbnail: https://s1.vika.cn/space/2023/03/25/c09aa983ece747039ec86af84373b3c5?attname=technology-1283624__340.jpg 
published: "true"
slug: jr64764
---


## 1 增  
### 1.1 增加列

```python
df['新列名'] = data
#或者
df.assign(**kwargs)
#新列的值等于column1 和 column2 两列的和
df.assign(新列名=lambda x:x[column1]+x[column2]) #函数的参数是一行，x[column1]表示该行、列名为 column1 的值，
df.assign(最多奖牌数量=lambda x:x[['金牌数','银牌数','铜牌数']].max(axis=1))
df.assign(列名1=data1,
          列名2=data2)
#或者
pd.concat(df,df1,axis=1)
#或者
df.insert(
    loc='int',#列插入的位置
    column=,    #插入的列名
    value=,    #值
    allow_duplicates = False)
```
  
### 1.2 增加行  

```python
#插入行末
df.iloc[df.shape[0]] = data
#或者利用 concat函数，因为 pandas 不建议使用 append
df = pd.DataFrame({'Name':['San Zhang','Si Li'],'Age':[20,21]})
s = pd.Series(['Wu Wang', 21])
df_s = pd.DataFrame(data=[s.values],columns=df.columns)    #data=[s.values]，表示这是一行数据，如果去掉 []，表示这是一列数据
pd.concat([df,df_s])
#插入指定位置
#思路一，使用该位置上、下两个索引的平均值，df.iloc[平均值]=data，然后 reset_index(drop=True)
#思路二，以该指定位置为分割线，将 df 分割成2部分，df1和df2，然后 pd.concat([df1,data,df2],ignore_index=True)
```

## 2 改 

### 2.1 直接修改值  

直接定位需要修改的单元格、行或列，然后赋新值。或者使用 replace。还可以使用map、transform、apply。  

```python
df.replace(
    to_replace=None,    #旧值
    value=<no_default>,    #新值
    inplace = False,
    regex = False,    #是否启用正则表达式
    )
df['gender'].replace({'female':0,'male':1})     # gender 列中的 female 修改为 0，male 修改为1
#df.replace({'gender':{'female':0,'male':1}})
Series.str.replace(pat,
                    repl,
                    case=None, #是否区分大小写
                    regex=True)
```  

### 2.2 修改列名或索引名  

```python
df.rename(index=None,
        columns=None,
        axis=None,
        inplace=False)
```  

### 2.3 修改轴名  

```python
df.rename_axis(index=None,    #索引轴名
               columns=None,  #列轴名
               axis=None,     #轴名
               inplace=False)
```  

## 3 删  

```python
DataFrame.drop( index=None,    #欲删除的索引
                columns=None,  #欲删除的列
                level=None,
                inplace=False, errors='raise')
```  

## 4 查  

`df[ ]、df.loc[ ]、df.iloc[ ]、df.query()、df.where()、 df.mask()`mask()`