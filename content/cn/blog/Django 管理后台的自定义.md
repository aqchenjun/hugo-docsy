---
categories: Django
title: Django 管理后台的自定义
tags: [教程,Python]
date: 2023-03-28
lastmod: 2023-03-28 
slug: 20230328112619
thumbnail:  
published: "true"
---


## 1 数据显示的自定义  

以 ItemInfos 模型为例，原来的admin.py：  

```python
from django.contrib import admin
from .models import ItemInfos
admin.site.register(ItemInfos)
```  

修改后的admin.py:  

```python
from django.contrib import admin
from .models import ItemInfos
import re

@admin.register(ItemInfos)
class ItemInfosAdmin(admin.ModelAdmin):
    list_display = ['part_A','contact','mobilephone','itemName','category','district']
   list_filter = ['category']
   list_display_links = ['itemName']
   list_editable = ['category']
   search_fields = ['part_A']
   autocomplete_fields = ['category']
   def mobilephone(self,obj):
       regPhone = '(\d{3})(\d{4})(\d{4})$'
       phonenum = re.match(regPhone,obj.phone)
       # 简单的显示手机号码
       # return '-'.join(phonenum.groups())
       # 复杂的显示，必须导入 format_html 模块
       return format_html('<span style="color:red">{}</span>-<span style="color:blue">{}</span>-<span style="color:black">{}</span>',phone_num.group(1),phone_num.group(2),phone_num.group(3))
    mobilephone.short_description = '手机号码'
```

注释：  

1、list_display 表示要显示的列，其中 'mobilephone'和 'district' 是自定义列。自定义列的定义有两种途径。  

一是如上所述，在 ItemInfosAdmin 类中增加同名列函数 mobilephone，参数obj表示当前行数据，如obj.phone即表示当前行数据中的 phone 字段。  

另外一种途径是在 models  ItemInfos 定义中增加同名列函数 'district' 。  

```python
class ItemInfos(models.Model):
    id = models.AutoField(primary_key=True)
    part_A = models.CharField('委托方(甲方)',max_length = 200,)
    contact = models.CharField('联系人',max_length = 20,)
    phone = models.CharField('联系电话',max_length = 50,)
    itemName = models.CharField('项目名称',max_length = 200,)
    category = models.ForeignKey(verbose_name = '报告类别',to='Categorys',on_delete = models.CASCADE)
    district_province = models.ForeignKey(to='ProvinceInfos',verbose_name='所在省',on_delete=models.CASCADE)
    district_city = models.ForeignKey(CityInfos,on_delete=models.CASCADE)
    district_county = models.ForeignKey(CountyInfos,on_delete=models.CASCADE)
    approval_department = models.CharField(verbose_name = '审批机关',max_length = 100)  

    def __str__(self):
        return self.itemName

    class Meta:
        verbose_name = '建设项目'
        verbose_name_plural = '建设项目'
        
    def district(self):
        return self.district_province.province + self.district_city.city +self.district_county.county
    district.short_description = '所在地区'
```  

2、autocomplete_fields 给下拉列表增加搜索框。  

## 2 管理界面部分占位符的自定义  

### 2.1 app 应用的显示名称  

修改app.py，在ItemInfos 类中增加 verbose_name 属性  

```python
class ItemInfos(AppConfig):
    name = 'items'
    verbose_name ='建设项目'
```  

### 2.2 修改标题  

在admin.py 中增加几个参数：  

```python
admin.site.site_title = 'project'
admin.site.site_header = '后台'
admin.site.index_title = 'gl'
```  

## 3 自定义ModelAdmin 的部分函数  

常用的自定义函数包括 get_readonly_fields()、get_queryset()、save_model()以及 参数 actions 等。