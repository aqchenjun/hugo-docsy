---
categories: JavaScript 入门到精通
title: JavaScript 基础：运算符
tags: [教程,JavaScript]
date: 2023-04-21
lastmod: 2023-05-16
thumbnail: 
published: "true"
slug: 3zbt9kj
---

>《JavaScript 入门到精通》读书笔记之二

## 1 思维导图
```plantuml
@startmindmap Mindmap
skinparam ArrowColor orchid
skinparam ArrowThickness 2
skinparam NodeBackgroundColor lemonchiffon
skinparam NodeBorderColor orchid
* 运算符
** 算术运算符
***:1. 乘法 *
2. 除法 / 
3. 加法 +
4. 减法 -
5. 取模(求余数) %;
** 赋值运算符
***:1. 加法赋值 +=
2. 减法赋值 -=
3. 乘法赋值 *=
4. 除法赋值 /=
5. 取余赋值 %=;
** 自增/自减运算符
***:1. 自增运算符 ++
2. 自减运算符 --;
** 比较运算符
***:1. 大于 >、小于 >、大于等于 >=、小于等于 <=
2. 左右两边值是否相等 ==
3. 左右两边是否类型和值都相等 ===
4. 左右值不相等 !=
5. 左右两边是否不全等 !==;
** 逻辑运算符
***:1. 逻辑与 &&
2. 逻辑或 ||
3. 逻辑非 !;
** 三元运算符
*** 条件 ? 表达式1 ：表达式2
@endmindmap
```

## 2 分支语句
### 2.1 if 语句
```js
if(条件表达式) { 满足条件要执行的语句 }

if (条件表达式){ 
// 满足条件要执行的语句
} else { 
// 不满足条件要执行的语句
}

if (条件表达式) {
// 满足条件要执行的语句
} else if (条件表达式) { 
// 满足条件要执行的语句 
} else {
// 不满足条件要执行的语句
}
```

### 2.2 switch 语句
```js
switch (表达式) {
  case 值1: 
  // 代码1  
  break 
  case 值2: 
  // 代码2  
  break 
  // ...  
  default: 
  // 代码n 
  }
```

>[!note]+ 注意
>1.  switch case语句一般用于等值判断,必须是全等(\=\=\=)，if适合于区间判断
>2.  switch case一般需要配合break关键字使用，没有break会造成case穿透
>3.  if 多分支语句开发要比switch更重要，使用也更多