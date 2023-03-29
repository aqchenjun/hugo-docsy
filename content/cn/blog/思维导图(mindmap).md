---
categories: plantUML 语言参考指引
title: 思维导图(mindmap)
tags: [ 教程]
date: 2023-03-04
lastmod: 2023-03-27 
thumbnail: https://thumbsnap.com/i/YK8syLtd.jpg
published: "true"
slug: 20230221141023
---
# 《plantUML 语言参考指引》读书笔记之二

## 1 开始和结束

`@startmindmap`开始

`@endmindmap`结束

## 2 子主题

```plantuml
@startmindmap
* 主题
** 一级子主题
*** 二级子主题一
*** 二级子主题二
*** 二级子主题三
left side
** 左侧一级子主题
@endmindmap
```

## 3 内容多行表示

使用`:`开始多行内容，`;`结束多行内容。目前如果要使用`:` `;`只能用`*`Markdown语法新建分支

```plantuml
@startmindmap
* 主题
** 一级子主题
***: 二级子主题一
example1
example2;
@endmindmap
```

## 4 插入图片

```
<img:file://logo3.png>
<img:http://plantuml.com/logo3.png>
```

## 5 设置颜色

### 5.1 全局设置

#### 5.1.1 skinparam 命令

相关的参数如下：

```
NodeBackgroundColor
NodeBorderColor
NodeFontColor
NodeFontName
NodeFontSize
NodeFontStyle
ArrowThickness
ArrowColor
```

用法：

```plantuml
@startmindmap
skinparam ArrowColor orchid
skinparam ArrowThickness 2
skinparam NodeBackgroundColor lemonchiffon
skinparam NodeBorderColor orchid
skinparam NodeFontColor black
* root node
** some first level node
*** second level node
*** another second level node
** another first level node
@endmindmap
```

#### 5.1.2 建立样式

```plantuml
@startmindmap
<style>
	mindmapDiagram {
		.green {
						BackgroundColor lightgreen
		}
		.rose {
						BackgroundColor #FFBBCC
		}
		.your_style_name {
						BackgroundColor lightblue
		}
	}
</style>
* root node
** some first level node <<green>>
*** second level node <<rose>>
*** another second level node <<your_style_name>>
** another first level node <<green>>
@endmindmap
```

### 5.2 单独设置

```plantuml
@startmindmap
*[#Orange] root node
**[#lightgreen] some first level node
***[#FFBBCC] second level node
***[#lightblue] another second level node
**[#lightgreen] another first level node
@endmindmap
```

## 6 去除外边框

在节点前加上 _ 以去除外边框

```plantuml
@startmindmap
skinparam ArrowColor orchid
skinparam ArrowThickness 2
skinparam NodeBackgroundColor lemonchiffon
skinparam NodeBorderColor orchid
skinparam NodeFontColor black
* root node
** some first level node
***_ second level node
*** another second level node
**_ another first level node
@endmindmap
```

## 7 添加图标

语法：<&ICON_NAME>

```plantuml
@startmindmap
* <size:20>first</size><color:red><size:20><&heart>
@endmindmap
```

## 8 使用列表

```plantuml
@startmindmap
* root
** d1
**:**test list 1**
* Bullet list
* Second item
** Sub item
*** Sub sub item
* Third item
----
**test list 2**
# Numbered list
# Second item
## Sub item
## Another sub item
# Third item;
@endmindmap
```
