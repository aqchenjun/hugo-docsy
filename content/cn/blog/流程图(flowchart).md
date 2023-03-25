---
source: Mermaid 语法
title: 流程图(flowchart)
tags: [ 教程]
thumbnail: https://thumbsnap.com/i/7mHPUyMf.jpg
date: 2023-02-15
lastmod: 2023-03-25 
published: "true"
---
## 1 方向

- TB - top to bottom
- TD - top-down/ same as top to bottom
- BT - bottom to top
- RL - right to left
- LR - left to right

## 2 节点形状

```mermaid
flowchart TB
		A(A:这是圆角矩形)
		B([B:这是运动场矩形])
		C((C:这是圆形))
		D{D:这是菱形}
		E[E:这是方角矩形]
```

## 3 连接线

```mermaid
flowchart TB
		A-->B
		C---D
		E--text-->F
		G-.->H
		I-.text.-J
		K==>L
		M==text==>N
		O--text-->P--text-->Q
```

连接线的长度和形状：

| Length | 1 | 2 | 3 |
| --- | --- | --- | --- |
| Normal | --- | ---- | ----- |
| Normal with arrow | --> | ---> | ----> |
| Thick | === | ==== | ===== |
| Thick with arrow | ==> | ===> | ====> |
| Dotted | -.- | -..- | -...- |
| Dotted with arrow | -.-> | -..-> | -...-> |

## 4 特殊符号

如果文本中有括号等特殊符号，文本应加上引号。特殊符号使用HTML名称

```mermaid
flowchart LR
    id1["This is the (text) in the box"]
		A["A double quote:#quot;"] -->B["A dec char:#165;"]
```

## 5 subgraph及其方向

语法格式：

subgraph title
    graph definition
end

方向用关键词 direction 设置。但有时因冲突而无效。

```mermaid
flowchart TB
c1-->one
subgraph one
		direction LR
		a2-->a1
end
subgraph two
		direction LR
		b1-->b2
end
subgraph three
		direction TB
		c1-->c2
end
```

## 6 注释

由
```mermaid
flowchart LR
		classDef someclass fill:#f96
   A -- text --> B -- text2 --> C
```

## 7 样式

```mermaid
flowchart TB
		linkStyle default fill:blue,stroke:orchid,stroke-width:2px,color:red;	
		classDef default fill: lemonchiffon, stroke: orchid, stroke-width: 1px;	
		classDef green fill:#9f6,stroke:#333,stroke-width:2px;	
		A--text-->B(text123)
		class B green
```

```mermaid
flowchart TD
    B["fab:fa-twitter for peace"]
    B-->C[fa:fa-ban forbidden]
    B-->D(fa:fa-spinner)
    B-->E(A fa:fa-camera-retro perhaps?)
```