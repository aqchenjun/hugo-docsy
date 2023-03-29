---
categories: Python 程序语言
title: turtle
tags: [ 教程, python]
date: 2023-03-11
lastmod: 2023-03-19 
thumbnail: https://s1.vika.cn/space/2023/03/25/1b48b2d3d1a74449a082589f406721f2?attname=turtle-2201433__340.jpg
published: "true"
slug: 20230311105822
---

# 《Python 程序语言》读书笔记之二
 

## 1 窗口及屏幕控制  

### 1.1  画布  

`turtle.**screensize**(*canvwidth=None*, *canvheight=None*, *bg=None*)`  

参数：  
>canvwidth -- 正整型数，以像素表示画布的新宽度值
>canvheight -- 正整型数，以像素表示画面的新高度值
>bg -- 颜色字符串或颜色元组，新的背景颜色
  
### 1.2  窗口  

#### 1.2.1 设置  

`turtle.setup(width=_CFG["width"], height=_CFG["height"], startx=_CFG["leftright"], tarty=_CFG["topbottom"]*)`

  
参数：  
>width:
>> 如为一个整型数值，表示大小为多少像素，如为一个浮点数值，则表示屏幕的占比；默认为屏幕的 50%
>
>height :
>> 如为一个整型数值，表示高度为多少像素，如为一个浮点数值，则表示屏幕的占比；默认为屏幕的 75%
>
>startx:
>> 如为正值，表示初始位置距离屏幕左边缘多少像素，负值表示距离右边缘，`None` 表示窗口水平居中
>
>starty
>>如为正值，表示初始位置距离屏幕上边缘多少像素，负值表示距离下边缘，`None` 表示窗口垂直居中。

### 1.3  关闭  

`turtle.**bye**()`  关闭海龟绘图窗口。  

`turtle.**exitonclick**()` 将 `bye()` 方法绑定到 Screen 上的鼠标点击事件。也就是鼠标点击后，关闭窗口。
  
## 2  标题栏  

`turtle.**title**(*titlestring*)`  

参数：  
>titlestring： 一个字符串，显示为海龟绘图窗口的标题栏文本。

## 3 画笔  

### 3.1  画笔的属性  

`turtle.pen(pen=None, pendict)`
 
参数：

>pen -- 一个包含部分或全部下列键的字典
>pendict -- 一个或多个以下列键为关键字的关键字参数

  

返回或设置画笔的属性，以一个包含以下键值对的 "画笔字典" 表示: 

- "shown": True/False
- "pendown": True/False
- "pencolor": 颜色字符串或颜色元组
- "fillcolor": 颜色字符串或颜色元组
- "pensize": 正数值
- "speed": 0..10 范围内的数值
- "resizemode": "auto" 或 "user" 或 "noresize"
- "stretchfactor": (正数值, 正数值)
- "outline": 正数值
- "tilt": 数值
  

实例：  

`turtle.pen(fillcolor="black", pencolor="red", pensize=10)`
  
画笔颜色和填充颜色的名称及代码可以参考 [[“HTML 颜色名称及代码”]]

  

### 3.2  画笔的宽度  

`turtle.pensize()`  

参数：  

>width -- 一个正数值  

### 3.3  画笔颜色  

`turtle.pencolor()`  

没有参数传入,返回当前画笔颜色,传入参数设置画笔颜色,可以是字符串如"green", "red",也可以是RGB 3元组,


### 3.4  画笔移动速度  

`turtle.speed(speed)`  

设置画笔移动速度，画笔绘制的速度范围[0,10]整数，数字越大越快。
  
## 4  绘图命令  

### 4.1  画笔运动命令  
| 命令                      | 说明                                              |
| ---slug: 20230311105822
------slug: 20230311105822
------slug: 20230311105822
------slug: 20230311105822
---- | ---slug: 20230311105822
------slug: 20230311105822
------slug: 20230311105822
------slug: 20230311105822
------slug: 20230311105822
------slug: 20230311105822
------slug: 20230311105822
------slug: 20230311105822
---- |
| turtle.forward(distance)  | 向当前画笔方向移动distance像素长                  |
| turtle.backward(distance) | 向当前画笔相反方向移动distance像素长度            |
| turtle.right(degree)      | 顺时针移动degree°                                 |
| turtle.left(degree)       | 逆时针移动degree°                                 |
| turtle.pendown()          | 移动时绘制图形,缺省时也为绘制                     |
| turtle.goto(x,y)          | 将画笔移动到坐标为x,y的位置                       |
| turtle.penup()            | 移动时不绘制图形,提起笔，用于另起一个地方绘制时用 |
| turtle.circle()           | 画圆,半径为正(负),表示圆心在画笔的左边(右边)画圆                                                   |


### 4.2  画笔控制命令

| 命令                          | 说明                                      |
| ---slug: 20230311105822
------slug: 20230311105822
------slug: 20230311105822
------slug: 20230311105822
-------- | slug: 20230311105822
------slug: 20230311105822
------slug: 20230311105822
------slug: 20230311105822
------slug: 20230311105822
------slug: 20230311105822
------slug: 20230311105822
----- |
| turtle.pensize(width)         | 绘制图形时的宽度                          |
| turtle.pencolor()             | 画笔颜色                                  |
| turtle.fillcolor(colorstring) | 绘制图形的填充颜色                        |
| turtle.color(color1, color2)  | 同时设置pencolor=color1, fillcolor=color2 |
| turtle.filling()              | 返回当前是否在填充状态                    |
| turtle.begin_fill()           | 准备开始填充图形                          |
| turtle.end_fill()             | 填充完成                                  |
| turtle.hideturtle()           | 隐藏箭头显示                              |
| turtle.showturtle()           | 与hideturtle()函数对应                                          |
 

### 4.3  全局控制命令
| 命令                                                       | 说明                                           |
| ---slug: 20230311105822
------slug: 20230311105822
------slug: 20230311105822
------slug: 20230311105822
------slug: 20230311105822
------slug: 20230311105822
------slug: 20230311105822
------slug: 20230311105822
------slug: 20230311105822
------- | slug: 20230311105822
------slug: 20230311105822
------slug: 20230311105822
------slug: 20230311105822
------slug: 20230311105822
------slug: 20230311105822
------slug: 20230311105822
------slug: 20230311105822
---- |
| turtle.clear()                                             | 清空turtle窗口，但是turtle的位置和状态不会改变 |
| turtle.reset()                                             | 清空窗口，重置turtle状态为起始状态             |
| turtle.undo()                                              | 撤销上一个turtle动作                           |
| turtle.isvisible()                                         | 返回当前turtle是否可见                         |
| stamp()                                                    | 复制当前图形                                   |
| turtle.write(s[,font=("font-name",font_size,"font_type")]) | 写文本，s为文本内容，font是字体的参数，里面分别为字体名称，大小和类型；font为可选项, font的参数也是可选项                                                |


## 5  circle 命令  

`turtle.circle(radius, extent=None, steps=None)`  

绘制一个 `radius` 指定半径的圆。圆心在海龟左边 `radius` 个单位；`extent` 为一个夹角，用来决定绘制圆的一部分。如未指定 `extent`，则绘制整个圆。如果  `extent` 不是完整圆周，则以当前画笔位置为一个端点绘制圆弧。如果 `radius` 为正值则朝逆时针方向绘制圆弧，否则朝顺时针方向。最终海龟的朝向会依据 `extent` 的值而改变。  

圆实际是以其内切正多边形来近似表示的，其边的数量由 `steps` 指定。如果未指定边数则会自动确定。此方法也可用来绘制正多边形，多边形边数为`steps`