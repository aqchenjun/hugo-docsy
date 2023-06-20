---
categories: JavaScript 入门到精通
title: JavaScript 进阶：数组和字符串的常用方法
tags: [教程,JavaScript]
date: 2023-05-29
lastmod: 2023-06-04
thumbnail: 
published: "true"
slug: iewfkpv
---

>《JavaScript 入门到精通》读书笔记之十四


## 1 数组
### 1.1 常用方法一览表

#### 1.1.1 不以回调函数为参数的方法
| 实例方法   | 功能                             | 参数                      | 返回值           |
| ---------- | -------------------------------- | ------------------------- | ---------------- |
| push()     | 将指定的元素添加到数组的末尾     | elementN                  | 新数组的长度     |
| unshift()  | 将指定元素添加到数组的开头       | elementN                  | 新数组的长度     |
| pop()      | 从数组中删除最后一个元素         | -                         | 删除的元素       |
| shift()    | 从数组中删除第一个元素           | -                         | 删除的元素       |
| splice()   | 在数组中插入或移除元素           | start, deleteCount, itemN | 新数组           |
| slice()    | 数组的切片(浅拷贝)               | start,end                 | 新数组           |
| join()     | 将数组的所有元素连接成一个字符串 | 连接符                    | 连接成的新字符串 |
| includes() | 判断一个数组是否包含一个指定的值    |         searchElement, fromIndex  |      true 或 false            |

#### 1.1.2 以回调函数为参数的方法
| 实例方法  | 功能                                 | 参数       | 回调函数的参数      | 返回值                    |
| --------- | ------------------------------------ | ---------- | ------------------- | ------------------------- |
| forEach() | 对数组的每个元素执行一次给定的函数   | callbackFn | element,index,array | -                         |
| map()     | 原数组中的每个元素都调用一次回调函数 | callbackFn | element,index,array | 新数组                    |
| filter()  | 对数组进行筛选过滤                   | callbackFn | element,index,array | 新数组                    |
| find()    | 在数组中查找元素                     | callbackFn | element,index,array | 查找到的元素或`undefined` |
| every()   | 检测数组中的所有元素是否满足条件     | callbackFn | element,index,array | true 或 false             |
| some()    | 检测数组中是否至少有一个元素满足条件   |    callbackFn  | element,index,array |true 或 false  |

##### 1.1.2.1 回调函数的参数
1. element：数组中当前正在处理的元素。
2. index：正在处理的元素在数组中的索引。
3. array：调用该方法的数组本身。

{{% alert title=注意 color=success %}}参数的顺序是固定的。如果只有一个参数，就是 element，如果是两个参数，就是 element 和 index。

 {{% /alert %}}##### 1.1.2.2 实例
```js
const arr = [1, 4, 7, 32, 56, 43];

// 要求对每个元素加1
// forEach() 方法
arr.forEach(item => item + 1);
// map() 方法
const newArr = arr.map(item => item +1)

// 求每个元素与其索引号的和
// forEach() 方法
arr.forEach((item,index) => item + index);
// map() 方法
const newArr = arr.map((item,index) => item +index)

// 寻找大于20的数
const newArr = arr.filter(item => item >20)
const resItem = arr.find(item => item >20)
const result = arr.every(item => item >20)

// 寻找质数
function isPrime(num) {
            start = 2;
            while (num > 1 && num % start !== 0) {
                start++
            }
            if (num === start) {
                return true
            } else {
                return false
            }
        }
const newArr = arr.filter(isPrime)
const resItem = arr.find(isPrime)
const result = arr.every(isPrime)
```

{{% alert title=提示 color=success %}}当回调函数的参数是函数时，函数的形参依次是 element、index 、array

 {{% /alert %}}#### 1.1.3 其他方法
| 实例方法 | 功能     | 参数                     | 返回值 |
| -------- | -------- | ------------------------ | ------ |
| reduce() | 累计汇总 | callbackFn, initialValue | value  |
| sort()         |    排序    |       compareFn          |    -    |

##### 1.1.3.1 参数
1. callbackFn：回调函数，其参数包括：
	1. pre：上一次调用 callbackFn 的结果。在第一次调用时，如果指定了 initialValue 则为指定的值，否则为 array[0] 的值。
	2. curr：当前元素的值。在第一次调用时，如果指定了 initialValue，则为 array[0] 的值，否则为 array[1]。
	3. currIndex：当前的索引位置。在第一次调用时，如果指定了 initialValue 则为 0，否则为 1。
	4. array：调用了 reduce() 的数组本身。
2. initialValue：初始值
3. compareFn：定义排序顺序的函数。返回值应该是一个数字，其正负性表示两个元素的相对顺序。该函数使用以下参数调用：
	1. a：第一个用于比较的元素。不会是 undefined。
	2. b：第二个用于比较的元素。不会是 undefined。
	3. 如果省略该函数，数组元素会被转换为字符串，然后根据每个字符的 Unicode 码位值进行排序。

##### 1.1.3.2 返回值
reduce() 方法返回使用“reducer”回调函数遍历整个数组后的结果。
sort() 方法直接对原数组进行修改，无返回值。

##### 1.1.3.3 示例
###### 1.1.3.3.1 对数组求和
```js
const arr=[1,3,4,6,2];
const sum=arr.reduce((pre,curr)=>pre+curr,0)
```

###### 1.1.3.3.2 统计数组中值的出现次数
```js
function count(item, arr) {
	const res = arr.reduce((pre, curr) => {
		if (curr === item) pre++
		return pre
	}, 0)
	return res
}

// 用 forEach 实现
function count(item, arr) {
	let count = 0;
	arr.forEach(ele => {
		if (item === ele) count++
	});
	return count
};
```

##### 1.1.3.4 数组排序
```js
const stringArray = ["Blue", "Humpback", "Beluga"];
const numberArray = [40, 1, 5, 200];
stringArray.sort() // ['Beluga', 'Blue', 'Humpback']
numberArray.sort() // [1, 200, 40, 5]
numArr.sort((a, b) => b - a) // [200, 40, 5, 1]
strArr.sort((a, b) => a.length - b.length) //  ['Blue', 'Beluga', 'Humpback']
```

### 1.2 数组的常用计算
#### 1.2.1 最大值和最小值
```js
const arr=[1,3,4,6,2];
console.log(Math.max(...arr))
console.log(Math.min(...arr))
```

#### 1.2.2 数组求和
##### 1.2.2.1 forEach() 方法
```js
const arr=[1,3,4,6,2];
let sum = 0;
arr.forEach(item => {
	sum += item;	
})
return sum
```

##### 1.2.2.2 [reduce 方法]({{< ref "/blog/JavaScript%20进阶：数组和字符串的常用方法#2.3.3.1-对数组求和" >}})

##### 1.2.2.3 join()+eval() 方法
```js
const arr=[1,3,4,6,2];
const sum = eval(arr.join('+'));
```

## 2 字符串

### 2.1 字符串的常用方法一览表
| 实例方法      | 功能                                                     | 返回值           |
| ------------- | -------------------------------------------------------- | ---------------- |
| [split 方法]({{< ref "/blog/web%20APIs：BOM%20操作#4.3-split(" >}})%20方法)       | 使用指定的分隔符字符串将一个String对象分割成子字符串数组 | 数组             |
| slice()   | 字符串切片                                               | 子字符串         |
| startsWith()  | 判断当前字符串是否以另外一个给定的子字符串开头           | true 或 false    |
| endsWith()    | 判断当前字符串是否是以另外一个给定的子字符串结尾的       | true 或 false    |
| includes()    | 判断一个字符串是否包含在另一个字符串中                   | true 或 false    |
| toUpperCase() | 转化为大写                                               | 大写的字符串     |
| toLowerCase() | 转化为小写                                               | 小写的字符串     |
| [replace 方法]({{< ref "/blog/web%20APIs：正则表达式#2.6-replace(" >}})%20方法)  、replaceAll()   | 替换                                                     | 替换后的新字符串 |
|  [match 方法]({{< ref "/blog/web%20APIs：正则表达式#2.3-match()-方法)、[matchAll 方法](web-APIs：正则表达式.md#2.4-matchAll(" >}})%20方法)     |      根据正则表达式查找匹配结果       |    匹配的结果              |

### 2.2 slice() 方法
提取某个字符串的一部分，并返回一个新的字符串，且不会改动原字符串。

#### 2.2.1 语法
`str.slice(beginIndex[, endIndex])`

#### 2.2.2 参数
beginIndex：从该索引（以 0 为基数）处开始提取原字符串中的字符。如果值为负数，表示从字符串尾部计数。

endIndex：可选。在该索引（以 0 为基数）处结束提取字符串。如果省略该参数，`slice()` 会一直提取到字符串末尾。

#### 2.2.3 返回值
从原字符串中提取出来的新字符串。


## 3 字符串与数组的关系
某种意义来说，字符串是一种只读数组。

### 3.1 共同的属性和方法：
length属性、slice()方法、includes()方法和 `[ ]` 方法。

### 3.2 字符串转数组
```js
const str = 'abvdefr';
arr = Array.from(str);  // ['a', 'b', 'v', 'd', 'e', 'f', 'r']
arr = [...str]   // ['a', 'b', 'v', 'd', 'e', 'f', 'r']
arr = str.split()  // ['abvdefr']
```

### 3.3 数组转字符串
`str = arr.join(separator)`

