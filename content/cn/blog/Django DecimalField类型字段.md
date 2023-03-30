---
categories: Django
title: Django DecimalField类型字段
tags: [教程,Python]
date: 2023-03-28
lastmod: 2023-03-29 
slug: 4k0545g
thumbnail:  
published: "true"
---


Django 模型字段的数据类型中，FloatField不能提供准确值，如果需要准确的小数值，应使用DecimalField。
模型字段定义：
`contractAmount = models.DecimalField('合同额',max_digits=9, decimal_places=2)`  

DecimalField 字段类型定义时，应设置max_digits 和decimal_places两个参数，其中max_digits表示除去小数点后的数字最大总位数，decimal_places表示小数点后的位数。

假设该字段的最大数是 9999.99，则max_digits = 6，decimal_places=2。

DecimalField 字段创建和修改时，要将数据源转换为decimal类型。由于数据源可能是空值，应加上判断。  

```python
from decimal import Decimal
contractAmount = request.POST.get('amount')
if contractAmount:
	contractAmount = Decimal(contractAmount)
else:
	contractAmount = Decimal(0.00)
#数据源超过了max_digits 和 decimal_places 的值会自动舍去。
```��
```