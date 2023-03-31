---
categories: HTML、CSS和JavaScript
title: 使用图标Font Awesome
tags: [教程,前端,html]
date: 2023-03-31
lastmod: 2023-03-31
thumbnail: https://s1.vika.cn/space/2023/03/31/0fe74b1f78324806a8022bd24a71a65d
published: "true"
slug: x9woewb
---

>《HTML 、CSS和JavaScript》读书笔记之三
  

## 1 引用方法  

```html
#引用方法一
<link href="//netdna.bootstrapcdn.com/font-awesome/4.7.0/css/font-awesome.min.css" rel="stylesheet">
#引用方法二：
#从 Font Awesome 网站上下载完整的压缩文件，解压后复制整个 font-awesome 文件夹到项目中的 static 目录。
<link rel="stylesheet" href="font-awesome/css/font-awesome.min.css">
```  

## 2 使用方法  

### 2.1 一般方法  

```html
<i class="fa fa-camera-retro"></i> text
<span class="fa fa-flag"><span>text
```  

### 2.2 图标放大 

使用 fa-lg (33%递增)、fa-2x、 fa-3x、fa-4x、 fa-5x 类 来放大图标。  

```html
<i class="fa fa-camera-retro fa-lg"></i>text
```  

### 2.3 用于列表符号  

使用 fa-ul 和 fa-li 类便可以简单的将无序列表的默认符号替换掉。  

```html
<ul class="fa-ul">  
	<li><i class="fa-li fa fa-check-square"></i>List icons</li>
	<li><i class="fa-li fa fa-check-square"></i>can be used</li>
	<li><i class="fa-li fa fa-spinner fa-spin"></i>as bullets</li>
	<li><i class="fa-li fa fa-square"></i>in lists</li>
</ul>
```  

### 2.4 旋转与翻转  

使用 fa-rotate-和 fa-flip-类可以对图标进行任意旋转和翻转。  

```html
<i class="fa fa-shield"></i> normal<br>
<i class="fa fa-shield fa-rotate-90"></i> fa-rotate-90<br>
<i class="fa fa-shield fa-rotate-180"></i> fa-rotate-180<br>
<i class="fa fa-shield fa-rotate-270"></i> fa-rotate-270<br>
<i class="fa fa-shield fa-flip-horizontal"></i> fa-flip-horizontal<br>
<i class="fa fa-shield fa-flip-vertical"></i> icon-flip-vertical
```  

### 2.5 加边框和对齐

使用 fa-border 以及 pull-right 或 pull-left 类。  

```html
<i class="fa fa-quote-left fa-3x pull-left fa-border"></i>
```  

### 2.6 固定宽度 

使用 fa-fw 可以将图标设置为一个固定宽度。主要用于不同宽度图标无法对齐的情况。  

```html
<div class="list-group">
	<a class="list-group-item" href="#">
		<i class="fa fa-home fa-fw"></i>&nbsp; Home
	</a>
	<a class="list-group-item" href="#">
		<i class="fa fa-book fa-fw"></i>&nbsp; Library
	</a>
	<a class="list-group-item" href="#">
		<i class="fa fa-pencil fa-fw"></i>&nbsp; App
	</a>  
	<a class="list-group-item" href="#">
		<i class="fa fa-cog fa-fw"></i>&nbsp; Settings
	</a>
</div>
```





