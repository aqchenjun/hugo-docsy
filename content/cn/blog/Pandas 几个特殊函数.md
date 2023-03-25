---
source: Pandas
title: Pandas 几个特殊函数
tags: [ 教程, python]
date: 2023-03-13
lastmod: 2023-03-21
thumbnail: https://s1.vika.cn/space/2023/03/25/8bdb572556604a6e91b3396ac6600a26?attname=20200517121952353.png  
published: "true"
---


## 1 函数列表  

当需要重复调用 DataFrameGroupby 对象，并且有聚合计算时，pipe方法通常很有用。  

|           | Series                                                                         |                                             | DataFrame                                                                                                                                                     |                              |
| --------- | ------------------------------------------------------------------------------ | ------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------- |
|           | 传入的参数                                                                     | 返回值                                      | 传入的参数                                                                                                                                                    | 返回值                       |
| map       | 1、函数。参数是Series中的每个元素。2、字典。对应Series中的每个元素。           | 与Series 相同 index的Series                 |                                                                                                                                                               |                              |
| applymap  |                                                                                |                                             | 函数。参数是DataFrame中的每个元素。                                                                                                                           | DataFrame                    |
| apply     | 函数。参数是Series中的每个元素，有args 和 wargs。                              | Series 或者 DataFrame                       | 1、函数。参数是整个DataFrame，计算方向由axis 确定。有args 和 kwargs。2、axis。0 or ‘index’：对列1 or ‘columns’：对行                                          | 沿 axis 的DataFrame 或Series |
| pipe      | 函数。参数是整个Series，有args 和 kwargs。                                     | 与调用的函数有关                            | 函数。参数是整个DataFrame，有args 和 kwargs。                                                                                                                 | 与调用的函数有关             |
| transform | 函数、函数名、上述两种的列表和字典。函数的参数是整个Series，有args 和 kwargs。 | 新的 DataFrame 或 Series                    | 1、函数、函数名、上述两种的列表和字典。函数的参数是整个DataFrame 的每列或每行，由axis 确定。有args 和 kwargs。2、axis。0 or ‘index’：对列1 or ‘columns’：对行 | 新的 DataFrame               |
| agg       | 函数、函数名、上述两种的列表和字典。函数的参数是整个Series，有args 和 kwargs。 | scalar 、Series 、DataFrame，与调用方法有关 | 1、函数、函数名、上述两种的列表和字典。函数的参数是整个DataFrame，有args 和 kwargs。2、axis。0 or ‘index’：对列1 or ‘columns’：对行                           | scalar 、Series 、DataFrame，与调用方法有关                              |

## 2 示例数据集  

```python
boolean=[True,False]
gender=["男","女"]
color=["white","black","yellow"]
data=pd.DataFrame({
    "height":np.random.randint(150,190,100),
    "weight":np.random.randint(40,90,100),
    "smoker":[boolean[x] for x in np.random.randint(0,2,100)],
    "gender":[gender[x] for x in np.random.randint(0,2,100)],
    "age":np.random.randint(15,90,100),
    "color":[color[x] for x in np.random.randint(0,len(color),100) ]
}
)
```

![image.png](https://s1.vika.cn/space/2023/03/13/f13f7cf23f7247f2bde83d50fa45e20b)

## 3 把数据集中gender列的男替换为1，女替换为0  

```python
#map 方法
data['gender'].map({'男':1,'女':0})
data['gender'].map(lambda x:1 if x=='男' else 0)

#transform 方法
data['gender'].transform(lambda x:1 if x=='男' else 0) 

#pipe 方法，注意与 transform 方法的区别
data['gender'].pipe(lambda x:x.replace({'女':0,'男':1}))
data['gender'].pipe(lambda x:x.map({'男':1,'女':0})) 

#apply 方法
data['gender'].apply(lambda x:1 if x=='男' else 0)  

#replace 方法
data['gender'].replace({'男':1,'女':0})
```

## 4 age列都减3  

```python
data['age'].map(lambda x:x-3)
data['age'].transform(lambda x:x-3)
data['age'].pipe(lambda x:x-3)
data.apply(lambda x:x['age']-3,axis=1)    #注意此时的 axis 的值
```

## 5 对 "height","weight","age" 三列求和、求平均数  

```python
data[["height","weight","age"]].agg(['sum','mean'])
data[["height","weight","age"]].apply([sum,'mean'])
data[["height","weight","age"]].pipe(lambda x:x.agg(['sum','mean']))
```

>注意，由于数据集未分组，因此 transform 不能使用聚合函数。

## 6 计算体重指数BMI  

```python
#直接计算
data['weight']/(data['height']/100)**2
#pipe方法
data.pipe(lambda x:x['weight']/(x['height']/100)**2)
#apply方法
data[["height","weight"]].apply(lambda x:x['weight']/(x['height']/100)**2,axis=1)
```