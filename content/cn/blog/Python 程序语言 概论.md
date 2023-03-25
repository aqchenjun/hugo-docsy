---
source: Python 程序语言
title: Python 程序语言 概论
tags: [ 教程, python]
date: 2023-03-11 
lastmod: 2023-03-15 
thumbnail: https://s1.vika.cn/space/2023/03/25/2b28a86f95204c798f00e99699455f82?attname=road-7508538_960_720.jpg
published: "true"
---

## 1 密码的正则表达式  

假定一个密码位数8-16，由数字、大写字母和小写字母组成，判断密码合规的正则表达式如下：  

```python

# 方法一
^(?=.*[0-9])(?=.*[a-z])(?=.*[A-Z])[a-zA-Z0-9]{8,16}$

# 方法二，这个正则表达式判断密码长度小于7、大于17、不含数字、不含小写字母、不含大写字母，满足任何一种情况均表示匹配，一旦匹配，表示密码不合规。只有不匹配，表示密码长度介于8-16、包含数字、小写字母和大写字母。

^(.{0,7}|.{17,}|[^0-9]+|[^a-z]+|[^A-Z]+)$

```

  

## 2 random 模块

  

```python

#返回从总体序列或集合中选择的唯一元素的 k 长度列表。用于无重复的随机抽样。返回新列表，同时保持原列表不变。

random.sample(population, k)  

#从非空序列 seq 返回一个随机元素

random.choice(seq)  

#从序列 range(start, stop[, step]) 中返回一个随机数

random.randrange(start, stop[, step])

random.choice(range(start, stop[, step]))  

#返回随机整数 N 满足 a <= N <= b

random.randint(a,b)  

#返回 [0.0, 1.0) 范围内的一个随机浮点数

random.random()  

#返回一个随机浮点数 N ，当 a <= b 时，a <= N <= b ，当 b < a 时，b <= N <= a
random.uniform(a, b)
```

  

## 3 星号的功能
  
1、表示乘号

2、表示倍数  

```python

'str' *3      #表示字符串增加为 3 倍

```

3、单个星号 “*” 用在元组变量前面，表示打包或解包

4、两个星号 “**” 用在字典变量前面，表示打包或解包
  

## 4 字典格式的字符串转换为字典

  
```python

from urllib.request import urlopen,Request
import json,ast,requests
from urllib import parse  

base_url = "https://sthjt.ah.gov.cn/site/label/8888?IsAjax=1&dataType=json&_=0.805670726353581&isJson=true&"
args = {
    'cityCode':'340100',
    'timeBegin':'2021-01-01+00',
    'timeEnd':'2021-12-31+23',
    'isPage':'true',
    'pageIndex':'0',
    'pageSize':'1',    'labelName':'airQualityDailyNum'
}
url = f"{base_url}{parse.urlencode(args)}"
response = requests.request('get', url,headers={
    'User-Agent':'Mozilla/5.0 (Linux; Android 6.0; Nexus 5 Build/MRA58N) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/102.0.0.0 Mobile Safari/537.36'
})
with response:
    text = json.loads(response.text)
    text = ast.literal_eval(text)
```

response.text 返回的是字符串文本：  

```text
"{\"data\":[{\"aqi\":\"94\",\"area\":\"合肥市\",\"cityCode\":340100,\"co′′′24h\":\"0.411\",\"level\":\"II级\",\"measure\":\"\",\"no224h\":\"15\",\"o38h24h\":\"152\",\"pm1024h\":\"31\",\"pm2524h\":\"11\",\"primaryPollutant\":\"臭氧8小时\",\"quality\":\"良\",\"so224h\":\"10\",\"timePoint\":1654880400000,\"unheathful\":\"\"}],\"pageCount\":1620,\"pageIndex\":0,\"pageSize\":1,\"startNumber\":0,\"total\":1620}"
```

首先用json.loads()方法将其转换为json字符串，再用ast.literal_eval()方法转换为字典。  

## 5 如何导入不同文件夹下的文件(module)  

为方便表述，我们假设：a.py 要 import 文件 b.py。
  

### 5.1  a.py 和 b.py 在同一目录下  

直接 import 即可。  

`import** b`  或者 `from b import *`
  
### 5.2 b.py 在 子目录 test下，a.py 和目录 test 在同一目录下  

![image.png](https://s1.vika.cn/space/2023/03/12/7ccc04223d5a41f9a68c611d58b7a2cc)

首先需要在test目录下有一个空文件 init.py，如果没有，则需要创建。创建该文件的目的是将test目录变成一个Python包。 

import 语句：  

`import test.b`  或者
`from test.b import *`

如果 test 目录中还有子目录 sub_test/，则不需要在 sub_test/ 中创建 __init__.py  即可通过如下方式导入 sub_test/ 中的 c.py。

`import test.sub_test.c`或者
`from test.sub_test.c import  *`

  

### 5.3 b.py在任意路径下

假设 b.py 在路径 H:\Documents\user\test 下，则需要通过如下代码将路径加入到系统路径中，然后直接导入 b.py即可。需要在test目录下有一个空文件 init.py，如果没有，则需要创建。
由于python 是解释型语言，将路径加入到系统路径的语句必须在导入语句前。  

```python
import sys
sys.path.append(r"H:\Documents\user\test")
#sys.path.append("H:\\Documents\\user\\test") 因为 python中 '\' 是转义符号

import b
#from b import *
```
  
sys.path 是一个列表，表示 Python 模块的搜索路径。sys.path的值来自四个方向：
  
1. 当前工作目录；
2. 环境变量 PYTHONPATH 列表内容（如果没有定义这个，则只有当前目录）；
3. Python 的安装路径；
4. 用户自己添加的搜索目录。
