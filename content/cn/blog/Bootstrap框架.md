---
categories: HTML、CSS和JavaScript
title: Bootstrap框架
tags: [教程,前端,bootstrap]
date: 2023-03-31
lastmod: 2023-03-31
thumbnail: https://s1.vika.cn/space/2023/03/31/2f3bea52b74e4a568b0b9620701d7f57
published: "true"
slug: xeljerv
---

>《HTML 、CSS和JavaScript》读书笔记之


## 1 关于垂直对齐和水平对齐  

### 1.1 垂直对齐  

### 1.2 子标签垂直对齐  

定义子标签垂直对齐的类包括.align-items-start、.align-items-end、.align-items-center、.align-items-baseline 和 .align-items-stretch。
{{% alert title=前提 color=success %}}该子标签的父元素是行，而且要设置其高度大于子标签。  

 {{% /alert %}}```html
<div class="container"> 
        <div class="row bg-secondary align-items-center" style="height:100px">
                <div class="col-2" style="height:50px"> 
                        <div class="row align-items-center" style="height:50px">显示内容
                        </div>
                </div>
        </div>
</div>
```

{{% alert title=注释 color=success %}} 上述代码中，第2行 align-items-center 定义子标签 .col-2 的垂直对齐方式，第4行 align-items-center 定义 “显示内容” 的垂直对齐方式。  

 {{% /alert %}}### 1.3 自身垂直对齐  

定义自身垂直对齐的类包括 .align-self-start、.align-self-end、.align-self-center、.align-self-baseline 和 .align-self-stretch，
{{% alert title=前提 color=success %}}该标签的父元素是行，而且要设置其高度大于该标签。  

 {{% /alert %}}```html
<div class="row bg-secondary align-items-center" style="height:100px">
    <div class="col-1 border p-0">Flex item</div>
    <div class="col-2 border p-0 align-self-start">Flex item</div>
    <div class="col-3 border p-0">Flex item</div>
</div>
```  

{{% alert title=注释 color=success %}} 上述代码中，第1行的 align-items-center 定义了子标签的垂直对齐方式，当第二个子标签 .col-2 有 align-self-start，因此，第一个和第三个 div 按照 align-items-center 来显示，而第二个 div 则按照 align-self-start 显示。
 align-items- 和 align-self- 的区别：align-items- 可以对子标签统一进行批量设置，而 align-self- 只能对单个标签进行设置。  

 {{% /alert %}}### 1.4 水平对齐  

定义子标签水平对齐方式的类包括：.justify-content-start、.justify-content-end、.justify-content-center、.justify-content-between、.justify-content-around。
{{% alert title=前提 color=success %}}该子标签的父元素是行，而且其宽度要大于所有的子标签宽度之和。  

 {{% /alert %}}```html
<div class="container">
    <div class="row bg-secondary justify-content-end" style="width:800px">
        <div class="col-2">Flex item 1</div>
        <div class="col-2">Flex item 2</div>
        <div class="col-2">Flex item 3</div>
    </div>
</div>
```  

### 1.5 顶部、底部对齐和左边、右边对齐  

1. 顶部对齐的类：mb-auto
2. 底部对齐的类：mt-auto
3. 靠左对齐的类：mr-auto
4. 靠右对齐的类：ml-auto  

其对父元素的要求与前面1、2相同。  

```html
<div class="row bg-secondary flex-column mt-3" style="height:100px">
    <div class="col-2">Flex item 1</div>
    <div class="col-2">Flex item 2</div>
    <div class="col-2 mt-auto">Flex item 3</div>
    #该标签底部对齐
</div>
```  

{{% alert title=注释 color=success %}} flex-column 的作用是将本来水平排列的标签转为垂直排列。  

 {{% /alert %}}## 2 关于折叠  

### 2.1 一般折叠  

#### 2.1.1 第一步
使用 button 标签或 a 标签。button 标签需要设置属性 data-toggle=“collapse” 和 data-target=“#标签id”或data-target=“.类名”，“标签id” 、“类名”即为折叠显示的标签id或类。 a 标签需要设置属性 data-toggle=“collapse” 和 href=“#标签id”。  

```html
<a class="btn btn-primary" data-toggle="collapse" href="#collapse1">Link href</a>
<button class="btn btn-secondary" data-toggle="collapse" data-target="#collapse2">Button with data-target</button>
```  

#### 2.1.2 第二步
设置需要折叠显示的内容这部分内容需要设置 class=“collapse”，id=‘标签名’。如果需要多个标签同时折叠显示，则应将这些标签设为同一类，然后在 class 中添加，用 button 调用。  

```html
<div class="collapse" id="collapse1">
    Some placeholder content for the collapse component.</div>
<div class="collapse" id="collapse2">
    abd
</div>
```  

### 2.2 手风琴折叠  

有多个需要折叠显示的内容，并且每次只显示一项内容。  

第一步，将这些内容打包成一个 div ，其class=‘accordion’，同时设置 id。
  

```html
<div class="accordion" id="accordionExample"> </div>
```  

 第二步，每项内容打包成一个 card。‘card-header’ 放置 button 标签  

```html
<div class="card-header">
       <h2 class="mb-0">
             <button class="btn btn-link btn-block text-left" type="button" data-toggle="collapse" data-target="#collapseOne">
                   Collapsible Group Item #1
             </button>
       </h2>
 </div>
```  

 第三步，在 card 标签里设置需要折叠显示的内容。这部分内容需要设置 class=“collapse”，id=‘标签名’，data-parent=手风琴标签名。class 中的 show 表示初始时显示。  

```html
<div id="collapseOne" class="collapse show" data-parent="#accordionExample">
      <div class="card-body">
        Some placeholder content for the first accordion panel.
      </div>
</div>
```
  

```html
<div class="accordion" id="accordionExample">
    <div class="card">
        <div class="card-header">
              <h2 class="mb-0">
                    <button class="btn btn-link btn-block text-left" type="button" data-toggle="collapse" data-target="#collapseOne">
                          Collapsible Group Item #1
                    </button>
              </h2>
        </div>
        <div id="collapseOne" class="collapse show" data-parent="#accordionExample">
              <div class="card-body">
                    Some placeholder content for the first accordion panel.
              </div>
        </div>
    </div>
    <div class="card">
        ...
    </div>
</div>
```