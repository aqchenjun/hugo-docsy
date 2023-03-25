---
source: plantUML 语言参考指引
title: 活动图(activity diagram)
tags: [ 教程]
date: 2023-03-04
lastmod: 2023-03-25 
thumbnail: https://thumbsnap.com/i/jNZcWa5N.jpg
published: "true"
---

## 1 开始/结束

使用关键字 @startuml 和 @enduml 表示开始和结束。

使用关键字 start 和 end(stop) 表示子图的开始和结束。

## 2 顺序结构

以冒号开始，以分号结束。用法：

:文字内容;

## 3 条件语句

### 3.1 if-then-else-endif 结构

```plantuml
@startuml
start
    if (Graphviz installed?) then (yes)
        :process all\ndiagrams;
    else (no)
        :process only
        __sequence__ and __activity__ diagrams;
    endif
end
@enduml
```

### 3.2 if-then-elif-else-endif 结构

```plantuml
@startuml
start
    if (condition A) then (yes)
        :Text 1;
    elseif (condition B) then (yes)
        :Text 2;
    stop
    elseif (condition C) then (yes)
        :Text 3;
    elseif (condition D) then (yes)
        :Text 4;
    else (nothing)
        :Text else;
    endif
stop
@enduml
```

## 4 循环结构

### 4.1 repeat-repeatwhile

```plantuml
@startuml
start
    repeat
        :read data;
        :generate diagrams;
    repeat while (more data?) is (yes)
    ->no;
stop
@enduml
```

### 4.2 while-endwhile

```plantuml
@startuml
    while (check filesize ?) is (not empty)
        :read file;
    endwhile (empty)
    :close file;
@enduml
```

## 5 并行结构fork

```plantuml
fork
    :语句1;
fork again
    :语句2;
fork again
    :语句3;
end fork
```

## 6 注释

```plantuml
#语法一
floating note left:注释语句

#语法二
note right
    注释语句
end note
```

## 7 设置颜色

### 7.1 设置方法

- 使用标准名字，如：red，blue，white等。
- 使用十六进制写法，如：#AABBCC，#FF00FF等。

### 7.2 全局设置

#### 7.2.1 skinparam 命令

相关的参数如下：

```text
ActivityBackgroundColor
ActivityBarColor
ActivityBorderColor
ActivityBorderThickness
ActivityDiamondBackgroundColor
ActivityDiamondBorderColor
ActivityDiamondFontColor
ActivityDiamondFontName
ActivityDiamondFontSize
ActivityDiamondFontStyle
ActivityEndColor
ActivityFontColor
ActivityFontName
ActivityFontSize
ActivityFontStyle
ActivityStartColor
```

用法：

```plantuml
@startuml
	skinparam conditionStyle InsideDiamond
	skinparam ConditionEndStyle hline
	skinparam ArrowColor orchid
	skinparam ArrowThickness 2
	skinparam ActivityBackgroundColor lemonchiffon 
	skinparam ActivityBorderColor orchid
	skinparam ActivityBorderThickness 2
@enduml
```

### 7.3 单独设置

#### 7.3.1 设置背景颜色

在节点前面加上 #颜色名或十六进制代码: 。

```plantuml
@startuml
    #blue:start;
    #green:if (condition) is (yes) then
    else(no)
    endif
    #black:exit;
@enduml
```

#### 7.3.2 设置字体颜色

用<color:颜色名或十六进制代码>  </color>标签对 包裹文字。

```plantuml
@startuml
    :<color:blue>start</color>;
    if (<color:yellow>condition</color>) is (yes) then
    else(<color:red>no</color>)
    endif
    :<color:grey>exit</color>;
@enduml
```

## 8 分割与合并

### 8.1 分割

关键词：split, split again and end split

```plantuml
@startuml  
start
split
    :A;
split again
    :B;
    :C;
end split
end
@enduml
```

### 8.2 多端输入

使用 -[hidden]- > 隐藏箭头。去掉 start 和 end 关键词。

```plantuml
@startuml  
split
    -[hidden]->
    :A;
split again
    -[hidden]->
    :B;
split again
    -[hidden]->
    :C;    
end split
:D;
@enduml
```

### 8.3 多端输出

关键词：kill or detach

```plantuml
@startuml  
:D;
split    
    :A;
    kill
split again    
    :B;
    detach
split again    
    :C; 
    kill   
end split
@enduml
```

## 9 标题

关键词：title

```plantuml
@startuml
title 这是一个活动图
@enduml
```

## 10 条件样式

```plantuml
skinparam conditionStyle InsideDiamond
skinparam ConditionEndStyle hline
```