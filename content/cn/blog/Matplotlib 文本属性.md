---
categories: Matplotlib
title: Matplotlib 文本属性
tags: [ 教程, python]
date: 2023-03-13
lastmod: 2023-03-18
thumbnail: https://s1.vika.cn/space/2023/03/25/bb106251dae44a2b877d3c07b9cef272?attname=v2-f77095068b00b4b2ff2c5c1381d0582a_1440w.jpg 
published: "true"
slug: 20230313170323
---


```python
plt.text(x=0, y=0, text='', *, color=None, verticalalignment='baseline', horizontalalignment='left', multialignment=None, fontproperties=None, rotation=None, linespacing=None, rotation_mode=None, usetex=None, wrap=False, transform_rotates_text=False, parse_math=None, **kwargs)

#ax.text(x=0, y=0, text='', *, color=None, verticalalignment='baseline', horizontalalignment='left', multialignment=None, fontproperties=None, rotation=None, linespacing=None, rotation_mode=None, usetex=None, wrap=False, transform_rotates_text=False, parse_math=None, **kwargs)
```

- x，y：位置
- alpha：透明度
- backgroundcolor：背景色
- color：字体颜色
- fontfamily：字体名称
- font_properties：字体属性
- fontsize：字号
- fontstyle：'normal', 'italic', 'oblique'
- fontweight：{a numeric value in range 0-1000, 'ultralight', 'light', 'normal', 'regular', 'book', 'medium', 'roman', 'semibold', 'demibold', 'demi', 'bold', 'heavy', 'extra bold', 'black'}
- horizontalalignment( ha )：{'left', 'center', 'right'}
- verticalalignment（va）：{'bottom', 'baseline', 'center', 'center_baseline', 'top'}
- linespacing=1.2：行间距
- rotation：旋转度数，float or {'vertical', 'horizontal'}  
