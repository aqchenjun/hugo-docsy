---
categories: Django
title: Django ForeignKey字段的新增和更新以及级联菜单的实现
tags: [教程,Python]
date: 2023-03-30
lastmod: 2023-03-30 
slug: yzi38kx
thumbnail:  
published: "true"
---

>《Python 程序语言·Django》读书笔记之

## 1 模型字段models.py 

```python
from django.db import models
class Categorys(models.Model):
    id = models.AutoField(primary_key = True)
    category = models.CharField('类别',max_length = 50,)  

class ProvinceInfos(models.Model):
    id = models.AutoField(primary_key = True)
    province = models.CharField(max_length = 100)  

class CityInfos(models.Model):
    id = models.AutoField(primary_key = True)
    province = models.ForeignKey(ProvinceInfos,on_delete=models.CASCADE)
    city = models.CharField(max_length = 100)  

class CountyInfos(models.Model):
    id = models.AutoField(primary_key = True)
    city = models.ForeignKey(CityInfos,on_delete=models.CASCADE)
    county = models.CharField(max_length = 100)  

class ItemInfos(models.Model):
    id = models.AutoField(primary_key=True)
    part_A = models.CharField('委托方(甲方)',max_length = 200,)
    category = models.ForeignKey(Categorys,on_delete = models.CASCADE)
    province = models.ForeignKey(ProvinceInfos,on_delete=models.CASCADE)
    city = models.ForeignKey(CityInfos,on_delete=models.CASCADE)
    county = models.ForeignKey(CountyInfos,on_delete=models.CASCADE)
```  

## 2 Html  

```html
<div class="btn-group" id="toolbar">
    <button type="button" class="btn btn-secondary" id="addbtn">新增</button>
    <button type="button" class="btn btn-secondary" id="updatebtn">修改</button>
</div>
<div class="modal fade" data-backdrop="static" data-keyboard="false" tabindex="-1">
    <div class="modal-dialog">
        <div class="modal-content">
            <div class="modal-header">
                <h5 class="modal-title"></h5>
                <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                    <span aria-hidden="true">&times;</span>
                </button>
            </div>
            <div class="modal-body">
                <form>
                    {% csrf_token %}
                    <div class="form-group d-none">
                        <label for="id" class="col-form-label">ID</label>
                        <div class="col-10">
                            <input type="text" class="form-control" id="ID">
                        </div>
                    </div>
                    <div class="form-group row">
                        <label for="part_A" class="col-form-label">委托方</label>

                        <div class="col-10">

                            <input type="text" class="form-control" id="part_A">
                        </div>
                    </div>
                    <div class="form-group row">
                        <label for="category" class="col-form-label">报告类别</label>
                        <div class="col-10">
                            <select id="category" class="form-control">
                            </select>
                        </div>
                    </div>
                    <div class="form-group">
                        <label>所在地区</label>
                        <div class="form-row">
                            <div class="col">
                                <select id="province" class="form-control">
                                    <option selected class="d-none">所在省</option>
                                </select>
                            </div>
                            <div class="col">
                                <select id="city" class="form-control">
                                    <option selected class="d-none">所在市</option>
                                </select>
                            </div>
                            <div class="col">
                                <select id="county" class="form-control">
                                    <option selected class="d-none">所在县区</option>
                                </select>
                              </div>
                        </div>
                    </div>
                </form>
            </div>
            <div class="modal-footer">
                <button type="button" class="btn btn-primary" id="submit">提交</button>
            </div>
        </div>
    </div>
</div>
<!-- 模态框结束 -->
```  

## 3 js语句  

### 3.1 类别select框的初始化函数，当modal初始化时，调用该函数，其中的函数参数id，表示选定项，当新增数据时，id=0  

```js
function category(id){
    var url="{% url 'items:category' %}";
    $.get(url,function(data){
        var select_option = '<option selected class="d-none">报告类别</option>'
        $.each(data,function(index,row){
            if(id==row.id){
                select_option += '<option value="'+ row.id +'" selected>'+ row.category +'</option>'
            }else{
                select_option += '<option value="'+ row.id +'">'+ row.category +'</option>'
            }
        })
        $("#category").html(select_option)
    })
}
```  

### 3.2 所在省select框的初始化函数，当modal初始化时，调用该函数，其中的函数参数id，表示选定项，当新增数据时，id=0  

```js
function provinces_list(id){
    var url="{% url 'items:province' %}";
    $.get(url,function(data){
        var select_option = '<option selected class="d-none">所在省</option>'
        $.each(data,function(index,row){
            if(id==row.id){
                    select_option += '<option value="'+ row.id +'" selected>'+ row.province +'</option>'
            }else{
                select_option += '<option value="'+ row.id +'">'+ row.province +'</option>'
            }
        })
        $("#province").html(select_option)
    })
}
```  

### 3.3 所在市select框的初始化函数
该函数返回选定省份的市列表，只有在所在省select框的值发生变化时，该函数才会被调用。其中参数pid表示所在省选定值，cid表示所在市选定值。  

```js
function citys_list(pid,cid){
    var url="{% url 'items:citys' %}";
    var data= {
        'province':pid
    }
    $.get(url,data,function(data){
        var select_option = "<option selected class='d-none'>所在市</option>"
        $.each(data,function(index,cityInfos){
            if(cid==cityInfos.id){
                select_option += "<option value="+ cityInfos.id +" selected>"+cityInfos.city+"</option>"
            }else{
                select_option += "<option value="+ cityInfos.id +">"+cityInfos.city+"</option>"
            }
        })
        $("#city").html(select_option)
    })
}
```  

### 3.4 所在县select框的初始化函数
该函数返回选定市的县列表，只有在所在市select框的值发生变化时，该函数才会被调用。其中参数cityid表示所在市选定值，cid表示所在县选定值。  

```js
function county_list(cityid,id){
    var url="{% url 'items:countys' %}";
    var data= {
        'city':cityid
    }
    $.get(url,data,function(data){
        var select_option = "<option selected class='d-none'>所在县区</option>"
        $.each(data,function(index,countyInfos){
            if(id==countyInfos.id){
                select_option += "<option value="+ countyInfos.id +" selected>"+countyInfos.county+"</option>"
            }else{
                select_option += "<option value="+ countyInfos.id +">"+countyInfos.county+"</option>"
            }
        })
        $("#county").html(select_option)
    })
}
``` 

### 3.5 模态框的初始化函数
其中参数row，当新增数据时，row=0，当更新数据时，row=选定行  

```js
function modal_init(row){
    $(".modal").modal('show')
    $("#ID").val(row.id)
    $("#part_A").val(row.part_A)
    $("#contact").val(row.contact)
    $("#phone").val(row.phone)
    $("#itemName").val(row.itemName)
    $("#department").val(row.approval_department)
    category(row.category_id)
    provinces_list(row.district_province_id)
    citys_list(row.district_province_id,row.district_city_id)
    county_list(row.district_city_id,row.district_county_id)
}
```  

### 3.6 新增按钮和更新按钮的点击事件  

```js
$("#addbtn").click(function(){
    modal_init(0)
    $(".modal-title").html('新增数据')
})
$("#updatebtn").click(function(){
    row = $("#table").bootstrapTable('getSelections')[0]
    if(row){
        modal_init(row)
        $(".modal-title").html('更新数据')
    }
})
```  

### 3.7 所在省select框的change事件
返回选定省的市列表，同时将所在县select框清空  

```js
$("#province").change(function(){
    citys_list($("#province").val(),0)
    $("#county").html("<option selected class='d-none'>所在县区</option>")
})
``` 

### 3.8 所在市select框的change事件
返回选定市的县列表  

```js
$("#city").change(function(){
    county_list($("#city").val(),0)
})
``` 

### 3.9 提交按钮事件  

```js
$("#submit").click(function(){
    var url = "{% url 'items:update_or_create' %}";
    var data = {
        'id':$("#ID").val(),
        'part_A':$("#part_A").val(),
        'contact':$("#contact").val(),
        'phone':$("#phone").val(),
        'itemName':$("#itemName").val(),
        'category':$("#category").val(),
        'province':$("#province").val(),
        'city':$("#city").val(),
        'county':$("#county").val(),
        'department':$("#department").val(),
        'csrfmiddlewaretoken':'{{ csrf_token }}'
    }
    $.post(url,data,function(data){
        $(".modal").modal('hide')
        $("#table").bootstrapTable('refresh')
    })
})
```  

## 4 urls.py  

```python
path('citys/',views.citys,name='citys'),
path('countys/',views.countys,name='countys'),
path('update_or_create/',views.update_or_create,name='update_or_create'),
path('category/',views.category,name='category'),
path('province/',views.province,name = 'province'),
```  

## 5 views.py  

由于ForeignKey字段category的值是模型Categorys的instance，因此当更新或创建cagegory时，应获得实例，即
`category = Categorys.objects.get(id=int(category_id))`  

```python
def category(request):
    categorys = Categorys.objects.values()
    return JsonResponse(list(categorys),safe=False)
def province(request):
    provinces = ProvinceInfos.objects.values()
    return JsonResponse(list(provinces),safe=False)  

def citys(request):
    province_id = request.GET.get('province')
    citys = CityInfos.objects.filter(province__id=province_id).values()
    return JsonResponse(list(citys),safe=False)  

def countys(request):
    city_id = request.GET.get('city')
    countys = CountyInfos.objects.filter(city__id=city_id).values()
    return JsonResponse(list(countys),safe=False)  

def update_or_create(request):
    id = request.POST.get('id')
    part_A = request.POST.get('part_A','')
    contact = request.POST.get('contact','')
    phone = request.POST.get('phone','')
    itemName = request.POST.get('itemName','')
    category_id = request.POST.get('category')
    province_id = request.POST.get('province')
    city_id = request.POST.get('city')
    county_id = request.POST.get('county')
    department = request.POST.get('department','')
    if category_id:
        category = Categorys.objects.get(id=int(category_id))
    else:
        category = Categorys()
    if province_id:
        province = ProvinceInfos.objects.get(id=int(province_id))
    else:
        province = ProvinceInfos()
    if city_id:
        city = CityInfos.objects.get(id=int(city_id))
    else:
        city = CityInfos()
    if county_id:
        county = CountyInfos.objects.get(id=int(county_id))
    else:
        county = CountyInfos()
    row = {
        'part_A':part_A,
        'contact':contact,
        'phone':phone,
        'itemName':itemName,
        'category':category,
        'district_province':province,
        'district_city':city,
        'district_county':county,
        'approval_department':department
    }
    if id:
        ItemInfos.objects.filter(id=id).update(**row)
        return HttpResponse('更新成功')
    else:
        ItemInfos.objects.create(**row)
        return HttpResponse('创建成功')
```