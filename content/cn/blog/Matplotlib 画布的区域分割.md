---
source: Matplotlib
title: Matplotlib 画布的区域分割
tags: [ 教程, python]
date: 2023-03-13
lastmod: 2023-03-17 
thumbnail:  
published: "true"
---


## 1 等分分割  

实现效果：

![image.png](https://s1.vika.cn/space/2023/03/13/f36e592651dd42b2a42277b82a6d1fd7) 

### 1.1 方法一：使用add_subplot  

```python
fig,ax = plt.subplots(figsize=(10,6))
ax1 = fig.add_subplot(121,facecolor='red')
ax2 = fig.add_subplot(222,facecolor='blue')
ax3 = fig.add_subplot(224,facecolor='green')
```

### 1.2 方法二：使用subplot  

```python
fig,ax = plt.subplots(figsize=(10,6),dpi=100)
ax0 = plt.subplot(121,facecolor='red')
ax1 = plt.subplot(222,facecolor='blue')
ax2 = plt.subplot(224,facecolor='green')
```

### 1.3 方法三：使用subplot2grid  

```python
fig,ax= plt.subplots(figsize=(10,6),dpi=100)
ax1 = plt.subplot2grid((2,2),(0,0),rowspan=2,facecolor='red')
ax2 = plt.subplot2grid((2,2),(0,1),rowspan=1,colspan=1,facecolor='blue')
ax3 = plt.subplot2grid((2,2),(1,1),facecolor='green')
```

### 1.4 方法四：使用add_axes  

```python
fig = plt.figure(figsize=(10,6))
ax1 = fig.add_axes([0,0,0.5,1],facecolor='red')
ax2= fig.add_axes([0.5,0.5,0.5,0.5],facecolor='blue')
ax3 = fig.add_axes([0.5,0.0,0.5,0.5],facecolor='green')
```
上述代码运行后，有重叠，调整add_axes中的参数后，完美解决问题。  

```python
fig = plt.figure(figsize=(10,6))
ax1 = fig.add_axes([0,0,0.48,1],facecolor='red')
ax2= fig.add_axes([0.53,0.53,0.47,0.47],facecolor='blue')
ax3 = fig.add_axes([0.53,0.0,0.47,0.47],facecolor='green')
```


## 2 非等分分割 

实现效果：

![image.png](https://s1.vika.cn/space/2023/03/13/c9d054ab5e814ab59875999544d24add)
 
### 2.1 方法一：使用subplot2grid  

```python
fig = plt.figure(figsize=(5,3),dpi=100)
ax1 = plt.subplot2grid((3,3),(0,0),rowspan=1,colspan=2,facecolor='red')
ax2 = plt.subplot2grid((3,3),(1,0),rowspan=2,colspan=2,facecolor='blue')
ax3 = plt.subplot2grid((3,3),(0,2),rowspan=3,colspan=1,facecolor='green')
```

### 2.2 方法二：使用add_axes  

```python
fig = plt.figure(figsize=(5,3),dpi=100)
ax1 = fig.add_axes([0.0,0.66,0.6,0.33],facecolor='red')
ax2 = fig.add_axes([0.0,0.0,0.6,0.55],facecolor='blue')
ax3 = fig.add_axes([0.66,0.0,0.3,1],facecolor='green')
```

  


