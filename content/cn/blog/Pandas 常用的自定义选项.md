---
categories: Pandas
title: Pandas 常用的自定义选项
tags: [ 教程, python]
date: 2023-03-14
lastmod: 2023-03-20
thumbnail: 
published: "true"
slug: 20230314200913
---


## 1 自定义的途径  

```python
pd.set_option()
pd.get_option()
pd.reset_option()
pd.options
pd.style
```

## 2 显示所有行、列  

```python
pd.set_option('display.max_rows', None) # 显示所有行，None 可以为数值
pd.set_option('display.max_columns', None) # 显示所有列
pd.options.display.max_rows=None  
#还原设置：
pd.reset_option("display.max_columns")
pd.reset_option('display.max_rows')
```

## 3 修改每列最大字符宽度  

```python
pd.set_option('display.max_colwidth',10)
pd.options.display.max_colwidth=10
df.style.set_properties(subset=None,**{'width':'100px'})
```

## 4 全局修改小数点精度  

```python
pd.set_option('display.precision',2)
# DataFrame.style.set_precision(2)
#Series.apply(round,ndigits=2)
```

## 5 折叠  

当我们输出数据宽度，超过了设置的宽度时，是否要折叠。通常使用False不折叠，相反True要折叠。  

```python
pd.set_option("expand_frame_repr", True)  # 折叠
pd.set_option("expand_frame_repr", False)  # 不折叠，默认值
```

## 6 忽略警告  

```python
# import warnings
# warnings.filterwarnings('ignore')
pd.set_option('mode.chained_assignment',None)
```

## 7 重置  

```python
pd.reset_option('^display')    #重置所有 display 开头的选项
pd.reset_option('all')    #重置所有选项
```

## 8 隐藏索引、列头、subset  

```python
DataFrame.style.hide(subset=None,
                     axis=0,
                     level=None,
                     names=False)
#隐藏索引
DataFrame.style.hide(axis=0)
```

## 9 标记缺失值  

将缺失值标记为“数据缺失“  

```python
DataFrame.style.set_na_rep('数据缺失')
#Series.apply(func=fillna,value='数据缺失')    #fillna 是缺失值填充方法，value 是传递给 fillna的参数
```

## 10 修改字体颜色  

修改“salary”列的颜色  

```python
DataFrame.style.set_properties(subset='salary',**{'color':'red'})
```

## 11 修改整个 DataFrame 背景颜色、对齐方式、字体大小  

并将 salary 列字体修改为红色  

```python
DataFrame.style.set_properties(**{'background-color':'blue','text-align':'center','font-size':'13px','color':'red'}).set_properties(subset='salary',**{'color':'red'})
```

## 12 applymap函数  

将 salary 列数值大于 30000 的单元格字体修改为红色。applymap 函数的参数就是 subset 指定的列。  

```python
def fu(val):
    color = 'red' if val>30000 else 'black'
    return 'color:{}'.format(color)
data.style.applymap(fu,subset='salary')
```

## 13 格式化输出日期类型  

使用 strftime() 函数。将 createTime 列格式化输出为 xx年xx月xx日。如果 createTime 不是 datetime64类型，首先用 pd.to_datetime()函数转换为datetime64类型。  

```python
data.style.format(formatter=lambda x:x.strftime('%Y年%m月%d日'),subset=['createTime'])
data['createTime'].dt.strftime('%Y年%m月%d日')
#data['createTime'].apply(lambda x:x.strftime('%Y年%m月%d日'))
```

## 14 自定义格式化数据  

在 salary 列后增加"元"，对 matchScore 列保留两位小数并增加"分"。  

```python
DataFrame.style.format('{}元'.format,subset='salary').format('{:.2f}分'.format,subset='matchScore')
#Series.apply(lambda x:'{.2f}分'.format(x))
```

## 15 设置列标题的对齐方式  

```python
pd.set_option("colheader_justify", "right")
pd.set_option("colheader_justify", "left")
```

## 16 用 Style 对象设置自定义选项

用 HTML 和 CSS 格式 

```python
df.style.apply(func, axis=0, subset=None, **kwargs)
df.style.format(formatter=None, subset=None, na_rep=None, precision=None, decimal='.', thousands=None, escape=None, hyperlinks=None))
df.style.clear()    #清除所有自定义格式  
df.style.set_properties(subset=None, **kwargs)
df.style.set_properties(color="white", align="right")  
df.style.set_properties(**{'background-color': 'yellow'})
```



