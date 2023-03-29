---
categories: Pandas
title: Pandas 分块读取大文件
tags: [ 教程, python]
date: 2023-03-28
lastmod: 2023-03-28 
slug: 20230328093023
thumbnail:  
published: "true"
---


## 1 直接用分块方式读取数据集文件  

以.csv文件为例，在read_csv()中加入**chunksize**参数即可实现分块读取，返回的是一个可迭代对象。 

TextFileReader，这个可迭代对象不能用下标访问。 可以用 for 循环遍历，或者get_chunk()函数。  

```python
reader = pd.read_csv('data.csv', sep=',', chunksize=10)    
#分 10 块读取文件

reader.get_chunk(6)    
# 得到一个 DataFrame，有6条数据

for df in reader:
    print(df)     #每个 df 都是一个 DataFrame，
```

需要注意的是，TextFileReader 对象的读取模式是依序读取，不重复。针对上述语句，reader.get_chunk(6)获得第一到第六条数据，后面的 print(df) 语句从第七条数据开始。  

## 2 先将数据集读取为可迭代对象，再分块读取 

以.csv文件为例，在read_csv()方法中指定参数iterator为True，返回一个可迭代对象  

TextFileReader。可以用 while 循环遍历，或者get_chunk()函数。  

```python
reader = pd.read_csv('data.csv', iterator=True)

data = reader.get_chunk(5) # 返回5行数据
while True:
    try:
        print(reader.get_chunk(10))    
    #每次读 10 行数据，由于前面有一个get_chunk(5) 语句，因此从第六条语句开始
    except StopIteration:
        break
```