---
categories: Django
title: Django BootstrapTable
tags: [教程,Python]
date: 2023-03-28
lastmod: 2023-04-11
slug: b6axasu
thumbnail:  
published: "true"
---

>《Python 程序语言·Django 》读书笔记之五

## 1 关于数据记录的选择  

### 1.1 利用Django+bootstrapTable实现如下功能: 

1. 表格数据来自两个模型
2. 表格中数据的增减，既可以通过加、减号，也可以直接编辑单元格。
3. 当选中数据时，显示选中数据的条数、部分数据的和值。
4. 增加全选按钮，单击可以选定或者不选定所有数据，而不是当前页的全部数据，而且这个按钮与表格中的checkbox联动。  

### 1.2 实现思路  

#### 1.2.1 表格数据来自两个模型  

需要手动构建json数据结构，即下面代码中的for循环，其中row即组合了两个模型中的数据，以commodityInfos_id为外键  

```python
def table(request):
    cartInfos = CartInfos.objects.all()
    total = cartInfos.count()
    rows = []
    for cart in cartInfos:
        commodity = CommodityInfos.objects.get(id=cart.commodityInfos_id)
        row = {
            'id':cart.id,
            'quantity':cart.quantity,
            'user_id':cart.user_id,
            'commodity_img':commodity.img.url,
            'commodity_name':commodity.name,
            'commodity_unitprice':commodity.price
        }
        rows.append(row)
    results = {
        'total':total,
        'rows':rows
    }
    return HttpResponse(json.dumps(results,cls=serializers.json.DjangoJSONEncoder))
```  

#### 1.2.2 表格中数据的增减  

##### 1.2.2.1 设置加、减号  

在需要增加的数据列定义中，增加formatter:参数：在数据框的前后增加 “-”和“+”，同时定义onclick事件，minus(id,value)和plus(id,value),更新表格用updateCellByUniqueId，在此之前要设置uniqueId。另外，还可以将加号和减号单独作两列，然后利用onClickCell事件。  

```javascript
field:'quantity',
title:'数量',
width:200,
formatter:function(value,row){
    return '<div class="d-flex" style="width:130px"><button class="btn" onclick="minus('+ row.id +','+ value +')">-</button><input type="text" class="form-control"  value='+ value +'><div class="toast hide" data-autohide="false"><div class="toast-header p-0 "><input type="text" class="form-control" value='+ value+' style="position:relative;left:0px"></div></div><button class="btn" onclick="plus('+ row.id +','+ value +')">+</button></div>'
}
```  

```js
function minus(id,value){
    var quantity = value
    if(quantity>1){
        quantity --
    }
    $('#table').bootstrapTable('updateCellByUniqueId', {
        id: id,
        field: 'quantity',
        value: quantity
    })
}
function plus(id,value){
    var quantity = value
    quantity ++
    $('#table').bootstrapTable('updateCellByUniqueId', {
        id: id,
        field: 'quantity',
        value: quantity
    })
}
```  

##### 1.2.2.2 直接编辑单元格  

单击单元格输入框后，激活toast，同时隐藏该单元格输入框。输入时(keyup()事件)，验证数据，完成后blur()事件，更新数据。定义onClickCell事件：  

```js
onClickCell:function(field,value,row,$element){
    if(field='quantity'){
        $element.find('.toast').toast('show')
        $element.find('input').first().addClass('d-none')
        $element.find('input').last().focus()
        $element.find('input').last().keyup(function(){
            if(this.value.length==1){
                this.value=this.value.replace(/[^1-9]/g,'')
            }else{
                this.value=this.value.replace(/\D/g,'')
            }
        })
        $element.find('input').last().blur(function(){
            $('#table').bootstrapTable('updateCellByUniqueId', {
                id: row.id,
                field: field,
                value: parseInt(this.value)
            })
            quantity_and_totalprice()
            $element.find('.toast').toast('hide')
            $element.find('input').first().removeClass('d-none')
        })
    }
}
```  

#### 1.2.3 显示选中数据的条数、部分数据的和值  

定义onCheck、onUncheck等事件，定义处理函数：  

```js
onCheck:function(row,$element){
    quantity_and_totalprice()
},
onUncheck:function(row,$element){
    quantity_and_totalprice()
},
onCheckAll:function(rowsAfter){
    quantity_and_totalprice()
},
onUncheckAll:function(rowsAfter){
    quantity_and_totalprice()
},
```
  

```js
function quantity_and_totalprice(){
    var rows = $("#table").bootstrapTable('getSelections')
    var totalprice = 0
    $("#quantity").html(rows.length)
    $.each(rows,function(index,row){
        totalprice += row.commodity_unitprice * row.quantity
    })
    $("#totalprice").html(totalprice.toFixed(2))
}
```  

#### 1.2.4 增加全选按钮  

增加该按钮点击事件，同时在上面的处理函数中增加该按钮状态的设置。  

```js
$("#checkAll").click(function(){
    if($(this).prop('checked')==true){
        for(i=0;total-1;i++){
            $("#table").bootstrapTable('check',i)
        }
    }else{
        for(i=0;total-1;i++){
            $("#table").bootstrapTable('uncheck',i)
        }
    }
})
function quantity_and_totalprice(){
    var rows = $("#table").bootstrapTable('getSelections')
    var totalprice = 0
    $("#quantity").html(rows.length)
    // 下面是要增加的
    if(rows.length==total){
        $("#checkAll").prop('checked',true)
    }else{
        $("#checkAll").prop('checked',false)
    }

    // 上面是要增加的
    $.each(rows,function(index,row){
        totalprice += row.commodity_unitprice * row.quantity
    })
    $("#totalprice").html(totalprice.toFixed(2))
}
```  

## 2 后端分页

bootstrapTable 的参数设置：sidePagination='server`  

此时前端发送请求参数形式是，?sort=id&order=desc&offset=0&limit=5，sort等于bootstrapTable配置中的sortName,order等于bootstrapTable配置中的sortOrder，limit等于bootstrapTable配置中的pageSize，offset等于bootstrapTable配置中的(pageNumber-1)*pageSize。  

由于前端传来了sort、order、offset、limit参数，因此在 def table中要进行处理，得到的数据返回前端给bootstrapTable。  

views.py：  

```python
def table(request):
    offset = int(request.GET.get('offset'))
    limit = int(request.GET.get('limit'))
    sort = request.GET.get('sort','')
    order = request.GET.get('order','')
    orderInfos = OrderInfos.objects.all()
    total = orderInfos.count()
    if sort:
         if order == 'desc':
         orderInfos = orderInfos.order_by(sort)
    else:
         orderInfos = orderInfos.order_by('-'+sort)
    rows = list(orderInfos[offset:offset+limit].values())
    rows = list(orderInfos.values())
    result = {
        'total':total,
        'rows':rows
    }
    return HttpResponse(json.dumps(result,cls=serializers.json.DjangoJSONEncoder))
```  

## 3 bootstrapTable 中的checkbox的选定状态的处理  

设置全局变量 var selectedId =[]。  

修改或新增bootstrapTable中的onCheck事件和onUncheck事件，选中时，在electedId中添加 选中行的id，取消选中时，在selectedId中删除选中行的id  

```js
onCheck:function(row){
    selectedId.push(row.id)
},
onUncheck:function(row){
    selectedId.splice(indexOf(row.id),1)
},
```  

在bootstrapTable 中的columns参数 checkbox 列中，增加formatter：  

```js
formatter:function(value,row){
     if(selectedId.includes(row.id)){
         return true
     } 
},
```  

或者在bootstrapTable 初始化中，增加responseHandler，其中rows.selectStatus 的selectStatus是checkbox自定义field  

```js
responseHandler:function(res){
    $.each(res.rows,function(index,rows){
        if(selectedId.includes(rows.id)){
            rows.selectStatus=true
        }
    })
    return res
},
```  

## 4 实现数据的增改删查  

### 4.1 数据的浏览  

html和js:（shopper/shopcart.html）  

```html
<link rel="stylesheet" href="{% static 'bootstrap-table/dist/bootstrap-table.css' %}">
<link rel="stylesheet" href="{% static 'font-awesome/css/font-awesome.css' %}">
<script src="{% static 'bootstrap-table/dist/bootstrap-table.js' %}"></script>
<script src="{% static 'bootstrap-table/dist/locale/bootstrap-table-zh-CN.js' %}"></script>  

<table id="table"></table>
<script>
    var columns = [
        {
            checkbox:true,
            }
        },{
            field:"id",
            title:"ID",
            footerFormatter:function(){
                return 'Total'
            }
        },{
            field:"price",
            title:"价格",
            sortable:true,
            formatter:function(value){
                return '￥'+value.toFixed(2)
            },
            footerFormatter:function(data){
                var sum = 0
                $.each(data,function(index,value){
                    sum += value.price
                })
                return "￥"+sum
            },
        },{
            field:'created',
            title:'创建时间'
        },{
            field:'user_id',
            title:'用户编号'
        },{
            field:'state',
            title:'状态'
        }
    ]
    $(function(){
        $("#table").bootstrapTable({
            url:"{% url 'shopper:table' %}",
            columns:columns,
            sidePagination:'client',
            maintainMetaData:true,
            pageSize:5,
            pageNumber:1,
            pagination:true,
            paginationPreText:'上一页',
            paginationNextText:'下一页',
            sortName:'id',
            sortOrder:'desc',
            sortReset:true,
            clickToSelect:true,
            showFooter:true,
        })
    })
</script>
```  

urls.py:  

```python
path('shopcart/', views.cartView, name = 'shopcart'),
path('table/',views.table,name='table'),
```  

views.py：  

```python
def cartView(request):
    return render(request,'shopper/shopcart.html')
def table(request):
    orderInfos = OrderInfos.objects.all()
    total = orderInfos.count()
    rows = list(orderInfos.values())
    data = {
        'total':total,
        'rows':rows
    }
    return HttpResponse(json.dumps(data,cls=serializers.json.DjangoJSONEncoder))
```  

需要注意的：  

1. bootstrapTable 参数配置 中的sidePagination,默认值是'client'，即接收全部数据，然后在前端分页，后端 views.py 不需要处理sort、order、offset、limit等参数，而且在bootstrapTable中设置maintainMetaData:true后，可以保持checkbox的选定状态，比较简单。除非数据量巨大，一般均取默认值。

2. 如果sidePagination='server'，即在服务端分页，此时前端发送请求参数形式是，`?sort=id&order=desc&offset=0&limit=5`，sort等于bootstrapTable配置中的sortName,order等于bootstrapTable配置中的sortOrder，limit等于bootstrapTable配置中的pageSize，offset等于bootstrapTable配置中的`(pageNumber-1)*pageSize`。由于前端传来了sort、order、offset、limit参数，因此在 def table中要进行处理，得到的数据返回前端给bootstrapTable。而且checkbox的选定状态的处理计较复杂
3. 前端bootstrapTable要求的数据格式为：  

```js
{
    'total':total,
    'rows':[
        {
        'id':id,
        'price':price,
        },{
        }
    ]
}
```  

1. 由于后端QuerySet服务器返回的JSON数据中，只有rows里面的数据，缺少包装头，因此需要加上data = {'total':total,'rows':rows}，其中的total等于记录数

2. 后端分页中，rows = list(orderInfos[offset:offset+limit].values())得到某页数据，由offset和limit决定。

3. 当采用json.dumps()序列化时，QuerySet中的日期不能序列化，必须加上参数`cls=serializers.json.DjangoJSONEncoder`  

### 4.2 新增记录  

实现思路：  

- 在html中，添加Modal，框中为一表单，对应Django模型中的各个属性，用来接收和显示数据。

- 新增按钮点击后，显示Modal框，并进行初始化，Modal框中的各输入框为默认值。

- 由于该框是新增记录和修改记录共用，因此初始化时，根据点击按钮的不同，显示不同的相关信息。

- Modal框接受输入信息后，点击提交按钮，通过js的click事件，提交至新增记录路由。

- python Django后端处理数据、新增一条记录。  

html 新增Modal框组件，为了显示日期，引入datatimepicker控件  

```html
<link rel="stylesheet" href="{% static 'jquery.datetimepicker.css' %}">
<script src="{% static 'jquery.datetimepicker.full.js' %}"></script>
```  


```html
<div class="modal fade" id="modal" data-backdrop="static" data-keyboard="false" tabindex="-1" aria-labelledby="staticBackdropLabel" aria-hidden="true">
        <div class="modal-dialog">
          <div class="modal-content">
            <div class="modal-header">
                <h5 class="modal-title"></h5>
                <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                    <span aria-hidden="true">&times;</span>
                </button>
            </div>
            <div class="modal-body">
                <form class="ml-5">
                    {% csrf_token %}
                    <div class="form-group row d-none">
                        <label class="col-3 col-form-label">ID：</label>
                        <input type="text" class="form-control col-6" id="idinput" readonly>
                    </div>
                    <div class="form-group row">
                        <label for="priceinput" class="col-3 col-form-label">价格：</label>
                        <input type="text" class="form-control col-6" id="priceinput">
                    </div>
                    <div class="form-group row">
                        <label for="createdinput" class="col-3 col-form-label">创建时间：</label>
                        <input type="text" class="form-control col-6 datetimepicker" id="createdinput" placeholder="请选择时间">
                    </div>
                    <div class="form-group row">
                        <label for="useridinput" class="col-3 col-form-label">用户编号：</label>
                        <input type="text" class="form-control col-6" id="useridinput">
                    </div>
                    <div class="form-group row">
                        <label for="stateinput" class="col-3 col-form-label">状态：</label>
                        <select class="form-control col-6" id="stateinput">
                            <option selected class="d-none">请选择</option>
                            <option>待支付</option>
                            <option>已支付</option>
                            <option>发货中</option>
                            <option>已签收</option>
                            <option>退货中</option>
                        </select>
                    </div>
                </form>
            </div>
            <div class="modal-footer">
                <button type="button" class="btn btn-secondary" data-dismiss="modal">关闭</button>
                <button type="button" class="btn btn-primary submit"></button>
            </div>
          </div>
        </div>
    </div>
```  

JS 新增datetimepicker初始化、新增按钮点击事件和Modal框中的点击提交事件  

```js
$(".datetimepicker").datetimepicker({
    format:"Y-m-d",
    timepicker:false
});
$.datetimepicker.setLocale('zh');  

$("#addbtn").click(function(){
    $("#modal").modal('show')
    $(".modal-title").html('新增记录')
    $(".submit").html('提交')
    $("#idinput").parent().addClass('d-none')
    $("#priceinput").val('')
    $("#createdinput").val('')
    $("#useridinput").val('')
    $("#stateinput").val('请选择')
})  

$(".submit").click(function(){
    var url="{% url 'shopper:update_or_create' %}"
    var data = {
        'price':$("#priceinput").val(),
        'created':$("#createdinput").val(),
        'user_id':$("#useridinput").val(),
        'state':$("#stateinput").val(),
        'csrfmiddlewaretoken':'{{ csrf_token }}',
    }
    $.post(url,data,function(data){
        console.log(data)
        $("#modal").modal('hide')
        $("#table").bootstrapTable('refresh')
    })
})
```  

urls.py 中新增  

```python
path('update_or_create/',views.update_or_create,name='update_or_create'),
```  

views.py 中新增  

```python
def update_or_create(request):
    id = request.POST.get('id','')
    price = request.POST.get('price','')
    created = request.POST.get('created','')
    user_id = request.POST.get('user_id','')
    state = request.POST.get('state','')
    d = dict(price=price,created=created,user_id=user_id,state=state)
    if id:
        OrderInfos.objects.filter(id=id).update(**d)
        return HttpResponse('更新成功')
    else:
        OrderInfos.objects.create(**d)
        return HttpResponse('创建成功')
```  

需要注意的：  

1. datetimepicker控件初始化时，其显示格式要与Django中的相应模型数据格式一致，用 .val() 获取值
2. Modal框中的提交按钮点击后，以ajax方式，完成后，隐藏Modal，并且刷新bootstrapTable

3. 由于数据表单以post方式提交，因此在 form 标签中，增加{% csrf_token %}，在 $.post 的data参数中，增加`'csrfmiddlewaretoken':'{{ csrf_token }}',`

4. 在后端views.py中，没有找到update_or_create的使用方法，因此没有采用update_or_create方法，而是根据id是否有值，分别新增数据和后面的修改数据。  

### 4.3 修改记录  

实现思路：大部分与新增记录相同，只是Modal框初始化时，各输入框的值为bootstrapTable的选定数据。JS 新增修改按钮点击事件：  

```js
$("#updatebtn").click(function(){
    $("#modal").modal('show')
    $(".modal-title").html('修改记录')
    $(".submit").html('保存修改')
    row = $("#table").bootstrapTable('getSelections')[0]
    $("#idinput").parent().removeClass('d-none')
    $("#idinput").val(row.id)
    $("#priceinput").val(row.price)
    $("#createdinput").val(row.created)
    $("#useridinput").val(row.user_id)
    $("#stateinput").val(row.state)
})
``` 

需要注意的是， `row = $("#table").bootstrapTable('getSelections')[0]` 表示选定行，由于是列表，此处选择第一项。  

### 4.4 删除记录 

实现思路：获得选定行的id数组，并将其传给后端，后端将id在该数组中的记录删除。js 新增删除按钮点击事件：  

```js
$("#deletebtn").click(function(){
    var row = $("#table").bootstrapTable('getSelections')
    var idlist = []
    $.each(row,function(index,value){
        idlist.push(value.id)
    })
    url = "{% url 'shopper:delete' %}"
    data = {'idlist':JSON.stringify(idlist)}
    $.get(url,data,function(data){
        console.log(data)
        $("#table").bootstrapTable('refresh')
    })
})
```  

views.py 中新增：  

```python
def delete(request):
    idlist = request.GET.get('idlist','')
    idlist = json.loads(idlist)
    for id in idlist:
        OrderInfos.objects.filter(id=id).delete()
    return HttpResponse('删除成功')
```  

需要注意的是：前端传数据到后端时，要用JSON.stringify()方法对数组进行JSON格式化，后端接收到数据后，要用json.loads()方法对字符串进行转换。  

### 4.5 查询记录  

实现思路：利用bootstrapTable中的配置参数queryParams，传递查询参数。html中新增查询表单：  

```html
<form>
    <div class="form-row">
        <div class="col">
            <input type="text" class="form-control" placeholder="price" id="pricequery">
        </div>
        <div class="col">
            <input type="text" class="form-control datetimepicker" placeholder="创建时间在此之后" id="createdafterquery">
        </div>
        <div class="col">
            <input type="text" class="form-control datetimepicker" placeholder="创建时间在此之前" id="createdbeforequery">
        </div>
        <div class="col">
            <!-- <input type="text" class="form-control" placeholder="state" id="statequery"> -->
            <select class="form-control" id="statequery">
                <option selected class="d-none">请选择</option>
                <option>待支付</option>
                <option>已支付</option>
                <option>发货中</option>
                <option>已签收</option>
                <option>退货中</option>
            </select>
        </div>
        <div class="col">
            <a class="btn btn-dark" id="querybtn" href="javascript:;">查询</a>
        </div>
    </div>
</form>
```  

js中bootstrapTable初始参数中新增queryParams,同时新增查询按钮点击事件：  

```js
$("#table").bootstrapTable({
    ...
    queryParams:function(params){
        params.price = $("#pricequery").val(),
        params.createdafter = $("#createdafterquery").val()
        params.createdbefore = $("#createdbeforequery").val()
        params.state = $("#statequery").val()
        return params
    }
})  

$("#querybtn").click(function(){
    $("#table").bootstrapTable("refresh")
})
```  

views.py def table 新增查询功能:  

```python
price = request.GET.get('price','')
createdafter = request.GET.get('createdafter','')
createdbefore = request.GET.get('createdbefore','')
state = request.GET.get('state','')
if price:
    orderInfos = orderInfos.filter(price=float(price))
if createdafter:
    createdafter = datetime.strptime(createdafter,'%Y-%m-%d').date()
    orderInfos = orderInfos.filter(created__gte= createdafter)
if createdbefore:
    createdbefore = datetime.strptime(createdbefore,'%Y-%m-%d').date()
    orderInfos = orderInfos.filter(created__lte =createdbefore)
if state != '请选择':
    orderInfos = orderInfos.filter(state = state)
```  

需要注意的是：  

1. bootstrapTable中的配置参数queryParams。该参数表示传递给table路由的参数，默认包括params.limit、params.offset、params.sort、params.order等，与 views.py中 def table(request) 的参数名称相对应。  

```js
queryParams:function(params){
        params.price = $("#pricequery").val(),
        params.createdafter = $("#createdafterquery").val()
        params.createdbefore = $("#createdbeforequery").val()
        params.state = $("#statequery").val()
        return params
    }
```  

1. 标签form中的button，即使没有设置type="submit"，点击后，还是默认提交数据至当前路由（原因不清楚），因此为避免不可预见情况发生，将button标签改为a 标签。

2. datetimepicker控件传给后端的日期数据格式为字符串，而Django Model中数据类型为DateField，为方便二者比较，必须用 `datetime.strptime(createdbefore,'%Y-%m-%d').date()`语句将日期字符串转换为日期。