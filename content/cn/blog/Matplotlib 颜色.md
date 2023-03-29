---
categories: Matplotlib
title: Matplotlib 颜色
tags: [ 教程, python]
date: 2023-03-13
lastmod: 2023-03-19 
thumbnail: https://s1.vika.cn/space/2023/03/25/b328b4afaf414fc889df0d416e9faa15?attname=u%3D370758891%2C1483979753%26fm%3D253%26fmt%3Dauto%26app%3D138%26f%3DJPEG.webp
published: "true"
slug: 20230313170442
---
 
Matplotlib 的颜色设置有两种情况，
- 一是单一颜色，参数是 c ，
- 另一种是多种颜色，参数是 c 和 cmap。  

## 1 单一颜色  

### 1.1  指定颜色名称  

颜色名称的列表如下：  

![image.png](https://s1.vika.cn/space/2023/03/13/7a99d2a5708e49d88f62ed21f9ea1abf)

用法：以scatter方法为例，通过参数 c 设置颜色，参数 c 定义数据点 marker 的颜色。  

```python
plt.scatter(x,y,c='palegreen')
```

### 1.2  字符串格式的数字

表示为灰度，例如0.75表示中浅灰色。

```python
plt.scatter(x,y,c='0.75')
```

## 2 多种颜色  

### 2.1  数值列表  

该数值列表长度与x、y一致，表示在cmap中的映射。cmap参数表示颜色实例，默认为'viridis'。  

方法：如果用plot方法，只能设置线条的单一颜色。改用scatter方法。  

```python
import matplotlib.pyplot as plt
import numpy as np
x = np.linspace(0, 3 * np.pi,500)
y = np.sin(x)
colors = np.arange(x.size)
fig,ax = plt.subplots(figsize=(10,6))
ax.scatter(x,y,s=10,c=colors,cmap='viridis')
# ax.scatter(x,y,s=10,c=colors,cmap='viridis_r')
```
matplotlib 中的所有内置颜色实例名称可以用 plt.get_cmap() 得到。颜色实例名称后加"_r"，表示映射顺序为逆向。cmap主要分为4大类：
  
>perceptually uniform sequential colormap 渐变色系，可以用于大多数科学数据。

![image.png](https://s1.vika.cn/space/2023/03/13/581da52c54004fc286947155d35b4deb)

>sequential colormap 单一色系

![image.png](https://s1.vika.cn/space/2023/03/13/d9f99bb79d2845ac86c0700405dbf08f)

![image.png](https://s1.vika.cn/space/2023/03/13/68d97a4475604dddb52cf4d9984f72b9)
  
>diverging colormap 发散色图，可以用于数据的中间值很大的情况。

![image.png](https://s1.vika.cn/space/2023/03/13/dbbaceed367c416c832c5913050b00f3)

>cyclic colormap 循环色图

![image.png](https://s1.vika.cn/space/2023/03/13/3bafb18681804baba6e4dd223316c15a)

>qualitative colormap

![image.png](https://s1.vika.cn/space/2023/03/13/3f864ea428434c1481a081717cfa350e)

>miscellaneous colormap

![image.png](https://s1.vika.cn/space/2023/03/13/36f1580a0e0f408a9289bf51bc5ff2c9)
 
### 2.2  二维元组数组  

该数组每行的元素是三元组(Triplets)或四元组(Quadruplets)：三元组，即颜色的红、蓝、绿分量，其中每个分量在[0,1]区间内。因此，(1.0, 0.0, 0.0)表示纯红色，而(1.0, 0.0, 1.0)则表示粉色。四元组(Quadruplets)，前三个元素与三元组定义相同，第四个元素定义透明度值，此值也在[0,1]区间内。因此，(1.0, 0.0, 0.0,1.0)表示纯红色，不透明，而(1.0, 0.0, 1.0,0.5)则表示粉色，透明度为0.5。  

```python
plt.scatter(x,y,c=(1.0, 0.0, 0.0))
plt.scatter(x,y,c=(1.0, 0.0, 1.0,0.5))
```