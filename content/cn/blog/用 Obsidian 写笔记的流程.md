---
title: 用 Obsidian 写笔记的流程
tags:
  - 综合
date: '2023-03-12 17:37'
published: 'true'
abbrlink: b01ebd45
source:
cover:
thumbnail: https://s1.vika.cn/space/2023/03/12/f097b4e8910c4e0fa0767d4084298d15
---

# 1、需要的插件
1. Templater
2. QuickAdd
3. Button
4. Dataview
5. Commander
6. Yaml Manager
7. Number Headings
# 2、新建书籍
## 1 建立书籍模板
### 1.1 模板内容
1. Front-matter 部分
2. “新建笔记”按钮
3. 笔记列表
### 1.2 Front-matter
可以自动完成填充的参数是：
>书名: `<% tp.file.title %>`
>开始阅读时间: `<% tp.file.creation_date("YYYY-MM-DD HH:mm") %>`

### 1.3 “新建笔记”按钮
#### 1.3.1 设置 QuickAdd 

![image.png](https://s1.vika.cn/space/2023/03/11/a9e52308fc054140a6bf1a1cdbd4abb2)

>选择 Template，然后点击 Add Choice
>进行 Configure

![image.png](https://s1.vika.cn/space/2023/03/11/11e291a12bc94d058de320e472ad61a4)

>主要设置两项：
>>1. Template Path
>>2. Choose folder when creating a new note

>点亮闪电标志

![image.png](https://s1.vika.cn/space/2023/03/11/ca4eb9aecafc45ed9acf0892c1c86f43)

#### 1.3.2 设置 Button
快捷键 Ctrl+P，选择 Buttons:Button Maker

![image.png](https://s1.vika.cn/space/2023/03/11/98ab3c0036bc4999a26217ddb4560fb8)

主要设置4项：
1. Button Name
2. Button Type。一般选择Command-run a command prompt command
3. Command。选择[2-1-3-1 设置 QuickAdd](#2-1-3-1-设置-QuickAdd)设置的命令。
4. Color。
5. 点击 “Insert Button”

![image.png](https://s1.vika.cn/space/2023/03/11/14944c73e53745efb8635a755df29400)

### 1.4 笔记列表
利用 Dataview。
```text
list without id	
from #笔记 
where source = "<% tp.file.title %>"
sort date
```
需要注意的是，`<% tp.file.title %>`表示书籍的文件名，而`where 出处 = "<% tp.file.title %>"`表示具有 #笔记 标签的 markdown 文件的“出处”参数的值等于书籍的文件名。另外，Dataview 只识别双引号。 

## 2 利用 "书籍模板" 新建书籍
### 2.1 参考[2-1-3-1 设置 QuickAdd](#2-1-3-1-设置-QuickAdd)
新增一个命名为“新增书籍”的QuickAdd命令

### 2.2 为该命令设置 Commander
#### 2.2.1 在左侧边栏添加命令

![image.png](https://s1.vika.cn/space/2023/03/11/53063d5ad8ac4f37b75fc2645b6598b0)

#### 2.2.2 选择 QuickAdd 命令
选择[2-2-1 参考](#2-1-3-1-设置-QuickAdd)设置的 QuickAdd 命令

![image.png](https://s1.vika.cn/space/2023/03/11/810ab579ada74862bcf8858ad147346f)

#### 2.2.3 选择图标和图标文字

# 3、新建笔记
## 1 建立"笔记模板"
### 1.1 模板内容
1. Front-matter
2. 笔记出处的说明
3. 笔记内容
### 1.2 Front-matter
可以自动完成填充的参数是：
出处: `<% tp.config.active_file.basename %>`
title: `<% tp.file.title %>`
date: `<% tp.date.now("YYYY-MM-DD HH:mm") %>`
说明：
>`<% tp.config.active_file.basename %>`，表示当前文件的文件名，没有.md后缀。因此需要在书籍文件激活时，新建该书籍的笔记。

### 1.3 笔记出处的说明
`《<% tp.config.active_file.basename %>》读书笔记之`
只有在利用模板创建笔记的时候，才能自动填充。

### 1.4 笔记内容
文章标题自动编号，无需手动添加。每篇文章只设一个一级标题

# 4、发博客
笔记用 Obsidian 编写，博客采用 Hexo 框架，二者的衔接存在如下问题：
1. Obsidian 的格式与 Hexo 的有不一致，尤其是链接。
2. Obsidian 的仓库和 Hexo 的博客目录不是一个，写好的笔记需要复制到博客目录。
3. Obsidian 的笔记的有些内容在发博客时，需要调整。
>解决办法，参考<a href="{% post_path 'obsidian 使用技巧' %}#3、运行-python-脚本) 和 [Hexo 的链接](hexo-的使用技巧-md#2、链接">3、运行 python 脚本) 和 [Hexo 的链接](hexo 的使用技巧.md#2、链接</a>

## 1 编写Python 脚本
```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
import sys
import os
import re  

# 获取当前文件名
filename = ' '.join(sys.argv[1:])
with open(filename,"r",encoding='utf-8') as f:
    content = f.read()  

# 去掉 Front Matter 中tags 的“笔记”值，
# 正则匹配时，因为有换行，而 . 不能匹配换行符，因此
# 采用 [\s\S]匹配所有字符
content = re.sub(r'(\-\-\-[\s\S]*tags?\:\s+.*)(笔记\s*\,?)(.*)(\n)', r'\1\3\4',content)  

# 取消文章内部的书籍链接，亦即形如《》或《》的链接格式
content = re.sub(r'\《\[(.*)\]\((?!http:)(?!https:).*?\)\》',r"《\1》",content)
content = re.sub(r'\《\[\[(.*?)\]\]\》',r'《\1》',content)

# 调整站内文章跳转链接的格式，亦即形如[](非#开头，非http:开头，非https:开头，以md结尾)的链接格式，从Obsidian 格式调整为 hexo 格式
titles = re.findall(r'\[.*?\](?!#)(?!http:)(?!https:).*?\.md\)', content)
for i in range(len(titles)):
    s = titles[i]
    # 将空格替换为 ' '
    s = re.sub(r'%20',r' ',s)
    # 转换链接格式
    s = re.sub(r'\[(.*?)\]\((.*?)\.md\)',r'{% post_link "\2"  "\1" %}',s)
    content = content.replace(titles[i],s)  

#调整当前文章内部锚点跳转链接的格式 ，形如[](#开头) ，调整为 hexo 格式
titles = re.findall(r'\[.*?\]\(#.*?\)',content)
results = []
for i in range(len(titles)):
    s = titles[i]
    # 将空格符号 %20 替换为 '-'
    s = re.sub(r'%20', r'-', s)
    # 将半角符号替换为 '-'，连续的半角符号只替换为一个 '-'
    s = re.sub(r'[\.\`\__\-\']+',r'-', s)
    content = content.replace(titles[i],s)

# 调整站内其他文章锚点链接的格式，形如<a href="{% post_path '非#开头，中间有 ' %}#开头，中间有 -md# 字符串">开头，中间有 .md# 字符串</a> ，调整为 hexo 格式
titles = re.findall(r'\[.*?\]\(.*\.md#.*?\)',content)
for i in range(len(titles)):
    s = titles[i]
    # 提取文章名
    name = re.findall(r'\((.*?).md',s) 
    # 将空格符号 %20 替换为 ' '
    titlename = re.sub(r'%20', r' ', name[0])
    # 提取章节名称
    chapter = re.findall(r'#(.*)\)',s)
    # 将空格符号 %20 替换为 '-'
    chaptername = re.sub(r'%20', r'-', chapter[0])
    # 将半角符号替换为 '-'，连续的半角符号只替换为一个 '-'
    chaptername = re.sub(r'[\.\`\__\-\']+',r'-', chaptername)
    
    # 建立 a 标签
    a_tag = '<a href="{% post_path ' + "'" + titlename + "'" + ' %}#' + chaptername + '">'+ re.sub(r'%20',r' ',chapter[0]) +'</a>'
    content = content.replace(titles[i],a_tag)  

posts_path = "F:\Blog\source\_posts"
with open(os.path.join(posts_path,filename[0]),'w',encoding='utf-8') as f: 
    # 将修改后的内容写入文件
    f.write(content)
```
## 2 在 Templater 插件里设置自定义函数
参考<a href="{% post_path 'obsidian 使用技巧' %}#3-2-4-用户的自定义函数">3.2.4 用户的自定义函数</a>
## 3 建立模板文件，调用上一步中设置的自定义函数
参考<a href="{% post_path 'obsidian 使用技巧' %}#3-3-用法">3.3 用法</a>
## 4 在 Templater 插件里，设置快捷键

![image.png](https://s1.vika.cn/space/2023/03/11/50efb0f5317f4dbf8180abb25729a3f5)

## 5 在 Commander 插件里，设置命令图标
参考[2-2-2 为该命令设置 Commander](#2-2-2-为该命令设置-Commander)

# 5、总结
```plantuml
@startuml
start
	skinparam conditionStyle InsideDiamond
	skinparam ConditionEndStyle hline
	skinparam ArrowColor orchid
	skinparam ArrowThickness 2
	skinparam ActivityBackgroundColor lemonchiffon  
	skinparam ActivityBorderColor orchid
	skinparam ActivityBorderThickness 2
	title Obsidian 写笔记的流程
	:点击左侧边栏的“新建书籍”图标，新建书籍;	
	:输入书籍名;
	:系统在 /我的书架/书籍 目录中新建 Markdown 文件，文件名就是输入的书籍名;
	:自动填写 Front-matter 中的 书名: <% tp.file.title %>;
	:自动填写 Front-matter 中的 开始阅读时间: <% tp.file.creation_date("YYYY-MM-DD HH:mm") %>;
	:插入“新建笔记”按钮;
	:插入 Dataview 代码，自动填写 where 出处 = "<% tp.file.title %>";
	:在该文件中，点击“新建笔记”按钮;
	:输入笔记名，系统在 /我的书架/读书笔记 目录中新建 Markdown 文件，文件名就是输入的笔记名;
	:自动填写 Front-matter 中的 出处: <% tp.config.active_file.basename %>;
	:自动填写 Front-matter 中的 title: <% tp.file.title %>;
	:自动填写 Front-matter 中的 date: <% tp.date.now("YYYY-MM-DD HH:mm") %>;
	:自动填写笔记的出处说明
《<% tp.config.active_file.basename %>》读书笔记之;
	:笔记编辑过程中，自动对标题编号;
	:笔记编辑过程中，点击左侧边栏的“更新Front-matter”图标，对Front-matter 参数进行修改;
	if (需要发博客) then (yes)
		:修改 Front-matter 的参数 publised 为true;
		:点击左侧边栏的“发布”图标;
		:自动去掉 笔记 标签;
		:自动转换链接格式;
		:自动另外保存到博客目录;
	else(no)
	endif
end
@enduml
```