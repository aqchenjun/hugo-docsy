---
categories: HTML 、CSS和JavaScript
title: js 中实现 sleep 的程序暂停功能
tags: [教程,前端,JavaScript]
date: 2023-04-29
lastmod: 2023-05-17
thumbnail: 
published: "true"
slug: 59kxwqm
---

>《HTML 、CSS和JavaScript》读书笔记之七

## 1 构建 sleep 函数
 使用async 和 await 的异步处理和返回Promise 等待完成，从而实现类似 sleep 的休眠功能。
 ```js
 /* 定义 sleep 函数，time 的单位是毫秒 */
 function sleep(time) {
		return new Promise(res => {
			setTimeout(res, time)
		})
	}
/* 调用 sleep 函数 */
async function fu(){
	await sleep(1000)
}
```

## 2 利用 setInterval 和 clearInterval 函数
setInterval 函数可以理解为一个以时间间隔为轮次的循环，有点类似 while 循环。终止循环采用 clearInterval 函数，有两种方法调用：
1. 在循环体语句中设置循环终止条件。
2. 设置单独的循环终止触发条件。
### 2.1 语法
```js
intervalID = setInterval(func, [delay, arg1, arg2, ...]);
```

**参数**
1. func。要重复调用的函数，每经过指定 delay 毫秒后执行一次。第一次调用发生在 delay 毫秒之后。
2. delay。每次延迟的毫秒数，函数的每次调用会在该延迟之后发生。如果未指定，则其默认值为 0。
3. arg1, ..., argN 。可选，当计时结束的时候，将被传递给 func 函数的附加参数。

**返回值：**

返回值 intervalID 是一个非零数值，用来标识通过 setInterval() 创建的定时器，这个值可以用来作为 clearInterval() 的参数来清除对应的定时器。

### 2.2 示例代码
该代码实现的功能：每隔一秒，在div 标签中更新数字，更新10次。
```js
const div = document.querySelector('div');
let i = 0;
let timer = setInterval(function () {
	i++;
	if (i > 10) {
		clearInterval(timer);
	} else {
		div.innerHTML = i;
	};
}, 1000)
```

## 3 利用 setTimeout 和clearTimeout 函数

### 3.1 语法
```js
let timer = setTimeout(回调函数, delay)
```

### 3.2 代码
运用了递归函数。

```html
const div = document.querySelector('div');
let i = 0;        
function setnum() {
	let timer = setTimeout(function () {
		i++;
		if (i <= 10) {
			div.innerHTML = i;
			setnum();
		}else{   /* 该语句此处可以省略 */
			clearTimeout(timer)
		};
	}, 1000);
};
setnum();
```