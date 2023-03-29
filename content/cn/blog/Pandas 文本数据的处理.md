---
categories: Pandas
title: Pandas 文本数据的处理
tags: [ 教程, python]
date: 2023-03-28
lastmod: 2023-03-28 
slug: 20230328085937
thumbnail:  
published: "true"
---


## 1 拆分
  

```python
Series.str.split(pat=None,
                n=- 1,     #从左开始的最大分割次数
                expand=False)
Series.str.rsplit(pat=None,
                    n=- 1,    #从右边开始的最大分割次数
                    expand=False)
```
 
## 2 合并与连接  

```python
#字符串的合并
Series.str.cat(others=None,    #需要合并的对象，如果为空，则合并自身。如果有值，以索引为主键，相应合并
                sep=None,
                na_rep=None,    #缺失值的替代值
                join='left')
#序列的连接
pandas.concat(objs,        #需要连接的序列
               axis=0,
               join='outer',
               ignore_index=False,
               keys=None,
               levels=None,
               names=None,
               verify_integrity=False,
               sort=False,
               copy=True)
```
 
## 3 替换  

### 3.1 where/mask  方法  

```python
Series.mask(cond,
            other=nan,  
            axis=None,
            level=None)
Series.where(cond,
             other=NoDefault.no_default,
             axis=None,
             level=None)
```

mask方法，如果 cond 为真，则替换为other。  

where方法，如果 cond 不为真，则替换为other。  

### 3.2 replace/str.replace方法  

replace多用于DataFrame数据的整体替换，str.replace多用于Series字符串的替换。  

```python
DataFrame.replace(to_replace=None,
                  value=NoDefault.no_default,
                  limit=None,
                  regex=False) 
Series.str.replace(pat,
                    repl, 
                    n=- 1,        #从左开始的替换次数
                    case=None,    #是否区分大小写
                    flags=0,
                    regex=None)
```

repl 可以是函数，传递给函数的参数是match对象。如果没有分组，取值方法是m.group(0)或者 m[0]，如果有分组，取值方法是m.group(组号)或者 m[组号]，也可以直接以 r'\1' 、r'\2' 取值。  

函数替换实例：  

```python
repl = lambda m: m.group(0)[::-1]
ser = pd.Series(['foo 123', 'bar baz', np.nan])
ser.str.replace(r'[a-z]+', repl, regex=True)
#结果
0    oof 123
1    rab zab
2        NaN
dtype: object 

pat = r"(?P<one>\w+) (?P<two>\w+) (?P<three>\w+)"
repl = lambda m: m.group('two').swapcase()
ser = pd.Series(['One Two Three', 'Foo Bar Baz'])
ser.str.replace(pat, repl, regex=True)
#结果
0    tWO
1    bAR
dtype: object
```
  
## 4 提取  

```python
Series.str.extract(pat,
                    flags=0,
                    expand=True)
Series.str.extractall(pat, flags=0)
```

## 5 匹配  

### 5.1 contains 类(Series 中每个元素包含指定 pat)  

```python
Series.str.contains(pat,
                    case=True,
                    flags=0,
                    na=None,    #对缺失值的填充，可以设为False
                    regex=True)
Series.str.startswith(pat,        #可以是字符，也可以是字符串
                      na=None)
Series.str.endswith(pat, na=None)
```

### 5.2 isin 类(Series 、DataFrame、Index中每个元素在指定 value 中)  

```python
Series.isin(values)    #values取值为set or list-like，但不能为字符串。字符串应转换为列表

#取反
~Series.isin(values)
Index.isin(values,
           level=None)    #MultiIndex 时有效
DataFrame.isin(values)    #values 可以取值 dict，表示分 column 判断
```

## 6 其他  

### 6.1 字母的大小写转化  

`upper、 lower、title、 capitalize、 swapcase`  

### 6.2 字符型数字转数字型  

```python
pandas.to_numeric(arg,
                  errors='raise',
                  downcast=None)    #表示成功转换后的数字最小数值类型
```

errors 有3种取值：{‘ignore’, ‘raise’, ‘coerce’}, default ‘raise’
- If ‘ignore’，then invalid parsing will return the input。
- If ‘raise’，then invalid parsing will raise an exception。
- If ‘coerce’，then invalid parsing will be set as NaN。  

downcast 有4种取值：‘integer’, ‘signed’, ‘unsigned’, or ‘float’。
- ‘integer’ or ‘signed’: smallest signed int dtype (min.: np.int8)
- ‘unsigned’: smallest unsigned int dtype (min.: np.uint8)
- ‘float’: smallest float dtype (min.: np.float32)

### 6.3 统计  

```python
Series.str.count(pat, flags=0)    #统计正则表达式 pat 在 Series 各字符串中出现的次数
Series.str.len()
Series.str.find(sub, start=0, end=None) #返回字符串 sub 在 Series 各字符串中出现的最小位置
```

### 6.4 填充/除空  

```python
Series.str.pad(width,            #结果字符串的最小宽度
               side='left',      #填充方式，‘left’, ‘right’, ‘both’, default ‘left’
               fillchar=' ')     #填充字符串，default ‘ ‘
Series.str.strip(to_strip=None)
Series.str.lstrip(to_strip=None)
Series.str.rstrip(to_strip=None)
```