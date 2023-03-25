---
source: Pandas
title: Pandas 输入输出
tags: [ 教程, python]
date: 2023-03-17
lastmod: 2023-03-23
thumbnail: https://s1.vika.cn/space/2023/03/25/ff6af523108a4729bdfb3bf4eaa91e44?attname=automation-3624328_960_720.jpg
published: "true"
---


## 1 json  

### 1.1 读入 json 文件  

```python
#从 json 文件中读入
data = pd.read_json(path_or_buf=None)    #a valid JSON str, path object or file-like object

#或者用 python 的json 模块
with open(file_path,'r') as fp:
    data = json.load(fp)
#    data=json.loads(fp.read())
```  

当需要从 json 字符串得到DataFrame，以及当 json 文件内出现嵌套时，需要应用 json_normalize 方法。  

```python
pd.json_normalize(data,                #dict or list of dicts，包括上面语句得到的DataFrame的某个字段
                  record_path=None,    #json 里面嵌套列表的名称
                  meta=None,           #json 里面的 key 名称
                  meta_prefix=None,
                  record_prefix=None,
                  errors='raise',
                  sep='.',
                  max_level=None)    #解析的最大层数，从0开始 
data = [
    {"id": 1, "name": {"first": "Coleen", "last": "Volk"}},
    {"name": {"given": "Mark", "family": "Regner"}},
    {"id": 2, "name": "Faye Raker"},
]
pd.json_normalize(data)
#结果
id name.first name.last name.given name.family        name
0  1.0     Coleen      Volk        NaN         NaN         NaN
1  NaN        NaN       NaN       Mark      Regner         NaN
2  2.0        NaN       NaN        NaN         NaN  Faye Raker  

data = [
    {"id": 1, "name": [{'first':"Coleen",'second':"Volk"}]} ,
    {"id": 2, "name": [{'first':"Faye Raker",'second':'tom'}]},
]
pd.json_normalize(data,record_path='name')
#结果
    first         second
0   Coleen        Volk
1   Faye Raker    tom
```  

需要注意的是，json 数据中，当设置 record_path ='name' 值时，不一定每条数据都有'name'键值，此时可以将该键值作为默认值添加进去，如下所示：

`pd.Series(data).apply(lambda x:x.setdefault('name',[]))`  

### 1.2 写入 json 文件  

```python
DataFrame.to_json(path_or_buf=None,    # 如果为 None，则返回 json 字符串。
                  orient=None,    #'records'、'columns'、'index'、'split'等 ，建议选 'records'
                  date_format=None)
#python json 模块
with open(file_path,'w') as fp:
    json.dump(DataFrame.to_json(),fp)
```  

### 1.3 用 python 处理 json 对象  

用 python 处理 json 对象，需要导入 json 模块，涉及到四个函数，json.load()、json.loads()、json.dump()、json.dumps()。带后缀 s 表示处理字符串，不带后缀 s 表示处理文件。  

| 方法         | 作用                         |
| ------------ | ---------------------------- |
| json.dumps() | 将python对象编码成Json字符串 |
| json.loads() | 将Json字符串解码成python对象 |
| json.dump() | 将python对象转化成json格式储存到文件中 |
| json.load() | 将文件中的json的格式转化成python对象提取 |

```python
with open(file_path,'r') as fp:
    data=json.load(fp)
    data=json.loads(fp.read())    #fp.read() 返回的是字符串，所以使用 loads()函数
with open(file_path,'w') as fp:
    json.dump(字符串,fp)
json_str = json.dumps(字符串)
```  

## 2 excel  

### 2.1 读入 excel 文件  

```python
pd.read_excel(io,           #文件名称或文件对象，包括 URL、StringIO、read()方法返回的对象等
              sheet_name=0, #excel 里面的 sheet 的名称或 index(从 0 开始)
              header=0,
              names=None,
              index_col=None,
              usecols=None,  
              skiprows=None,
              nrows=None,
```  

### 2.2 写入 excel 文件  

#### 2.2.1 单个 sheet 的 excel 文件的保存  

```python
df.to_excel(file_path,
            sheet_name='Sheet1')
```  

#### 2.2.2 多 sheet 的 excel 文件的保存  

```python
df.to_excel(excel_writer,
            sheet_name='Sheet1'
```  

- 首先，创建 ExcelWriter 对象
- 其次，针对每个 df ，设置不同的 sheet_name，连接到 ExcelWriter 对象
- 最后，保存 ExcelWriter 对象  

```python
excel_file = pd.ExcelWriter("file1.xlsx")        #创建 ExcelWriter 对象
df1.to_excel(excel_file,sheet_name='Sheet1',index=False)
df2.to_excel(excel_file,sheet_name='Sheet2',index=False)
df3.to_excel(excel_file,sheet_name='Sheet3',index=False)
excel_file.save()                                #保存 ExcelWriter 对象  
#使用 with 语句可以简化
with pd.ExcelWriter('file1.xlsx') as excel_file:
	df1.to_excel(excel_file,sheet_name='Sheet1',index=False)
	df2.to_excel(excel_file,sheet_name='Sheet2',index=False)
	df3.to_excel(excel_file,sheet_name='Sheet3',index=False)
```  

## 3 MySQL  

### 3.1 读入 MySQL 数据库中的数据表  

需要安装的包：mysqlclient、mysql-connector、sqlalchemy  

```python
import pandas as pd
import MySQLdb    #安装包的名称是 mysqlclient
import mysql.connector as mysqlconnector    #安装包的名称是mysql-connector
from sqlalchemy import create_engine
# engine_str = 'mysql+DBAPI名称://用户名:密码@主机名或IP地址:端口/数据库名称?charset=utf8'
engine = create_engine('mysql+mysqlconnector://root:123456@localhost:3306/babys?charset=utf8')
sql = 'select * from shopper_orderinfos'
pd.read_sql(sql,engine)
#参数 sql 可以是查询，也可以是一个表
pd.read_sql('shopper_orderinfos',engine)
#也可以直接MYSQLdb连接，但 pandas 官方推荐 sqlalchemy
#conn = MySQLdb.connect(host='localhost',user='root',password='123456',port=3306,database='babys')
#pd.read_sql('select * from shopper_orderinfos',con=conn)
```  

### 3.2 保存 DataFrame 至 MySQL 数据库  

```python
df.to_sql(name,             #表名
          con,              #sqlalchemy.engine
          if_exists='fail', #表名已经存在的操作,'fail': 报错,'replace': 覆盖,'append':追加
          index=True,
          index_label=None,
          chunksize=None,
          dtype=None,
          method=None)
```  

## 4 读取字符串转换为DataFrame  

### 4.1 json字符串  

需要注意的是，字符串中，各字典分行排列，之间不能有逗号。同时，read_json 函数的参数 lines=True。  

```python
pd.read_json(data，lines=True)
pd.read_json(StringIO(data),lines=True)
#data 格式如下：
data = """
   {"a": 1, "b": 2}
   {"a": 3, "b": 4}
   """
```  

### 4.2 文本字符串  

利用 StringIO 对象转换字符串。  

```python
data = "a,b,c\n1,2,3\n4,5,6\n7,8,9"    #字符串以 \n 为分隔符
pd.read_csv(StringIO(data),header=None)    #字符串不包括列名
pd.read_csv(StringIO(data))            #字符串中的 a,b,c 为列名
```  

## 5 关于 skiprows 和 usecols参数  

pandas 的读取文件方法的参数中，skiprows表示从头开始跳过的行，可以是数值、列表和函数，实际上可以实现选择性读取指定行。usecols 表示读取的列，可以是列表和函数。 

需要注意的是，如果是函数，skiprows 的函数参数是行号，如果函数结果返回True，即跳过，不显示，返回False，则显示，此时header的行号为0。usecols 的函数参数是列名，如果函数结果返回True，则显示，返回False，则不显示。  

```python
#只显示偶数行
pd.read_csv('某招聘网站数据.csv',skiprows= lambda x:x%2 )
#当行号为偶数是，x%2 = 0，即为 False，当行号为奇数时，x%2 != 0，即为 True。
#只显示奇数行
pd.read_csv('某招聘网站数据.csv',skiprows= lambda x:not (x==0 or x%2))
#只显示 1、3、6、7行
pd.read_csv('某招聘网站数据.csv',skiprows= lambda x:x not in [0,1,3,6,7]) 
#只显示指定列表中的列
pd.read_csv('某招聘网站数据.csv',usecols = lambda x:x in ['positionId','test', 'test1'])
```  

## 6 批量读取文件并合并成一个文件  

```python
file_path='demodata'                   #文件所在路径
file_list = os.listdir(file_path)      #获得文件列表
for i in range(0,len(file_list)):      #针对每个文件
    if i == 0:                         #使用第一个文件建立 df
        df = pd.read_excel(os.path.join('demodata',file_list[0]))
    else:
        df = pd.concat([df,pd.read_excel(os.path.join('demodata',file_list[i]))])        #两两合并

df
```