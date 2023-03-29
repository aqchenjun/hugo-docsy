---
categories: Django
title: Django 根据不同的登录用户显示不同的数据
tags: [教程,Python]
date: 2023-03-28
lastmod: 2023-03-29 
slug: 20230328113257
thumbnail:  
published: "true"
---

>《Python 程序语言：Django》读书笔记之3

## 1 注册  

### 1.1 在项目中新建app，userlogin  

`py manage.py startapp userlogin`  

### 1.2 在settings.py 里新增 应用 userlogin  

### 1.3 在userlogin目录里新建文件form.py，创建LoginModelForm类  

```python
from django import forms
from django.contrib.auth.models import User
from django.core.exceptions import ValidationError  

class LoginModelForm(forms.ModelForm):
    class Meta:
        model = User
        fields = {'username','password'}
        labels = {
            'username':'请输入手机号',
            'password':'请输入密码'
        }
        error_messages = {
            '__all__':{
                'required':'请输入内容',
                'invalid':'请检查输入的内容'
            }
        }
        widgets = {
            'username':forms.widgets.TextInput(
                attrs={
                    'class':'form-control',
                    'id':'username',
                    'placeholder':'请输入手机号',
                    'autocomplete':'off',
                }),
            'password':forms.widgets.PasswordInput(
                attrs={
                    'class':'form-control',
                    'id':'password',
                    'placeholder':'请输入密码'
                }
            )
        }
    def clean_username(self):
        if len(self.cleaned_data['username']) == 11:
            return self.cleaned_data['username']
        else:
            raise ValidationError('用户名为手机号')
```  

### 1.4 创建路由  

projects.urls.py:  

```python
path('login/',include('userlogin.urls'))
```  

userlogin.urls.py:  

```python
app_name = 'login'
urlpatterns = [
    path('', views.loginView,name='login'),
    path('register/',views.registerView,name='register'),
    path('logout/',views.logoutView,name='logout'),
]
```  

### 1.5 创建视图函数views.py  

```python
def registerView(request):
    if request.method=='POST':
        infos = LoginModelForm(data=request.POST)
        data = infos.data
        username = data['username']
        password = data['password']
        if User.objects.filter(username = username):
            state = '用户名已存在，请重新注册' 
        else:
            d = dict(username=username,password = password)
            user = User.objects.create_user(**d)
            user.save()
            return redirect(reverse('login:login'))
    infos = LoginModelForm()
    title = '用户手机号注册'
    submit_btn = '注册'
    tip = '已有用户名？请登录'
    return render(request,'userlogin/login.html',locals())
```  

### 1.6 创建login.html  

```python
{% extends 'base.html' %}
{% block content %}
<div class="container mt-3">
    <div class="row justify-content-center">
        <div class="h4 mb-5">{{ title }}</div>
    </div>
    <div class="row justify-content-center">
        <div class="col-3">
            <form method="post" action="#" novalidate>
                {% csrf_token %}
                <div class="form-row">
                    <div class="col">
                        <div class="input-group mb-3">
                            <div class="input-group-prepend">
                                <span class="input-group-text" id="basic-addon1">@</span>
                            </div>
                            {{ infos.username }}
                        </div>
                    </div>
                </div>
                <div class="form-row">
                    <div class="col">
                        <div class="input-group mb-3">
                            <div class="input-group-prepend">
                                <span class="input-group-text" id="basic-addon1">@</span>
                            </div>
                            {{ infos.password }}
                        </div>
                    </div>
                </div>
                <div>{{ state }}</div>
                <button type="submit" class="btn btn-primary" >{{ submit_btn }}</button>
            </form>
            <button class="btn btn-primary" onclick = login_or_register() id="tip">{{ tip }}</button>
        </div>
    </div>
</div>
{% endblock %}

{% block script %}
<script>
    function login_or_register(){
        if('{{ tip }}'=='没有用户名？请注册'){
            window.location.href="{% url 'login:register' %}"
        }else{
            window.location.href="{% url 'login:login' %}"
        }
    }
</script>
{% endblock %}
```
  

## 2 登录  

在视图函数views.py 新增loginView 路由：  

```python
def loginView(request):
    if request.method == 'POST':
        infos = LoginModelForm(data=request.POST)
        data = infos.data
        username = data['username']
        password = data['password']
        if User.objects.filter(username=username):
            user = authenticate(username = username,password = password)
            if user:
                login(request,user)
                return redirect(reverse('items:item'))
            else:
                state = '密码错误'
        else:
            state = '用户名不存在'
    infos = LoginModelForm()
    submitbtn = '登录'
    title = '手机号登录'
    tip = '没有用户名？请注册'
    return render(request,'userlogin/login.html',locals())
```  

## 3 显示合同签订人是登录用户的项目数据  

在项目显示路由中，添加装饰器，同时将用户id传递给页面前端，作为table的参数  

```python
@login_required(login_url = '/login/')
def itemView(request):
    categorys = Categorys.objects.all()
    provinces = ProvinceInfos.objects.all()
    user_id = request.user.id
    return render(request,'items/items.html',locals())
def table(request,user_id):
    contractInfos = ContractInfos.objects.filter(person_of_sign_id=user_id).values()
    item_id_list = []
    for contract in contractInfos:
        item_id_list.append(contract['item_id_id'])
    itemInfos = ItemInfos.objects.filter(id__in = item_id_list).values()
    for item in itemInfos:
        category = Categorys.objects.get(id=item['category_id'])
        province = ProvinceInfos.objects.get(id=item['district_province_id'])
        city = CityInfos.objects.get(id=item['district_city_id'])
        county = CountyInfos.objects.get(id=item['district_county_id'])
        item['category']=category.category
        item['province']=province.province
        item['city']=city.city
        item['county']=county.county
    total = itemInfos.count()
    rows = list(itemInfos)
    result = {
        'total':total,
        'rows':rows
    }
    return HttpResponse(json.dumps(result,cls=serializers.json.DjangoJSONEncoder))
```  

## 4 退出登录  

```python
def logoutView(request):
    logout(request)
    return redirect(reverse('items:item'))
```