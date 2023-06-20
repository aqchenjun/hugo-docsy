---
categories: HTML 、CSS和JavaScript
title: setInterval()和setTimeout()
tags: [教程,前端,JavaScript]
date: 2023-06-15
lastmod: 2023-06-16
thumbnail: 
published: "true"
slug: c1zazyk
---

>《HTML 、CSS和JavaScript》读书笔记之十二

## 1 [setInterval 方法]({{< ref "/blog/js%20中实现程序暂停功能#2-利用-setInterval-和-clearInterval-函数" >}})
### 1.1 按每秒显示时间的变动
```js
// 显示 5 次
let num = 5;
const div = document.querySelector('div');
const timer = setInterval(function () {            
	div.innerHTML = new Date().toLocaleString();
	num--;
	if (num === 0) clearInterval(timer);
}, 1000);
```

### 1.2 倒计时
```js
const div = document.querySelector('div');
let n = 5;
const timer = setInterval(function () {
    div.innerHTML = n;
    n--;
    if (n === 0) clearInterval(timer)
}, 1000)
```

### 1.3 顺计时
```js
const div = document.querySelector('div');
let n = 1;
const timer = setInterval(function () {
    div.innerHTML = n;
    n++;
    if (n > 5) clearInterval(timer)
}, 1000)
```


## 2 [setTimeout 方法]({{< ref "/blog/web%20APIs：BOM%20操作#2-延时器函数：setTimeout(" >}})%20和clearTimeout())
### 2.1 按每秒显示时间的变动
```js
// 参数 n 表示显示的次数
const div = document.querySelector('div');
function getTime(n) {
	if (n === 0) return;
	div.innerHTML = new Date().toLocaleString();
	setTimeout(getTime, 1000, n-1);
};
```

### 2.2 倒计时
```js
// 参数 n 表示倒计时的起点
function fn(n) {
	if (n === 0) return;
	div.innerHTML = n;
	setTimeout(fn, 1000, n - 1);
};
```

### 2.3 顺计时
```js
// 参数 n 表示起点，m 表示终点
function fn(n, m) {
	if (n === m) return;
	setTimeout(function () {
		fn(n + 1, m);
		div.innerHTML = n;
	}, 1000)
};
```

>[!note]+ 提示
>- 由于 setInterval() 方法和 setTimeout() 方法均为异步执行，因此循环语句无效。
>- 回调函数的参数放在延迟时间的后面。