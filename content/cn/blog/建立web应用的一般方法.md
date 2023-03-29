---
categories: Django
title: Django 建立web应用的一般方法
tags: [教程,Python]
date: 2023-03-28
lastmod: 2023-03-28 
slug: 20230328093946
thumbnail:  
published: "true"
---

  

## 1 需要的主要工具  

1. python 3.8
2. Django 3.0.4
3. mysqlclient
4. mysql 8.0.28
5. VSCode
6. Bootstrap 4.0
7. jquery  

## 2 创建项目babys和应用commoditys  

进入工作目录。  

`django-admin startproject babys`  

进入babys目录。  

`py manage.py startapp commoditys`  

## 3 在项目 babys 目录下创建templates目录、static目录和media目录  

项目 **babys** 目录下的templates目录存放项目通用模板，如base.html。各应用app的模板存放在各应用目录下的templates目录下应用子目录下面。static目录存放静态资源，如css、js、图标等。media存放可以动态调整的媒体文件。  

## 4 建立 base.html  

在项目 **babys** 目录下的 templates 目录建立 base.html。  

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>{{ title }}</title>
    {% load static %}
    <link rel="stylesheet" href="{% static 'css/bootstrap.min.css' %}">
    <script src="{% static 'js/jquery.slim.min.js' %}"></script>
    <script src="{% static 'js/bootstrap.min.js' %}"></script>
</head>
<body>
    {% block content %}
    {% endblock %}
</body>
</html>
```
 
## 5 修改项目目录下的settings.py：  

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'commodity.apps.CommodityConfig', 这一行是新增的 
]
MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.locale.LocaleMiddleware',这一行是新增的，中文支持
    'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
]
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [os.path.join(BASE_DIR,'templates')],这一行是修改的
        'APP_DIRS': True,这一行是修改的
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql', 新增对mysql的支持
        'NAME': 'babys',
        'USERNAME':'chenqian',
        'PASSWORD':'123456',
        'HOST':'127.0.0.1',
        'PORT':'3306',
    }
}
STATIC_URL = '/static/'
下面三行是新增的
STATICFILES_DIRS = (os.path.join(BASE_DIR , 'static'),)
MEDIA_URL = '/media/'
MEDIA_ROOT = os.path.join(BASE_DIR, 'media')
```  

## 6 修改路由urls.py 

项目 babys 目录下的urls.py：  

```python
from django.contrib import admin
from django.urls import path,re_path,include
from . import views
from django.views.static import serve
from django.conf import settings

urlpatterns = [
    path('admin/', admin.site.urls),
    re_path('media/(?P<path>.*)',serve,{'document_root':settings.MEDIA_ROOT},name = 'media'),
    path('commodity/',include('commodity.urls')),  
]
```

修改commodity应用目录下的urls.py，如果没有这个文件，则创建：  

```python
from django.urls import path
from . import views

app_name = 'commodity'
urlpatterns = [
    path('',views.commodityViews, name = 'commodity'),
    path('details/<int:id>',views.commodityDetails, name = 'details'), 
    path('login/', views.loginView,name='login'),
]
```  

## 7 建立Models  

打开VSCode，在commoditys目录下，修改models.py，创建模型类：  

```python
class CommodityViews(models.Model):
    id = models.AutoField(primary_key=True)
    ...
```

## 8 数据迁移创建数据表  

### 8.1 建立数据库  

由于上述项目 babys 目录下的 [settings.py](http://settings.py) 中，设置'ENGINE': 'django.db.backends.mysql', 因此需要在 mysql 中建立数据库，名称与项目 babys 相同。  

### 8.2 数据迁移  

#### 8.2.1  生成数据库迁移文件  

```shell
#项目 babys 目录下
python manage.py makemigrations
```  

#### 8.2.2  迁移到数据库，完成建表、修改字段等操作  

```shell
python manage.py migrate
```  

## 9 建立Template  

在应用 commoditys 目录下建立templates\commoditys目录，在该目录下创建模板(commodity.html文件)，以显示数据。  

```html
{% extends 'base.html' %}

{% block title %}
commodity
{% endblock %}

{% block local_css %}
{% load static %}
<link rel="stylesheet" href="{% static 'font-awesome/css/font-awesome.css' %}">
<style>
    .red{
        background-color:red
    }
    .grey_white{
        color:#999
    }
    .black{
        color:black
    }
</style>
{% endblock %}

{% block content %}
<div class="container-fluid">
    <div class="row">
        <div class="col-2">
            <a href="{% url 'commodity:commodity' %}" class="btn btn-block">所有商品</a>
            <dl>
                {% for firsts in firsts_list %}
                <dt style="cursor:pointer"><i class="fa fa-plus-square-o"></i> {{ firsts.firsts }}</dt>
                {% for types in types_list %}
                    {% if types.firsts == firsts.firsts %}
                    <dd class="d-none" seconds="{{ types.seconds }}" style="cursor:pointer">{{ types.seconds }}</dd>
                    {% endif %}
                {% endfor %}
                {% endfor %}
            </dl>
        </div>
        <div class="col-10">
            <nav class="nav" id="sort_bar">
                <a class="nav-link" href="javascript:;" seconds="" sort="sold">销量<i class="fa fa-long-arrow-down"></i> </a>
                <a class="nav-link price" href="javascript:;" seconds="" sort="price" reverse="">
                    价格
                    <i class="fa fa-caret-down" style="position:relative;top:5px"></i>
                    <i class="fa fa-caret-up" style="position:relative;top:-5px;left:-13px"></i>
                </a>
                <a class="nav-link" href="javascript:;" seconds="" sort="created">新品<i class="fa fa-long-arrow-down"></i></a>
                <a class="nav-link" href="javascript:;" seconds="" sort="likes">收藏<i class="fa fa-long-arrow-down"></i></a>
            </nav>
            <div class="row"  id="content">
                {% for c in commodity_list_page  %}
                <div class="col-4">
                    <a href="{% url 'commodity:details' c.id %}"><img src="/media/{{ c.img }}" class="w-100"></a>
                    <div>{{ c.name }}</div>
                    <div class="d-flex justify-content-between">
                        <div>{{ c.price|floatformat:'1' }}</div>
                        <div>{{ c.sold }}人已付款</div>
                    </div>
                </div>
                {% endfor %}
            </div>
            <nav>
                <ul class="pagination">
                    {% if commodity_list_page.has_previous %}
                    <li class="page-item">
                        <a class="page-link" href="javascript:;" seconds="" page="{{ commodity_list_page.previous_page_number }}" sort="" reverse="">Previous</a>
                    </li>
                    {% else %}
                    <li class="page-item disabled">
                        <a class="page-link">Previous</a>
                    </li>
                    {% endif %}
                    {% for page in commodity_list_page.paginator.page_range %}
                        {% if page == commodity_list_page.number %}
                        <li class="page-item active">
                            <a class="page-link" href="javascript:;" seconds="" page="{{ page }}" sort="" reverse="">{{ page }}</a>
                        </li>
                        {% else %}
                        <li class="page-item">
                            <a class="page-link" href="javascript:;" seconds="" page="{{ page }}" sort="" reverse="">{{ page }}</a>
                        </li>
                        {% endif %}
                    {% endfor %}
                    {% if commodity_list_page.has_next %}
                    <li class="page-item">
                        <a class="page-link" href="javascript:;" seconds="" page="{{ commodity_list_page.next_page_number }}" sort="" reverse="">Next</a>
                    </li>
                    {% else %}
                    <li class="page-item disabled">
                        <a class="page-link">Next</a>
                    </li>
                    {% endif %}
                </ul>
            </nav>
        </div>
    </div>
</div>
{% endblock %}

{% block script %}
<script>
    $("dt").click(function(){
        if($(this).find('i').hasClass('fa-plus-square-o')){
            $(this).find('i').removeClass('fa-plus-square-o').addClass('fa-minus-square-o')
            $(this).nextUntil('dt').removeClass('d-none')
            $(this).siblings('dt').find('i').removeClass('fa-minus-square-o').addClass('fa-plus-square-o')
            $(this).siblings('dt').nextUntil('dt').addClass('d-none')
        }else{
            $(this).find('i').removeClass('fa-minus-square-o').addClass('fa-plus-square-o')
            $(this).nextUntil('dt').addClass('d-none')
        }
    })
    $(document).on('click','dd,.page-link,.nav-link',function(){
        var seconds = $(this).attr('seconds')
        $("#sort_bar").find('a').attr('seconds',seconds)
        var page = $(this).attr('page')
        var sort = $(this).attr('sort') || ''
        $("#sort_bar").find('a').each(function(){
            if($(this).attr('sort') == sort){
                $(this).addClass('red')
            }else{
                $(this).removeClass('red')
            }
        })
        if(sort=='price'){
            if($(this).hasClass('price')){
                if($(this).attr('reverse') == 1){
                    $(this).attr('reverse',0)                    $(this).find('i').first().removeClass('black').addClass('grey_white')
                    $(this).find('i').last().removeClass('grey_white').addClass('black')
                }else{
                    $(this).attr('reverse',1)                    $(this).find('i').first().removeClass('grey_white').addClass('black')$(this).find('i').last().removeClass('black').addClass('grey_white')
                }
            }
        }else{
            $(".price").attr('reverse','')            $(".price").find('i').removeClass('black').addClass('grey_white')
        }
        reverse = $(this).attr('reverse')
        data = {
            'seconds':seconds,
            'page':page,
            'sort':sort,
            'reverse':reverse,
        }
        url = "{% url 'commodity:commodity' %}"
        $.get(url,data,function(data){
            var cont=''
            $.each(data.commodity_list_page,function(i,item){
                cont += "<div class='col-4'><a href=/commodity/details/"+item.id+"><img src='/media/" + item.img + "' class='w-100'></a><div>"+item.name +"</div><div class='d-flex justify-content-between'><div>"+ item.price.toFixed(2) +"</div><div>"+item.sold+"人已付款</div></div></div>" 
            })
            $("#content").html(cont)
            paginator = ''
            if(data.has_previous){
                cur_page = data.number
                cur_page --
                paginator +="<li class='page-item'><a class='page-link' href='javascript:;' seconds='"+ seconds+"' page="+ cur_page +" sort='"+sort+"' reverse='"+reverse+"'>Previous</a></li>"
            }else{
                paginator +="<li class='page-item disabled'><a class='page-link'>Previous</a></li>"
            }
            for(i=1;i<data.num_pages+1;i++){
                if(i== data.number){
                    paginator += "<li class='page-item active'><a class='page-link' href='javascript:;' seconds='"+seconds+"' page="+i+" sort='"+sort+"' reverse='"+reverse+"'>"+i+"</a></li>"
                }else{
                    paginator += "<li class='page-item'><a class='page-link' href='javascript:;' seconds='"+seconds+"' page="+i+" sort='"+sort+"' reverse='"+reverse+"'>"+i+"</a></li>"
                }
            }
            if(data.has_next){
                cur_page = data.number
                cur_page ++
                paginator +="<li class='page-item'><a class='page-link' href='javascript:;' seconds='"+seconds+"' page="+ cur_page +" sort='"+sort+"' reverse='"+reverse+"'>Next</a></li>"
            }else{
                paginator +="<li class='page-item disabled'><a class='page-link'>Next</a></li>"
            }
            $(".pagination").html(paginator)            
        })
    })
</script>
{% endblock %}
```


## 10 在views.py中创建视图函数
  

```python
def commodityViews(request):
    firsts_list = Types.objects.values('firsts').distinct()
    types_list = Types.objects.all()
    page = request.GET.get('page',1)
    seconds = request.GET.get('seconds','')
    sort = request.GET.get('sort','')
    reverse = request.GET.get('reverse',1)
    commodity_list = CommodityInfos.objects.all()
    if seconds:
        commodity_list = commodity_list.filter(types=seconds)
    if sort:
        if int(reverse) == 1:
            commodity_list = commodity_list.order_by('-'+sort)
        else:
            commodity_list = commodity_list.order_by(sort)
    paginator = Paginator(commodity_list,3)
    try:
        commodity_list_page = paginator.get_page(page)
    except PageNotAnInteger:
        commodity_list_page = paginator.get_page(1)
    except EmptyPage:
        commodity_list_page = paginator.get_page(paginator.num_pages)
    context = {
        'firsts_list':firsts_list,
        'types_list':types_list,
        'commodity_list_page':commodity_list_page,
    }
    result = {        'commodity_list_page':list(commodity_list_page.object_list.values()),
        'has_previous':commodity_list_page.has_previous(),
        'has_next':commodity_list_page.has_next(),
        'number':commodity_list_page.number,
        'num_pages':paginator.num_pages,
    }
    if request.is_ajax():
        return JsonResponse(result,safe=False)
    return render(request,'commodity/commodity.html',context)
```

