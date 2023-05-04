---
categories: HTML 、CSS和JavaScript
title: table 标签的常用 CSS
tags: [教程,前端,CSS]
date: 2023-04-28
lastmod: 2023-05-04
thumbnail: 
published: "true"
slug: 5rvq220
---

>《HTML 、CSS和JavaScript》读书笔记之六

```css
/* 设置表格的总宽、本身居中对齐及内部文本的对齐方式、表格边框线为单线 */
table {
		width: 600px;
		margin:auto;
		text-align: center;
		border-collapse: collapse;
        }
/* 设置边框线型 */
th,
td {
	border: 1px solid #ccc;	
}
/* 设置表格的标题 */
caption {
	font-size: 18px;
	margin-bottom: 10px;
	font-weight: 700;
}
/* 设置表格的行高及鼠标样式 */
tr {
	height: 40px;
	cursor: pointer;
}
/* 设置表格的第一行，也可以用 th 标签 */
table tr:nth-child(1) {
	background-color: #ddd;
}
/* 设置表格除第一行外的 hover 样式 */
table tr:not(:first-child):hover {
	background-color: #eee;
}
```



