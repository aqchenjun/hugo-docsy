---
categories: HTML、CSS和JavaScript
title: JavaScript的字符串操作
tags: [教程,前端,javascript]
date: 2023-03-31
lastmod: 2023-03-31
thumbnail: 
published: "true"
slug: j1qm66x
---

>《HTML 、CSS和JavaScript》读书笔记之五
 

## 1 JS 中字符串转日期  

一般日期字符串格式是：value=yyyy-mm-dd，转换成js日期对象的方法：  

1、首先将字符串格式转换成yyyy/mm/dd

```js
//将字符 '-' 替换成字符 '/'
value = value.replace(/-/g,'/')
```  

2、再转换成Date对象  

```js
date=newDate(Date.parse(value))
```  

3、Date对象的常用方法：
- date.getFullYear()，取得完整年份
- date.getMonth()+1，取得月份
- date.getDate()，取得天数
- getHours()，getMinutes()，getSeconds()  

## 2 字符串截取  

## 3 indexOf： 查找子串在母串的第一次出现的索引值  

```js
var str = 'abckef'
var index = str.indexOf(str)   #Of 首字母大写
```  

## 4 substring：用于提取字符串中介于两个指定下标之间的字符  

起始下标为 0 ，末尾下标为 -1  

```js
var str = "0123456789"
alert(str.substring(5));------------"56789"
alert(str.substring(0,5));----------"01234"
```  

## 5 substr ：用于返回一个从指定位置开始的指定长度的子字符串  

```js
var str = "0123456789"
alert(str.substr(0,5));-------------"01234"
```  

## 6 slice  

```js
var str = "0123456789"
alert(str.slice(6,-1));-------------"6789"
```