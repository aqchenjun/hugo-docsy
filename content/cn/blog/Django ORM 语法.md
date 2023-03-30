---
categories: Django
title: Django ORM 语法
tags: [教程,Python]
date: 2023-03-30
lastmod: 2023-03-30 
slug: 12cs3u0
thumbnail: https://s1.vika.cn/space/2023/03/30/ec2d40b5e9f94a318609a8a6bd167f92
published: false
---

>《Python 程序语言·Django》读书笔记之六

## 1 update_or_create  

`obj,created = update_or_create(defaults=None, **kwargs)`  

A convenience method for updating an object with the given kwargs, creating a new one if necessary. The defaults is a dictionary of (field, value) pairs used to update the object. The values in defaults can be callables.  

Returns a tuple of (object, created), where object is the created or updated object and created is a boolean specifying whether a new object was created.  

The update_or_create method tries to fetch an object from database based on the given kwargs. If a match is found, it updates the fields passed in the defaults dictionary. 

参数：  

- defaults：字典类型，用来更新对象，  
- kwargs：字典类型，用来检索过滤  

返回值：  

- obj：更新或创建的对象  
- created：boolen类型，True表示创建成功，False表示更新成功

{{% alert title=注意 color='success' %}}不论是更新还是创建，obj 均反映 defaults中的数据。obj和created可以省略
  {{% /alert %}} 


 


