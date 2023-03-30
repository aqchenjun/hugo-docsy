---
categories: Matplotlib
title: Matplotlib 常见设置
tags: [ 教程, python]
date: 2023-03-13
lastmod: 2023-03-15
thumbnail: https://s1.vika.cn/space/2023/03/13/7d30c13b23104b94b79e25d9b22f21a7?attname=20230313060628.png
published: "true"
slug: 1zou6a5
---
 
>坐标轴以X轴为例  

## 1 中文及文本显示  

### 1.1 全局设定  

```python
plt.rcParams['font.family']=['SimHei']
#plt.rcParams['font.sans-serif']=['SimHei']  与上面语句功能相同
plt.rcParams['axes.unicode_minus']=True  #设置坐标轴上的负号
```

### 1.2 单独设置  

可以采用Text properties，灵活方便，不仅可以显示中文，还可以设置字体属性。主要分两种情况：  
1. 类似plt.title()、plt.xlabel()函数，其特点是函数有fontdict参数和kwargs参数，可以设置中文以及字体属性。
2. 类似plt.xticks()函数，其特点是函数没有fontdict参数但是有kwargs参数，也可以设置中文以及字体属性。
    1. kwargs：关键字参数，用来控制文本的展示：例如字体大小、字体样式等 。
    2. 主要关键字包括backgroundcolor、color、family、rotation、size、style、weight、linespacing等。可以字典形式。
3. 类似plt.pie()函数，其特点是函数没有fontdict参数但是有textprops参数，用法与 fontdict参数一致。
4. legend()函数，字体设置的参数是 prop，用法与 fontdict参数一致，但无法设置颜色 color，颜色用labelcolor 参数。其标题参数是title，标题字体设置参数 title_fontproperties，用法与 fontdict参数一致。

```python
font = {'family':"SimHei","size":20,"color":'red','rotation':20}
plt.title(fontdict=font) 或者 plt.title(**font)
plt.xticks(**font)
```

## 2 常用字体 
- 宋体：SimSun
- 黑体：SimHei
- 微软雅黑：Microsoft YaHei
- 微软正黑体：Microsoft JhengHei
- 新宋体：NSimSun
- 新细明体：PMingLiU
- 细明体：MingLiU
- 标楷体：DFKai-SB  

## 3 图形标题  

```python
plt.title(label,
          fontdict=None, #字体属性
          loc='center', #定位，'left', 'center', 'right'
          pad=6.0,    #与轴线的距离，单位是 points
          y=None,
          **kwargs)      #文本属性
ax.set_title(label,
             fontdict=None,
             loc=None,
             pad=None,
             y=None,
             **kwargs)
```

## 4 坐标轴的名称  

```python
plt.xlabel(xlabel,
           fontdict=None,#字体属性
           labelpad=4.0, #与轴线的距离，单位是 points 
           loc='center', #定位，'left', 'center', 'right'
           **kwargs)  #文本属性
ax.set_xlabel(xlabel,
              fontdict=None,
              labelpad=None,
              loc=None,
              **kwargs)
```

## 5 坐标轴刻度  

```python
plt.xticks(ticks=None,#array
           labels=None, #替代 ticks 的 array
           *,
           minor=False,
           **kwargs)   #文本属性           
ax.set_xticks(ticks, labels=None, *, minor=False, **kwargs)

#设置刻度的位置
ax.xaxis.set_ticks_position(position)        
#'position':{'top', 'bottom', 'both', 'default', 'none'}
ax.yaxis.set_ticks_position(position)        
#'position':{'left', 'right', 'both', 'default', 'none'}  

#设置刻度范围
ax.set_xlim(left=None, right=None, *, emit=True, auto=False, xmin=None, xmax=None)
ax.set_ylim(bottom=None, top=None, *, emit=True, auto=False, ymin=None, ymax=None)  

#刻度的显示位置
ax.xaxis.set_ticks_position(position)      
#{'top', 'bottom', 'both', 'default', 'none'}
ax.yaxis.set_ticks_position(position)      
#{'left', 'right', 'both', 'default', 'none'} 
#刻度线的属性设置
ax.tick_params(axis='both', **kwargs)
ax.tick_params(bottom=True, top=False, left=False, right=False)    #只显示X轴的刻度线
```

>需要注意的是，当需要调整刻度的文本属性的时候，labels 不能为 None。  

## 6 坐标脊柱  

```python
#不显示x、y坐标轴
plt.axis('off')
#不显示x轴
ax = plt.gca()
ax.axes.get_xaxis().set_visible(False)
#不显示x轴
ax.axes.get_yaxis().set_visible(False)  
#spines 坐标脊柱类属性包括 set_visible()、set_color()、set_position()、set_linewidth()、set_linestyle()
#去掉脊柱(坐标线)
ax.spines['top'].set_visible(False) #去掉上边框
ax.spines['bottom'].set_visible(False) #去掉下边框
ax.spines['left'].set_visible(False) #去掉左边框
ax.spines['right'].set_visible(False) #去掉右边框  
#设置 X 轴和Y 轴的交点
ax.spines['bottom'].set_position(('data',0.5))        #x 轴落在Y轴的0.5刻度处
ax.spines['left'].set_position(('data',1.0))  #Y 轴落在X轴的1.0刻度处
```

## 7 图例  

```python
plt.legend(handles=None,
           labels=list of str
           loc='best',            #'upper left', 'upper right', 'lower left', 'lower right','upper center', 'lower center', 'center left', 'center right','center','best'
           prop=fontdict,
           fontsize=int or {'xx-small', 'x-small', 'small', 'medium', 'large', 'x-large', 'xx-large'},
           labelcolor='black',    #str or list,图例文字的颜色
           facecolor='white',     #图例的背景色
           title=None,  #图例的标题
    title_fontproperties=fontdict,    #图例标题的字体属性
           title_fontsize=,       #图例标题的字号，
           )
```

## 8 标注  

```python
plt.text(x, y,   #标注所在的位置
         s,  #标注文字内容
         fontdict=None,               #标注字体属性
         **kwargs)  #文本属性
ax.text(x, y, s, fontdict=None, **kwargs)
```

>标注示例：数据  

```python
df = pd.DataFrame(np.random.rand(5, 4), columns=["a", "b", "c", "d"])
df
```

  ![image.png](https://s1.vika.cn/space/2023/03/13/dcdcfddd4b814271a4a80c2854919e98)
  

```python
fig,ax = plt.subplots(figsize=(10,6))
df.plot(kind='bar',ax=ax,width=0.5)
for x in df.index:
    ax.text(x-0.3,df2.loc[x,'a'],df2.loc[x,'a'].round(2),va='bottom')
    ax.text(x-0.15,df2.loc[x,'b'],df2.loc[x,'b'].round(2),va='bottom')    ax.text(x,df2.loc[x,'c'],df2.loc[x,'c'].round(2),va='bottom')    ax.text(x+0.15,df2.loc[x,'d'],df2.loc[x,'d'].round(2),va='bottom')
```

  ![image.png](https://s1.vika.cn/space/2023/03/13/e4efce5427c84bb0bf559b8412f5ab62)


```python
df.plot(kind='bar',stacked=True,ax=ax,width=0.5)
for x in df.index:    ax.text(x,df.loc[x,'a'],df.loc[x,'a'].round(2),va='bottom',ha='center')    ax.text(x,df.loc[x,'a']+df.loc[x,'b'],(df.loc[x,'a']+df.loc[x,'b']).round(2),va='bottom',ha='center')    ax.text(x,df.loc[x,'a']+df.loc[x,'b']+df.loc[x,'c'],(df.loc[x,'a']+df.loc[x,'b']+df.loc[x,'c']).round(2),va='bottom',ha='center')    ax.text(x,df.loc[x,'a']+df.loc[x,'b']+df.loc[x,'c']+df.loc[x,'d'],(df.loc[x,'a']+df.loc[x,'b']+df.loc[x,'c']+df.loc[x,'d']).round(2),va='bottom',ha='center')
```

![image.png](https://s1.vika.cn/space/2023/03/13/3882fc170542454b9b459248e06de0ca)
e0ca)
