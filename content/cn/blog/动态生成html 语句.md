---
categories: Django
title: 动态生成html 语句
tags: [教程,Python,html]
date: 2023-03-31
lastmod: 2023-03-31 
slug: 8wc8dxx
thumbnail:  
published: "true"
---

>《Python 程序语言·Django》读书笔记之十三

1. 动态生成html时，字符串中的特殊字符要转义，即字符前加上‘\’，特殊字符包括单引号、双引号、大括号等。现在来看，这句话值得商榷。
2. 动态生成html时，前端页面中的图片src地址，要在后端传来的地址前面加上'/media/'。前提是首先配置好媒体资源。假设变量：address表示后端传来的地址  

```html
"<img src='/media/'"+ address+">"
```  

如address="imgs/p1.jpg"，那html字符串就是："<img src='/media/imgs/p1.jpg>"  

3. 当用字符串拼接动态生成网址时，如果路由是: 

`path('details/<int:id>',views.commodityDetails, name = 'details')`  

网址字符串cont是

`cont = "<a href=/commodity/details/"+item.id+">"` 

如果路由是 

`path('details/',views.commodityDetails, name = 'details')` 

网址字符串cont是 

`cont = "<a href={% url 'commodity:details' %}?id="+item.id+">"`