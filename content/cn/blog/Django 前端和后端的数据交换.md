---
categories: Django
title: Django Django 前端和后端的数据交换
tags: [教程,Python]
date: 2023-03-30
lastmod: 2023-03-30 
slug: l9dlnr8
thumbnail:  
published: "true"
---

>《Python 程序语言·Django》读书笔记之

## 1 后端向前端页面传递数据，也就是views文件到HTML文件  

### 1.1 当需要加载整个页面时，利用render传递字典格式的数据  

#### 1.1.1 传递  

`return render(request,'items/items.html',context)`  

上述代码中，items/items.html是一个前端页面，context表示需要传递的数据，其格式为字典。例如： 

`context = {'item_id':item_id,'id',2}`  

或者： 

`return render(request,'items/items.html',locals())`  

locals()代表该视图函数中的所有变量，以字典形式，包括函数本身自带的变量。  

#### 1.1.2 前端页面的接收  

在前端页面中使用，要加上双大括号，例如：{{ item_id }}。如果用在模板标签中，则可以直接引用。  

### 1.2 当加载部分页面时，必须传递JSON格式的数据  

利用json模块中的dumps函数，将要传递的数据转换为json格式，如果数据里面有日期变量，dumps函数还要加上参数 cls = DjangoJSONEncoder  

```python
import json
return HttpResponse(json.dumps(results))
```  

或者：  

```python
from django.http import JsonResponse
return JsonResponse(results)
```  

如果results不是字典格式，还需要加上参数 safe = False  

### 1.3  实例  

#### 1.3.1  方法一：在views.py中，返回Json格式的字符串  

```python
def cartView(request):
    orderInfos = serializers.serialize('json',OrderInfos.objects.all())
    return render(request,'shopper/shopcart.html',locals())
```  

在前端html中，设置一个隐藏input标签，其id="jsonstring" ，value = {{ orderInfos }}  

在js中，  

```js
$(function(){
    var data=$("#jsonstring").val()
    var jsondata = eval("("+ data +")")
  })
``` 

var jsondata 即为json对象  

#### 1.3.2 方法二：在views.py中，返回Json格式的字符串  

```python
def cartView(request):
    orderInfos = serializers.serialize('json',OrderInfos.objects.all())
    List = ['自强学堂', '渲染Json到模板']
    Dict = {'site': '自强学堂', 'author': '涂伟忠'}
    return render(request,'shopper/shopcart.html',locals())
```  

在前端html js中，加上safe过滤器，与JsonResponse中的参数safe=False的作用相似  

```js
$(function(){
    var data = {{ orderInfos|safe }}
    var List = {{ List|safe }}
    var Dict = {{ Dict|safe }}
  })
``` 

var data List Dict即为json对象，data的结构： 

```js
[
{"model": "shopper.orderinfos", "pk": 1, "fields":
    {"price": 1213.0, "created": "2022-04-12", "user_id": 1, "state": "\u5f85\u652f\u4ed8"}
},
{"model": "shopper.orderinfos", "pk": 2, "fields":
    {"price": 34.0, "created": "2022-04-12", "user_id": 1, "state": "\u5df2\u652f\u4ed8"}
}
]
``` 

#### 1.3.3 方法三：在views.py中返回Json格式的字符串  

```python
def cartView(request):
    orderInfos = json.dumps(list(OrderInfos.objects.all().values()),cls=serializers.json.DjangoJSONEncoder)
    List = ['自强学堂', '渲染Json到模板']
    Dict = {'site': '自强学堂', 'author': '涂伟忠'}
    return render(request,'shopper/shopcart.html',locals())
```  

`cls=serializers.json.DjangoJSONEncoder`的作用是将QuerySet中的诸如datetime等转换为json。前端html js与上相同，只是orderInfos的结构更简洁  

```js
[
{"id": 1, "price": 1213.0, "created": "2022-04-12", "user_id": 1, "state": "\u5f85\u652f\u4ed8"},
{"id": 2, "price": 34.0, "created": "2022-04-12", "user_id": 1, "state": "\u5df2\u652f\u4ed8"}
]
```  

## 2 前端向后端传递数据，也就是HTML文件到views文件  

### 2.1 Form表单中设置action参数  

`<form action="{% url 'items:items' %}" method='post'></form>`  

该方法只能用post方法传递数据至前端，而且加载整个页面  

### 2.2 设置a标签  

`<a href="{% url 'items:items' x %}">连接</a>`  

上述代码中的 x 变量即是要传递给前端的数据，但只能以get方法，而且 x 变量来自后端渲染页面时传递过来的。  

### 2.3 ajax get方法  

#### 2.3.1 方法一  

`var url="{% url 'incomes:table' item_id %}"`  

上述代码中的 item_id 变量即是要传递给前端的数据，但只能以get方法，而且该变量来自后端渲染页面时传递过来的。 

#### 2.3.2 方法二  

`var url = url:"/incomes/table/"+ item_id.toString()`

上述代码中的 item_id 变量即是要传递给前端的数据，该变量来自当前页面。  

#### 2.3.3 方法三  

`window.location.href="/incomes/" + item_id`  

上述代码打开新页面，item_id 变量即是要传递给前端的数据，该变量来自当前页面。  

### 2.4 ajax post 方法  

通过 url模板标签指定path路由，需要传递的数据用data表达。  

```python
var url = "{% url 'contract:update_or_create' %}"
var csrf = $('input[name="csrfmiddlewaretoken"]').val()
    data = {
        'item_id' : $('#item_id').val(),
        'person_of_sign' :$('#person_of_sign').val(), 
        'date_of_sign' :$('#date_of_sign').val(),
        'contractAmount':$('#amount').val(),
        'pay':$('#pay').val(),
        'account':$('account').val(),
        'invoice':$("#invoice").val(),
        'csrfmiddlewaretoken':csrf,
    }
    $.post(url,data,function(data){
        console.log(data); 
    })
```  

如果数据不是字符串格式，例如数组，应转换为json格式  

`var data = {'idlist':JSON.stringify(idlist)}`  

Django后端接收数据：  

```python
id = request.POST.get('id')
 item_id = request.POST.get('item_id')
``` 

如果是json格式，还要转换；  

```python
import json
idlist = request.POST.get('idlist')
idlist = json.loads(idlist)
```  



{{% alert title=注意 color=success %}}
ajax 发送 post 请求时，解决 csrf 报错的方法：

{{% /alert %}}


1. 第一步：在body标签中添加：
   `{% csrf_token %}`
   
2. 第二步：js中定义此变量
   `var csrf = $('input[name="csrfmiddlewaretoken"]').val();`
   
3. 第三步：将此变量传递给后端，username和password是自己定义的可忽略
   `data:{"username": u,`
   `"password": p,`
   `'csrfmiddlewaretoken': csrf,}`