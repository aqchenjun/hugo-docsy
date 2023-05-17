---
categories: JavaScript 入门到精通
title: JavaScript 基础语法
tags: [教程,JavaScript]
date: 2023-04-14
lastmod: 2023-05-16
thumbnail: 
published: "true"
slug: zfwc2mp
---

>《JavaScript 入门到精通》读书笔记之一

## 1 思维导图

```plantuml
@startmindmap Mindmap
skinparam ArrowColor orchid
skinparam ArrowThickness 2
skinparam NodeBackgroundColor lemonchiffon
skinparam NodeBorderColor orchid
* js基础语法
** 注释
***_ 行注释：//
***_ 块注释：/* 语句块 */
** 语句结束标志
*** 分号 ;
*** 换行
** script 标签的位置
***_ 外部式
****:<script src = " "></script>
----
一般位于 <head> </head> 之间。
另外，此处 script 标签之间的 js 语句不执行;
***_ 内部式
****: <script> js 语句 </script>
----
位于 </body> 前;
***_ 内联式
****: 其他标签内部
----
<button onclick= js函数></button>;
** 输入命令
***_ prompt(提示信息,默认值)
** 输出命令
***_ alert()
***_ console.log()
***_ document.write()
**** document.write()会覆盖其他的输出
** 变量
***_ 声明与赋值
****:let age;
let age = 19;;
***_ 命名规范
****:1.由字母、数字、下划线、$符号组成，不能以数字开头
2.不能是关键字和保留字，例如：var for while const
3.严格区分大小写
4.一般遵守小驼峰式命名法;
** 常量
*** const
** 数据
*** 数据类型
****_ 基本数据类型
*****:- number 数值型
- string 字符型
- boolean 布尔型
- undefined 未定义
- null 空引用;
****_ 引用数据类型
*****:- object 对象
- function 函数
- array 数组;
*** 数据类型的转换
****_ 转数值
***** Number()
***** parseInt()
***** parseFloat()
****_ 转字符串
***** str.toString()
***** String(str)
***_ 字符串的拼接
**** 运算符 +
****:模板字符串
----
1.用反引号 (` `)
2.用 ${变量} 包住变量
3.变量之间可以运算
4.实例：`myname is ${name}`;
@endmindmap
``` 

## 2 案例
### 2.1 变量值的交换
#### 2.1.1 方法一，解构赋值
```js
let a = 1;
let b = 2;
[a,b] = [b,a];
// 赋值语句要以分号(;)结束，否则出错
```
#### 2.1.2 方法二，用临时变量
```js
let a = 1;
let b = 2;
let c = a;
a = b;
b = c;
```
### 2.2 计算银行卡余额案例

#### 2.2.1 题目描述 

  1、用户输入总的银行卡金额，依次输入本月花费的电费，水费，网费。  
  2、页面打印一个表格，计算出本月银行卡还剩下的余额。
#### 2.2.2 代码
##### 2.2.2.1 style.css
```css
  /* 设置表格本身及内部文本的对齐方式，设置边框线为单线 */
table {
    margin: auto;
    text-align: center;
    border-collapse: collapse;
}
/* 设置表格的边框线为实线、黑色 */
table,
th,
td {
    border: 1px solid black;
}
```

##### 2.2.2.2 script.js
```js
window.onload = init;
function init() {
    const tableHeader = `<caption><h3>2020年12月消费支出</h3></caption>
<tr>
    <th>银行卡金额</th>
    <th>电费</th>
    <th>水费</th>
    <th>网费</th>
    <th>银行卡余额</th>
</tr>`;
    let money = parseFloat(prompt('输入银行卡金额：'));
    let power = parseFloat(prompt('输入电费：'));
    let water = parseFloat(prompt('输入水费量：'));
    let net = parseFloat(prompt('输入网费：'));
    document.querySelector('table').innerHTML = `${tableHeader}
    <tr>
        <td>${money}</td>
        <td>${power}</td>
        <td>${water}</td>
        <td>${net}</td>
        <td>${money - power - water - net}</td>
    </tr>`}; 
```

##### 2.2.2.3 exam.html
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <link rel="stylesheet" type="text/css" href="style.css">
    <script src="script.js"></script>
</head>
<body>
    <table></table>
</body>
</html>
```

>[!note]+ 知识点
>1. 表格本身的居中对齐，要使用 margin:auto
>
>2. 表格边框由双线变为单线，要使用 border-collapse: collapse
