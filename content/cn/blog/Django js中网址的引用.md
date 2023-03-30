---
categories: Django
title: Django js中网址的引用
tags: [教程,Python]
date: 2023-03-30
lastmod: 2023-03-30 
slug: 1j36jn6
thumbnail:  
published: "true"
---

>《Python 程序语言·Django》读书笔记之八

## 1 ajax中的网址  

### 1.1 不需传递参数  

path路由是：  

```python
app_name = 'contract'
path('contract/', views.contractViews,name='contract'),
```  

HTML javascript 中的ajax url值：  

```js
var url = "{% url 'contract:contract' %}"
```  

### 1.2 需要传递参数

path路由：  

```python
app_name = 'contract'
path('contract/<int:item_id>/', views.contractViews,name='contract'),
```  

HTML javascript 中的ajax url值：  

```js
var url = '/contract/' + item_id.toString()
```  

## 2 打开新的页面  

```js

//无需传递参数
window.location.href= {% url 'contract:contract' %}

//需要传递参数
window.location.href= '/contract/' + item_id.toString()
```()
```