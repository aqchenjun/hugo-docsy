---
categories: Pandas
title: Pandas 数据集的连接
tags: [ python, 教程]
date: 2023-03-17
lastmod: 2023-03-22
thumbnail: https://s1.vika.cn/space/2023/03/25/3c22c65f9b8a4c469656f8f3d5a1b87e?attname=business-5475661_960_720.jpg
published: "true"
slug: lgii2rg
---


## 1 concat 函数  

```python
pandas.concat(objs,   #连接对象列表
              axis=0,          #连接方向，{0/’index’, 1/’columns’}, default 0
              join='outer',    #连接方式，{‘inner’, ‘outer’}, default ‘outer’
              ignore_index=False,

              keys=None,       #数据来源标签列表，与 objs 对应。将形成 MultiIndex。keys 位于 level=0。
              levels=None,     #数据来源标签列表，keys 应是其子集。
              names=None,      #多级索引的名称
               )
```
 
## 2 merge 函数  

```python
DataFrame.merge(right,
                how='inner',
                on=None,   #连接字段
                left_on=None,  
                right_on=None,
                left_index=False,    #左表的 index 是否为连接字段
                right_index=False,
                suffixes=('_x', '_y'),    #左、右表相同列名的后缀
                indicator=False)        #数据来源的标识，“left_only” 、 “right_only”  和“both”
```  

## 3 join 函数  

涉及到索引连接的，尤其涉及到多级索引的，pandas推荐使用 join 函数。  

```python
DataFrame.join(other,
               on=None,    #左表列或索引层的名称
               how='left',
               lsuffix='',
               rsuffix='',
               sort=False)
```  

## 4 左表连接字段是列→右表 index

将参数 on 设置为左表连接字段的名称。  

```python
left = pd.DataFrame({
           "A": ["A0", "A1", "A2", "A3"],
           "B": ["B0", "B1", "B2", "B3"],
           "key": ["K0", "K1", "K0", "K1"],
          }
 )
right = pd.DataFrame({"C": ["C0", "C1"], "D": ["D0", "D1"]}, index=["K0", "K1"])
left.join(right, on="key",how='inner')    #如果 on 是列表，则右表应是 MultiIndex
#pd.merge(left, right, left_on="key", right_index=True, how="inner", sort=False)
``` 

## 5 左表单级索引→右表多级索引  

左表的索引名称要能够对应右表多级索引中的一个索引名称。  

```python
left = pd.DataFrame(
                   {"A": ["A0", "A1", "A2"], "B": ["B0", "B1", "B2"]},                   index=pd.Index(["K0", "K1", "K2"], name="key"),
   )
index = pd.MultiIndex.from_tuples(
                                   [("K0", "Y0"), ("K1", "Y1"), ("K2", "Y2"), ("K2", "Y3")],
                                   names=["key", "Y"],
   )
right = pd.DataFrame(
                       {"C": ["C0", "C1", "C2", "C3"], "D": ["D0", "D1", "D2", "D3"]},
                       index=index)
left.join(right,how='inner')    #左表的索引是 key，对应右表多级索引中的 key 索引
#pd.merge(left.reset_index(),right.reset_index(), on=["key"], how="inner").set_index(["key","Y"])
```  

## 6 多级索引→多级索引  

这种连接的前提条件是：右表的多级索引全部应用于连接，而且是左表多级索引的子集。  

```python
leftindex = pd.MultiIndex.from_product([list("abc"), list("xy"), [1, 2]], names=["abc", "xy", "num"])
left = pd.DataFrame({"v1": range(12)}, index=leftindex)
rightindex = pd.MultiIndex.from_product([list("abc"), list("xy")], names=["abc", "xy"])
right = pd.DataFrame({"v2": [100 * i for i in range(1, 7)]}, index=rightindex)
left.join(right, on=["abc", "xy"], how="inner")
```  

## 7  concat、merge、join的比较：


|     | concat | merge | join |
| --- | ------ | ----- | ---- |
|| 调用方法 | pd.concat() | DataFrame.merge()、pd.merge() | DataFrame.join() |
| 连接方向 | 行、列，由axis 参数确定 | 列 | 列 |
| 连接字段 | 索引和列 | 自主设定 | 索引—索引、列—索引 |
| 数据来源标识 | keys 参数，形成 MultiIndex | indicator 参数，增加一列 |  |

## 8 序列与表的合并  

### 8.1 追加序列到表的行末  

相当于添加一条记录。新版 pandas 不推荐 append 方法，而是推荐 concat。由于 Series 是一列，不能直接 concat ，可以先转换为 DataFrame。  

```python
df1 = pd.DataFrame({'Name':['San Zhang','Si Li'],'Age':[20,21]})
s = pd.Series(['Wu Wang', 21])
df_s = pd.DataFrame(data=[s.values],columns=df1.columns)    #data=[s.values]，表示这是一行数据，如果去掉 []，表示这是一列数据
s.index=df1.columns
df_s = s.to_frame().T       #结果一致
pd.concat([df1,df_s])
```  

df1:

![image.png|150](https://s1.vika.cn/space/2023/03/17/96686d1479444ebbae04fad5e686c2a9)


  

concat 结果：

![image.png|150](https://s1.vika.cn/space/2023/03/17/1a1e6384f94d4336818fef5a07677edc)

## 9 关于 combine  

combine 函数能够让两张表按照一定的规则进行组合，新表的列和索引是这两张表的并集。  

例：选出对应索引位置较小的元素  

```python
df1 = pd.DataFrame({'A':[1,2], 'B':[3,4], 'C':[5,6]})
df2 = pd.DataFrame({'B':[5,6], 'C':[7,8], 'D':[9,10]}, index=[1,2])
df1.combine(df2, lambda s1,s2:s1.where((s1.isna())|(s1<s2),s2))
#s1中的元素是NaN，或者s1中的元素小于s2中相应索引位置的元素，保持s1不变，否则用s2中相应索引位置的元素替换（可能是数值，也可能是NaN，因为NaN也不满足“s1中的元素小于s2中相应索引位置的元素”）
```

lambda 函数展开来：  

```python
def choose_min(s1, s2):
    res = s1
    for index in s1.index:
            if pd.notna(s1[index]):
                if pd.isna(s2[index]):
                    res[index] = s2[index]
                elif s1[index]>s2[index]:
                    res[index] = s2[index]
    return res
```  

除了 combine 之外，pandas 中还有一个 combine_first 方法，其功能是在对两张表组合时，若第二张表中的值在第一张表中对应索引位置的值不是缺失状态，那么就使用第一张表的值填充。  

```python
df1 = pd.DataFrame(data = {'A':[1.0,2.0,np.nan],'B':[4.0,np.nan,6.0],'C':[7.0,np.nan,np.nan]},
                  index = ['one','two','three'])
df2 = pd.DataFrame(data = {'A':[5.0,2.0,3],'B':[np.nan,np.nan,6.0],'C':[7.0,8,np.nan]},
                  index = ['one','two','four'])
df1.combine_first(df2)
#df1.combine(df2,lambda s1,s2:s1.mask((s1.isna())&(s2.notna()),s2)) 效果一致
```��
```