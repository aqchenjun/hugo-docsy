---
categories: JavaScript 入门到精通
title: web APIs：DOM获取及属性操作
tags: [教程,JavaScript]
date: 2023-05-04
lastmod: 2023-05-16
thumbnail: https://s1.vika.cn/space/2023/05/04/8528539db9f7494898a3c89ae22910c2
published: "true"
slug: qtsfu9r
---

>《JavaScript 入门到精通》读书笔记之六


## 1 ECMAScript 与 JavaScript

严格意义上讲，我们在 JavaScript 阶段学习的知识绝大部分属于 ECMAScript 的知识体系，ECMAScript 简称 ES， 它提供了一套语言标准规范，如变量、数据类型、表达式、语句、函数等语法规则都是由 ECMAScript 规定的。浏览器将 ECMAScript 大部分的规范加以实现，并且在此基础上又扩展一些实用的功能，这些被扩展出来的内容我们称为 Web APIs。
  ![guide.png](https://s1.vika.cn/space/2023/05/04/72293a5ba57c407aa2a3de682483eb3d)
ECMAScript 运行在浏览器中然后再结合 Web APIs 才是真正的 JavaScript，Web APIs 的核心是 DOM 和 BOM。
 
## 2 DOM  

DOM（Document Object Model）是将整个 HTML 文档的每一个标签元素视为一个对象，这个对象下包含了许多的属性和方法，通过操作这些属性或者调用这些方法实现对 HTML 的动态更新，为实现网页特效以及用户交互提供技术支撑。

### 2.1 DOM 树
  

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>标题</title>
</head>
<body>
  文本
  <a href="">链接名</a>
  <div id="" class="">文本</div>
</body>
</html>
```

如下图所示，将 HTML 文档以树状结构直观的表现出来，我们称之为文档树或 DOM 树，文档树直观的体现了标签与标签之间的关系。

![web-api.jpg](https://s1.vika.cn/space/2023/05/04/8528539db9f7494898a3c89ae22910c2)
  

### 2.2 DOM 节点  

节点是文档树的组成部分，每一个节点都是一个 DOM 对象，主要分为元素节点、属性节点、文本节点等。  

1. 【元素节点】其实就是 HTML 标签，如上图中 `head`、`div`、`body` 等都属于元素节点。

2. 【属性节点】是指 HTML 标签中的属性，如上图中 `a` 标签的 `href` 属性、`div` 标签的 `class` 属性。

3. 【文本节点】是指 HTML 标签的文字内容，如 `title` 标签中的文字。

4. 【根节点】特指 `html` 标签。

5. 其它...  

### 2.3 document  

`document` 是 JavaScript 内置的专门用于 DOM 的对象，该对象包含了若干的属性和方法，`document` 是学习 DOM 的核心。
  
## 3 获取DOM对象  

1. `document.querySelector()`   满足条件的第一个元素

2. `document.querySelectorAll()`  满足条件的元素集合，返回伪数组。该数组可以遍历。
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>  
</head>
<body>
  <ul>
    <li class="item">1</li>
    <li class="item">2</li>
    <li class="item">3</li>
  </ul>
  <script>
    const lis = document.querySelectorAll('.item')  
    console.log(lis)
    for (let i = 0; i < lis.length; i++) {
      lis[i].style.color = 'red'
    }
  </script>
</body>
</html>
```
  

## 4 操作元素内容

通过修改 DOM 的文本内容，动态改变网页的内容。  

1. `innerText` 将文本内容添加/更新到任意标签位置，文本中包含的标签不会被解析。    

2. `innerHTML` 将文本内容添加/更新到任意标签位置，文本中包含的标签会被解析。

## 5 操作元素属性 

有3种方式可以实现对属性的修改：  

### 5.1 常用属性修改  

直接通过属性名修改，最简洁的语法。  

### 5.2 控制样式属性  

通过修改行内样式 `style` 属性，实现对样式的动态修改。  

通过元素节点获得的 `style` 属性本身的数据类型也是对象，如 `box.style.color`、`box.style.width` 分别用来获取元素节点 CSS 样式的 `color` 和 `width` 的值。
  
任何标签都有 `style` 属性，通过 `style` 属性可以动态更改网页标签的样式，如要遇到 `css` 属性中包含字符 `-` 时，要将 `-` 去掉并将其后面的字母改成大写，如 `background-color` 要写成 `box.style.backgroundColor`。  

### 5.3 操作类名(className) 操作CSS  

如果修改的样式比较多，直接通过style属性修改比较繁琐，我们可以通过借助于css类名的形式。
示例：
`对象名.className = '新类名'`
  
>注意：
>>1.由于class是关键字, 所以使用className去代替。
>>
>>2.className是使用新值换旧值, 如果需要添加一个类,需要保留之前的类名

### 5.4 通过 classList 操作类控制CSS  

为了解决className 容易覆盖以前的类名，我们可以通过classList方式追加和删除\切换类名。

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        div {
            width: 200px;
            height: 200px;
            background-color: pink;
        } 
        .active {
            width: 300px;
            height: 300px;
            background-color: hotpink;
            margin-left: 100px;
        }
    </style>
</head>
<body>
    <div class="one"></div>
    <script>
        // 1.获取元素        
        let box = document.querySelector('div')
        // add是个方法 添加  追加
        // box.classList.add('active')
        // remove() 移除 类
        // box.classList.remove('one')
        // 切换类
        box.classList.toggle('one')
    </script>
</body>
</html>
```

## 6 操作表单元素属性 

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <input type="text" value="请输入">
    <button disabled>按钮</button>
    <input type="checkbox" name="" id="" class="agree">
    <script>
        // 1. 获取元素
        let input = document.querySelector('input')
        // 2. 取值或者设置值  得到input里面的值可以用 value

        // console.log(input.value)
        input.value = '小米手机'
        input.type = 'password'  

        // 2. 启用按钮
        let btn = document.querySelector('button')
        // disabled 不可用   =  false  这样可以让按钮启用
        btn.disabled = false
        // 3. 勾选复选框
        let checkbox = document.querySelector('.agree')
        checkbox.checked = false
    </script>
</body>
</html>
```
 

## 7 自定义属性

在标签上一律以data-开头，在DOM对象上一律以dataset对象方式获取。  

```html

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
   <div data-id="1"> 自定义属性 </div>
    <script>
        // 1. 获取元素
        let div = document.querySelector('div')
        // 2. 获取自定义属性值
         console.log(div.dataset.id)
    </script>
</body>
</html>
```

## 8 间歇函数
  
`setInterval` 是 JavaScript 中内置的函数，它的作用是间隔固定的时间自动重复执行另一个函数，也叫定时器函数。
### 8.1 间歇函数的应用实例一

在循环过程中实现暂停

### 8.2 间歇函数的应用实例二

钟表的实现：
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Clock</title>
    <style>
        .clock {
            background: url('images/clock.jpg');
            width: 600px;
            height: 600px;
            margin: auto;
        }  
        .hh,
        .mm,
        .ss {
            position: absolute;
            width: 100%;
            height: 100%;
            top: 0;
            left: 0;
        }  
        .hh {
            background: url('images/hour.png') no-repeat center;
        }  
        .mm {
            background: url('images/minute.png') no-repeat center;
        }  
        .ss {
            background: url('images/second.png') no-repeat center;
        }
    </style>
</head>
<body>
    <div class="clock">
        <div class="hh"></div>
        <div class="mm"></div>
        <div class="ss"></div>
    </div>
    <script>
        const second = document.querySelector('.ss');
        const minute = document.querySelector('.mm');
        const hour = document.querySelector('.hh');
        // 设置秒针每次旋转的度数
        let du = 6;
        let timer = setInterval(function () {
            second.style.transform = `rotate(${du}deg)`;
            minute.style.transform = `rotate(${du / 60}deg)`;
            hour.style.transform = `rotate(${du / 720}deg)`
            du += 6
        }, 10);
    </script>
</body>
</html>
```

图片资源如下：
clock.jpg:

![clock.jpg|400](https://s1.vika.cn/space/2023/05/04/574e6995c97d4a5a86ddabf41220e5a7)

hour.png:

![hour.png|20](https://s1.vika.cn/space/2023/05/04/874926b37d924c41ae699e143823f650)


minute.png：

![minute.png|20](https://s1.vika.cn/space/2023/05/04/b27eb797b10b4a41a38e2a673fdf6919)

second.png：

![second.png|20](https://s1.vika.cn/space/2023/05/04/deaa41bd4edc4eeb9ed0760d6991eed4)
