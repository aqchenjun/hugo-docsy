---
categories: HTML 、CSS和JavaScript
title: CSS 的背景样式 background
tags: [教程,前端,CSS]
date: 2023-04-30
lastmod: 2023-05-04
thumbnail: 
published: "true"
slug: 1xa0ww9
---

>《HTML 、CSS和JavaScript》读书笔记之八

CSS 中提供了一系列用于设置 HTML 元素背景效果的属性，如下所示：
1. background-color：设置元素的背景颜色；
2. background-image：设置元素的背景图像；
3. background-repeat：控制背景图像是否重复；
4. background-attachment：控制背景图像是否跟随窗口滚动；
5. background-position：控制背景图像在元素中的位置；
6. background-size：设置背景图像的尺寸；
7. background-origin：设置 background-position 属性相对于什么位置来定位背景图像；
8. background-clip：设置背景图像的显示区域；
9. background：背景属性的缩写，可以在一个声明中设置所有的背景属性。

## 1 background-color
元素的背景色，属性的值为颜色值或关键字"transparent"二者选其一。
```css
background-color: red;
background-color: #bbff00;
background-color: rgb(255, 255, 128);
background-color: transparent;
background-color: inherit;
```

## 2 background-image
```css
background-image:url("mdn_logo_only_color.png");
```

## 3 background-repeat
默认情况下背景图像会从元素内容的左上角开始（若有内边距则从元素内边距区域的左上角开始），在水平和垂直方向上重复背景图像以填充整个元素区域（不包括元素的外边距区域）， background-repeat 属性用来设置背景图像是否重复或如何重复，该属性的可选值如下：
>1. repeat：默认值，背景图像将在垂直方向和水平方向上重复
>2. repeat-x：背景图像仅在水平方向上重复
>3. repeat-y：背景图像仅在垂直方向上重复
>4. no-repeat：背景图像仅显示一次，不在任何方向上重复
>5. inherit：从父元素继承 background-repeat 属性的设置

## 4 background-size


1. xpos ypos：使用像素（px）或其它 CSS 单位来设置背景图像的高度和宽度，xpos 表示宽度，ypos 表示高度，如果只设置第一个值，那么第二个值将被设置为默认值 auto（自动）

2. x% y%：使用百分比表示背景图像相对于所在元素宽度与高度的百分比，x% 表示宽度，y% 表示高度，如果只设置第一个值，那么第二个值将被设置为默认值 auto（自动）

3. cover：保持背景图像的横纵比例并将图像缩放至足够大，使背景图像可以完全覆盖元素所在的区域，这么做可能会导致背景图像的某些部分超出元素区域而无法显示

4. contain：保持背景图像的横纵比例并将图像缩放至足够大，使背景图像可以完整的显示在元素所在区域，背景图像可能无法完全覆盖整个元素区域
