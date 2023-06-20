---
categories: JavaScript 入门到精通
title: JavaScript 进阶：深浅拷贝、异常处理和 this
tags: [教程,JavaScript]
date: 2023-06-16
lastmod: 2023-06-19
thumbnail: 
published: "true"
slug: oj1njpl
---

>《JavaScript 入门到精通》读书笔记之十七



## 1 思维导图
```plantuml
@startmindmap Mindmap
skinparam ArrowColor orchid
skinparam ArrowThickness 2
skinparam NodeBackgroundColor lemonchiffon
skinparam NodeBorderColor orchid
* 对象复制
**_ 简单数据类型
*** 直接赋值
**_ 引用数据类型
*** 一维数组：浅拷贝
**** Array.prototype.concat()、[...arr]
*** 单层对象：浅拷贝
**** Object.assgin() 、{...obj}
*** 深拷贝
**** 用递归函数实现
**** lodash/cloneDeep
**** JSON 对象
* 异常处理
** throw：会中断程序
** try {} catch(e){}：捕获异常
** debugger：断点调试
* this 的处理
** this 的指向
***_ 普通函数
**** 谁调用，指向谁
**** 无调用者指向 window
***_ 箭头函数
**** 没有 this，沿用上一级的，上一级没有继续往上找
** 改变指向
*** apply：fun.apply(this, [arg1, arg2,......])
*** bind：fun.bind(this, arg1, arg2,......) ，不立即执行
@endmindmap
```


## 2 深拷贝(JSON 方法)

第一步：JSON.stringify(obj)，将 js 对象转换为 JSON 字符串；
第二步：JSON.parse(str)，将 JSON 字符串转换为 js 对象
```js
const obj = {
	name:'tom',
	family:{
		baby:'Marry'
	},
	color:['red','black'],
}
const newObj = JSON.parse(JSON.stringify(obj))
```

## 3 this 的处理示例
下面代码实现的功能：点击 button 按钮后，该按钮不可用，2秒种后恢复可用。
```html
<button>点击</button>
<script>
	const btn = document.querySelector('button');
	btn.addEventListener('click', function () {
		this.disabled = true;
		setTimeout(function(){                
			this.disabled = false
		}.bind(this), 2000);
	});
</script>
```

- 第一个 this，指向 btn 按钮
- 第二个 this，原本指向 window，由于 bind()方法修改了指向，因此指向 btn 按钮
- 第三个 this，由于依然在 btn 按钮点击事件的回调函数的语句中，因此与第一个 this 指向相同

>[!note]+ 提示
>bind() 方法可以接在函数名的后面，也可以接在匿名函数 function(){} 的后面