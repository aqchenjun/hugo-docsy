---
categories: Matplotlib
title: Matplotlib绘图基础
tags: [ 教程, python]
date: 2023-03-13
lastmod: 2023-03-20 
thumbnail: https://s1.vika.cn/space/2023/03/25/4b9a767a423a432f98351273e4dff2be?attname=u%3D628989781%2C1120270390%26fm%3D253%26fmt%3Dauto%26app%3D138%26f%3DPNG.webp
published: "true"
slug: 6hziwee
---


## 1 导包  

```python
import matplotlib.pyplot as plt
```

## 2 建立数据  

X轴和Y轴的数据可以有函数关系，也可以没有。但必须是一一对应关系。  

## 3 绘图  

```python
import numpy as np
x = np.arange('2022-01-01','2022-01-22',dtype=np.datetime64).astype(str).tolist()
y = np.random.randint(1,100,size=x.size)#与X轴数据一一对应
#设置图片大小和清晰度
plt.figure(figsize=(20,8),dpi=80)
#设置图片标题
plt.title('示例',fontdict={'family':'SimHei','size':25}) 
#设置X轴
plt.xlabel('X轴',fontdict={'family':'SimSun','size':20})
xticks= range(0,len(x),3)#设置X轴上的刻度间距，也就是变量x（是一个字符型list）的位置列表，这些位置上的变量x的值显示在X轴上。datatime64数据不能设置刻度间距，所以转换为字符型。如果是数值型，就是本身。
xticks_font={'family':'SimSun','size':15,'rotation':20}#刻度值的字体属性
plt.xticks(xticks,labels=['日期%s' %i for i in x[::3]],**xticks_font)#在每个刻度值前加上“日期”，下面两种labels的赋值语句具有同样效果。
#labels=['日期{}'.format(i) for i in x[::3]]
#labels=[f"日期{i}" for i in x[::3]]  
#Y轴同样可以设置  
#绘图，设置线条属性，label为图例名称
plt.plot(x,y1,label="图一",color='blue',marker='o',markersize=12,linestyle='dashed',linewidth=2)
plt.plot(x,y2,label='234')  
#显示图例,prop为字体属性
legend = plt.legend(loc='upper center',prop={'family':'SimSun','size':20},title='图例')
legend.get_title().set_family('SimSun')#设置图例标题为中文  
#在图形上显示数据
for a,b in zip(x,y1):
    plt.text(a,b,b,fontsize=12) 
#保存
plt.savefig('example.png')
#显示
plt.show()
```


`


