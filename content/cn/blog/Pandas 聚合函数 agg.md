---
source: Pandas
title: Pandas 聚合函数 agg
tags: [ 教程, python]
date: 2023-03-17
lastmod: 2023-03-22 
thumbnail: https://s1.vika.cn/space/2023/03/25/66f0f22ad0f647debc038a14caf617f7?attname=sunrise-7852888_960_720.jpg
published: "true"
slug: 20230317174438
---


## 1 参数  

```python
DataFrame.agg(func=None,
              axis=0,
              *args,
              **kwargs)
```  

参数：  

func 的取值：
- 函数
- 函数字符名
- 上述2类的列表
- 字典或元组  

axis 的取值：
- 0 or ‘index’: 对每列进行聚合计算
- 1 or ‘columns’: 对每行进行聚合计算
- args：Positional arguments to pass to func。
- *kwargs：Keyword arguments 。必须以元组形式 (column, aggfunc) 出现
  
## 2 示例  

```python
df = pd.DataFrame([[1, 2, 3],
                    [4, 5, 6],
                    [7, 8, 9],
                    [np.nan, np.nan, np.nan]],
                   columns=['A', 'B', 'C'])
#func 取值函数名列表
df.agg(['sum', 'min'])
#结果
        A     B     C
sum  12.0  15.0  18.0
min   1.0   2.0   3.0  

#func 取值字典，不同的列聚合函数不相同
df.agg({'A' : ['sum', 'min'], 'B' : ['min', 'max']})
#结果
        A    B
sum  12.0  NaN
min   1.0  2.0
max   NaN  8.0 

#func 取值元组，对聚合结果赋值。赋值的变量名称应符合 python 要求，否则出错。另外，也可以在聚合完成后，用 rename 方法。
df.agg(x=('A', max), y=('B', 'min'), z=('C', np.mean))
#结果
     A    B    C
x  7.0  NaN  NaN
y  NaN  2.0  NaN
z  NaN  NaN  6.0

#设置 **kwargs，对聚合结果赋值。赋值的变量名称可以不符合 python 要求
df.agg(**{
           "total values":("B", sum)
           }
       )  

#axis=1,对每行进行聚合计算
df.agg("mean", axis="columns")
#结果
0    2.0
1    5.0
2    8.0
3    NaN
dtype: float64
```