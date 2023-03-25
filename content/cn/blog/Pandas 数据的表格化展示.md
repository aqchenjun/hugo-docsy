---
source: Pandas
title: Pandas 数据的表格化展示
tags: [ 教程, python]
date: 2023-03-14
lastmod: 2023-03-23 
thumbnail: https://s1.vika.cn/space/2023/03/25/506e3155902c4b45a7054c9dd856f1c9?attname=notebook-1850613_960_720.jpg 
published: "true"
---

# [[Pandas]]读书笔记详情之四

## 1 表头（标题）  

```python 
#创建一个 style，设置 'caption' 元素的属性
s = df.style.set_table_styles([                            {'selector':'caption','props':'caption-side:bottom;text-align:center;font-size:20px'}
	],overwrite=False)
	
#针对上述 style 新建一个 caption
s.set_caption('this is a title') 
``` 

CSS 中 caption 元素的常用属性：  
1. caption-side：{'top'、'bottom'、'inherit}，默认 'top'
2. color：文本颜色
3. text-align：文本对齐方式，{'left', 'center', 'right'}
4. font-family：字体名称
5. font-size：字体大小，单位是 px
6. font-weight：字体粗细，{'lighter', 'normal', 'bold', 'bolder',100、200、300、400、500、600、700、800、900}
7. line-height：行间距
8. border：边框属性
9. width：文本宽度
10. margin、pad：边距  

## 2 列头  

```python
#整个列头区
col_headers = {
    'selector': 'th:not(.index_name):not(.row_heading)',
    'props': 'background-color: #000066; color: white;'
}

#列的有关 css 类
.col_heading
.levelk，k 表示多级索引的级数
.colm，m 表示列号
df.style.set_table_styles([
                            col_header,
                            {'selector': '.col_heading','props': 'background-color: #000066; color: white;'},

                            {'selector': '.col_heading.level1','props': 'background-color: #000066; color: white;'},

                            {'selector': '.col0','props': 'background-color: #000066; color: white;'}

],overwrite=False).set_table_styles({

                                ('Regression', 'Tumour'): [{'selector': 'th', 'props': 'border-left: 1px solid white'},
                                                           {'selector': 'td', 'props': 'border-left: 1px solid #000066'}]

              },overwrite=False)  #针对指定列设置边框等 css 属性
```
## 3 行头  

```python
#整个行头区
row_headers = {
    'selector': 'th:not(.index_name):not(.col_heading)',
    'props': 'background-color: #000066; color: white;'
}

#行的有关 css 类
.row_heading
.levelk，k 表示多级索引的级数
.rowm，m 表示列号
```

## 4 索引头  

```python
#整个索引头区
index_headers = {
    'selector': 'th:not(.row_name):not(.col_heading)',
    'props': 'background-color: #000066; color: white;'
}

#索引头类
.index_name
df.style.set_table_styles({'selector':'.index_name','props':'color:red'})
```

## 5 单元格  

单元格的显示设置，有两种方法，
>一是在 df.style.set_table_styles() 中，定义类并设置其 css 属性，然后在单元格中用 style.set_td_classes() 设置类名；

>二是用 style 的 format() 方法、set_properties()方法以及 apply、applymap、apply_index、applymap_index、pipe等函数  

### 5.1 set_table_styles 方法  

```python
Styler.set_table_styles(table_styles=None, axis=0, overwrite=True, css_class_names=None)
Styler.set_td_classes(classes)

#单元格相关的 css 类
.data
.colm，m 表示列号
.rowm

#示例
df = pd.DataFrame([[1,2],[3,4]], index=["a", "b"],
    columns=[["level0", "level0"], ["level1a", "level1b"]])

df
```
  
<img src="https://s1.vika.cn/space/2023/03/14/5ee997c780f14014aa448dd7845e85df" width=150px> 

```python
classes = pd.DataFrame(data='red',index=['a'],columns=[['level0'],['level1a']])
classes
```

<img src="https://s1.vika.cn/space/2023/03/14/dd079d4533e7476e90d05052e9a8e784" width=150px>
  

```python
s=df.style.set_table_styles([{'selector':'.red','props':'color:red'}])
s.set_td_classes(classes)
```  

使用 True 和 False 类。  

```python
s= df.style.set_table_styles([                               {'selector':'.True','props':'color:green'},                                {'selector':'.False','props':'color:red'}
])
s.set_td_classes(df>3)        #将 df 中大于 3 的单元格设置 class=True，小于 3 的单元格设置 class=Falase。上一条语句已经设置了 True 和 False 类的 css 属性，将据此显示 df
```

### 5.2 format 方法  

```python
Styler.format(formatter=None,
              subset=None,
              na_rep=None,
              precision=None,
              decimal='.',
              thousands=None,
              escape=None,
              hyperlinks=None)

返回：self.Styler
```

注释：  

>formatter：str, callable, dict or None。  
>>传入的参数是每个单元格的数据，返回值是需要显示在每个单元格的值。 
>>
>>如果传入的是字符串 str ，该字符串将通过 str.format(x)的方法显示。 
>>
>>下例表示所有单元格的数据显示为 2 位小数  
>>
>>`df.style.format(formatter='{:.2f}')`
下面的语句结果相同：
`df.style.format(formatter='{:.2f}'.format)`
>>
`df.style.format(formatter=lambda x:'{:.2f}'.format(x))`
>>
如果传入的是字典，keys 必须对应列名或列号，values 必须对应 str、函数  
>>
`df.style.format(formatter={0: '{:.2f}', 1: '£ {:.1f}'})`  

>subset：标签或标签列表。  
>>
对数据源的限制，相当于DataFrame.loc[]（二维）或 DataFrame.loc[:, ]（一维，列标签）  

### 5.3 set_properties()方法  

```python
Styler.set_properties(subset=None, **kwargs)
df.style.set_properties(**{'background-color': 'yellow','text-align:center'})
```

该方法对 subset 中的每个单元格，按 kwargs 的设置显示数据。  

### 5.4 apply、applymap、pipe函数  

```python
Styler.apply(func, axis=0, subset=None, **kwargs)
#下面语句，将每列中的最大值的字体设为红色
def highlight_max(x, color):
    return np.where(x == np.max(x), "color: {};".format(color), None)
df = pd.DataFrame(np.random.randn(5, 2), columns=["A", "B"])
df.style.apply(highlight_max, color='red')  
#用 set_table_styles() 和 set_td_classes()方法
df.style.set_table_styles([{'selector':'.True','props':'color:red'}]).set_td_classes(df==df.max())
```

注释：  

>func：输入参数 {'axis' = 0 or 'index'  : 列,默认值,  
>>
'axis' = 1 or 'column'  : 行,  
>>
'axis' =  None : DataFrame}  
>
返回值：css 字符串，属性名:属性值  

>kwargs：传递给 func 的参数  

>返回值：self：Styler  

该方法对 subset 中的指定列或行或整个DataFrame，按 func 返回的 css 属性值设置显示数据。  

```python
Styler.applymap(func, subset=None, **kwargs)
Styler.pipe(func, *args, **kwargs)
```

## 6 其他  

```python

Styler.hide(subset=None, axis=0, level=None, names=False)    #隐藏显示部分行、列

Styler.clear()
Styler.background_gradient(cmap='PuBu', low=0, high=0, axis=0, subset=None, text_color_threshold=0.408, vmin=None, vmax=None, gmap=None)
```

