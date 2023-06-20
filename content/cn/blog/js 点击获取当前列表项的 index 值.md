---
categories: HTML 、CSS和JavaScript
title: js 点击获取当前列表项的 index 值
tags: [教程,前端,JavaScript]
date: 2023-05-18
lastmod: 2023-05-19
thumbnail: 
published: "true"
slug: 249xcel
---

>《HTML 、CSS和JavaScript》读书笔记之十一

## 1 列表 li

### 1.1 实现的功能
点击某个 li，能得到该 li 在列表项中的 index

### 1.2 准备工作
在 li 标签中添加 data-id 属性，其值为该 li 标签在列表项中的 index。
除了手动添加外，还可以用 js 添加。思路是：
1. 获取 li 标签父节点的所有子元素的长度 n
2. 创建 li 标签，设置自定义属性 data-id 为 n
3. 将 li 标签追加到 ul 标签中。

代码：
```js
const ul = document.querySelector('ul');
const n = ul.children.length;
const li = document.createElement('li');
li.dataset.id = `${n}`;
ul.appendChild(li);
```

### 1.3 点击获取
利用事件委托
```js
ul.addEventListener('click', function (e) {
	console.dir(e.target.dataset.id)
});
```

## 2 表格 table
表格中，每行的 rowIndex 属性表示元素所在行相对于整个 table 的逻辑位置，第一行的 index =1，依次类推。
```js
 table.addEventListener('click', function (event) {
	if (event.target.className === "trash") {		
		console.log(event.target.parentNode.parentNode.rowIndex - 1)		
	}
});
```






