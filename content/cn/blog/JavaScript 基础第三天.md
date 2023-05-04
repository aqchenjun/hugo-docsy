---
categories: JavaScript 入门到精通
title: JavaScript 基础第三天
tags: [教程,JavsScript]
date: 2023-04-23
lastmod: 2023-04-25
thumbnail: 
published: "true"
slug: 78uvc7u
---

>《JavaScript 入门到精通》读书笔记之三

## 1 思维导图
### 1.1 循环
```plantuml
@startmindmap Mindmap
skinparam ArrowColor orchid
skinparam ArrowThickness 2
skinparam NodeBackgroundColor lemonchiffon
skinparam NodeBorderColor orchid
* 循环
**_ 循环三要素
***:1. 初始值
2. 终止条件
3. 变量的变化量;
**_ while 循环
***_ 语法
****: while (条件表达式) {
	     // 循环体  
};
***_ 适用条件
**** 循环的次数不确定时候使用
**_ for 循环
***_ 语法
****:for(起始值; 终止条件; 变化量) {
  // 循环体
  };
***_ 适用条件
**** 循环的次数确定时候使用
**_ 终止循环
***_ break
**** 退出整个循环
***_ continue
**** 退出本次循环，继续下一次循环
@endmindmap
```

### 1.2 数组
```plantuml
@startmindmap Mindmap
skinparam ArrowColor orchid
skinparam ArrowThickness 2
skinparam NodeBackgroundColor lemonchiffon
skinparam NodeBorderColor orchid
* 数组
**_ 查
*** 数组名[索引]
**_ 改
*** 数组名[索引] = 新值
**_ 增
***_ 增加到数组尾
****:数组名.push(item1,item2,...)
----
返回值：新的 length 属性值;
***_ 增加到数组头
****:数组名.unshift(item1,item2,...)
----
返回值：新的 length 属性值;
***_ 插入到数组任意位置
****:数组名.splice(start, deleteCount, item1, item2,...)
----
注意：如果仅插入数据，deleteCount=0;
**_ 删
***_ 删除末尾数据
****:数组名.pop()
返回值：删除的元素;
***_ 删除开头数据
****:数组名.shift()
返回值：删除的元素;
***_ 删除数组任意数据
**** 数组名.splice(start, deleteCount)
** 遍历
*** 循环
@endmindmap
```

## 2 综合案例
渲染柱状图：
![image.png](https://s1.vika.cn/space/2023/04/23/53843a77499442759404af9fc65b0d0d)

利用 BootStrap 的栅格布局，标注数字和季度名称的位置则通过 position 和 margin 属性值来调整。
代码
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@4.6.2/dist/css/bootstrap.min.css">
    <script src="https://cdn.jsdelivr.net/npm/jquery@3.5.1/dist/jquery.slim.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@4.6.2/dist/js/bootstrap.bundle.min.js"></script>
    <title>Document</title>
    <style>
        /* 柱状图的背景色及宽度 */
        .box {
            background-color: pink;
            width: 40px;
        }
        /* 数字标注 */
        span {
            position: absolute;
            margin-top: -20px;
            margin-left: 5px;
        }
        /* 坐标轴的轴线及高度 */
        #bar {
            border-left: 1px solid black;
            border-bottom: 1px solid black;
            height: 200px;
        }
    </style>
</head>

<body>
    <div class="container mt-5">
        <div class="row align-items-end" id="bar">

        </div>
    </div> 

    <script>
        let season = [];
        let chinese = ['一', '二', '三', '四'];
        for (let i = 0; i < 4; i++) {
            let num = Number(prompt(`输入第${chinese[0]}季度：`));
            season.push(num);
        }
        let str = ``;
        for (let i = 0; i < 4; i++) {
            str += `<div class="col">
                <div class="row justify-content-center">
                    <div class="box" style="height:${season[i]}px">
                        <span>${season[i]}</span>
                        <div style="position:absolute;margin-top:${season[i]}px">第${chinese[i]}季度</div>
                    </div>
                </div>
            </div>`
        }
        document.getElementById('bar').innerHTML = str;  
    </script>
</body>
</html>
```



