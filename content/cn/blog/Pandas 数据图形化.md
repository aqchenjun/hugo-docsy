---
categories: Pandas
title: Pandas 数据图形化
tags: [ 教程, python]
date: 2023-03-17
lastmod: 2023-03-23 
thumbnail: https://s1.vika.cn/space/2023/03/25/c2f98a321b754d918b3cbdcb48071711?attname=v2-3cfb6bc76bddb7ecb42045a48ab3be8f_1440w.jpg
published: "true"
slug: q10m3ut
---



```python
df.plot(x=None,        #label or position
        y=None,        #label, position or list of label, positions
        kind='line',
        ax=None,
        subplots=False,
        layout=(rows, columns),        #for the layout of ubplots
        figsize = None,
        legend=True,            #显示图例
        **kwargs
        )
```

  

## 1 kind参数：

- ‘line’ : line
- ‘bar’ : vertical bar
- ‘barh’ : horizontal bar
- ‘hist’ : histogram    - ‘box’ : box
- ‘kde’ : Kernel Density Estimation     - ‘density’ : same as ‘kde’
- ‘area’ : area
- ‘pie’ : pie
- ‘scatter’ : scatter (DataFrame only)
- ‘hexbin’ : hexbin (DataFrame only)      

{{% alert title=注意 color='success' %}}需要注意的是：plot() 函数默认的X轴刻度是 DataFrame 的index，如果需要通过 xticks()函数调整X轴刻度的话，其 ticks 参数应为 DataFrame 的index列表或其切片，如果需要通过 text()函数设置文本，参数x应为 DataFrame 的index列表或其切片。 或者将 plot() 函数的 use_index 参数设为 False，以保持与 matplotlib 的处理方法一致。

 {{% /alert %}}
t %}}
