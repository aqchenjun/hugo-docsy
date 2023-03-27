---
source: 
title: Obsidian 使用技巧
tags: [ 综合]
date: 2023-03-12
lastmod: 2023-03-27 
thumbnail: https://s1.vika.cn/space/2023/03/12/6a734b8314f54be7a542238033c99e92
published: "true"
---

>注意事项
>> 1. 文章内部链接时，括号内的地址以#开头，如果#前有文件名，应删除。
>> 2. dataview 只能识别双引号
>> 3. 文章标题自动编号，无需手动添加。每篇文章只设一个一级标题
## 1 Execute Code 插件
### 1.1 功能
在Obsidian 中直接运行代码
### 1.2 安装
通过第三方插件，[github 仓库地址](https://github.com/twibiral/obsidian-execute-code)
### 1.3 配置
1. Language-Specific Settings  设置为 Python。
2. Python path 设置python.exe的路径。
3. 其余保持默认
### 1.4 使用
```python
print('hello world')
```
编辑完成后，切换至预览模式，点击 Run 按钮。
### 1.5 反馈
能够运行简单的Python 语句，当需要导入模块时，还存在问题。

## 2 分栏
### 2.1 安装
[Github 仓库](https://github.com/efemkay/obsidian-modular-css-layout)
选择 [MCL Multi Column.css](https://github.com/efemkay/obsidian-modular-css-layout/blob/main/MCL%20Multi%20Column.css)
点击 “Copy raw contents”，复制后新建文件，保存在  .obsidian\snippets 目录中。
然后，设置，外观，CSS 代码片段，启用 MCL-Multi-Column。

### 2.2 使用
#### 2.2.1 有标题
>[!multi-column]
>
>{{% alert title=注意 color='success' %}}这是第一列

[!warning]+ 警告
这是第二列

[!info]+ 通知
这是第三列

 {{% /alert %}}#### 2.2.2 隐藏标题
>[!multi-column]
>
>>[!blank-container]
>>这是第一列
>
>>[!warning]+ 警告
>>这是第二列
>
>>[!blank-container]
>>这是第三列

## 3 运行 python 脚本
运行python 脚本的方法有多种，包括利用 jupyter 插件和 [Execute Code 插件](#1-Execute-Code-插件)，但都存在各种各样的问题。这里利用 Templater 插件。
### 3.1 安装 Template 插件
### 3.2 配置 Template插件
#### 3.2.1 设置模板路径
模板文件的存放位置。Template folder location 选项设置模板路径，一般设为 Template，可以自定义。
![|700](https://thumbsnap.com/i/P13fhEcb.png)
#### 3.2.2 设置脚本路径
脚本文件的存放位置。Script files folder location 选项设置脚本路径，一般设为 Scripts，可以自定义。
![|700](https://thumbsnap.com/i/9MpPN75B.png)
#### 3.2.3 设置cmd文件的路径。
Shell binary location 选项设置cmd文件的路径，一般是C:\Windows\System32\cmd.exe。

#### 3.2.4 用户的自定义函数
这一步可以在建立python 脚本文件后，配置
![](https://thumbsnap.com/i/7yKkMsfF.png)

图中，左边框是自定义函数名，右边是该函数内容。内容分三部分：
1. python 命令。
2. python 脚本文件的路径，是相对于库的路径。
3. 传给脚本文件的参数，可以省略。如果有，将以字符串的形式传递，以空格为分隔符，脚本文件以 sys.argv 列表的形式获得，其中sys.argv[0]为命令行，sys.argv[1]为第二个字符串，依次类推。上图实例中参数依次为 <% tp.file.path() %> <% tp.file.path(true) %>，表示Obsidian 当前文件的绝对路径和相对路径。
>注意
>>由于本文只需要得到当前文件的绝对路径，因此第二个参数省略。

### 3.3 用法
>建立python 脚本文件，文件名暂按上图实例中的名，内容包括对需要的Obsidian 当前文件的参数进行处理。将该文件保存到[脚本路径](#3.2.2-设置脚本路径)。
代码如下：
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
    #寻找 published，将其属性修改为 true
    content = re.sub(r"published\: false|published\: |published\:",r'published: true',content)
    #取消文章的内部链接
    content = re.sub(r'\[(.*)\]\([^http:|https:].*\)',r"\1",content)
posts_path = "F:\Blog\source\_posts"
with open(os.path.join(posts_path,filename[0]),'w',encoding='utf-8') as f:
    # 将修改后的内容写入文件
    f.write(content)
```
>按照[3.2.4 用户的自定义函数](#3.2.4-用户的自定义函数)的介绍，新增自定义函数。

^2d2d1d


>在[3.2.1 设置模板路径](#3.2.1-设置模板路径)中设置的目录中，新建模板文件，文件名可以自定义 。文件内容如下：
>><% tp.user.copy_article_to_des() %>

^9f4b35

>`copy_article_to_des` 为[新增自定义函数](#^2d2d1d)中的函数名。

>需要运行时，点击左侧边栏的 "Templater" 图标，选择[模板文件夹](#^9f4b35)中建立的模板文件，即可。

### 3.4 注意字符乱码问题
解决方案：

1. 按键盘 `Win+R`

2. 输入 `intl.cpl`

3. 选择“管理”
4. 点击 “更改系统区域设置”
5. 选中 “Beta版：使用 Unicode UTF-8 提供全球语言支持(U)”
6. 点击确定之后，需要重启系统。重启完后，就解决问题了。

## 4 备份与同步
### 4.1 实现目标
1. 网络备份
2. 手机端随时查看笔记内容
3. 电脑端和手机端的剪藏
### 4.2 准备工作
#### 4.2.1 [坚果云](https://www.jianguoyun.com/)注册
- 注册后，创建同步文件夹，名称与Obsidian 仓库名一致。

![创建同步文件夹](https://thumbsnap.com/i/ambDKSy8.jpg)

- 点击右上角的用户名，下拉菜单中选择“账户信息”。
- 选择 “安全选项” 框

![安全选项](https://thumbsnap.com/i/XpQGJjSd.jpg)

- 拉倒页面底端，点击“添加应用”。输入名称，然后点击“生成密码”。复制密码。

![生成密码](https://thumbsnap.com/i/7wtAwCc1.jpg)

#### 4.2.2 Instapaper 注册
[Instapaper](https://www.instapaper.com)注册

### 4.3 需要的工具
#### 4.3.1 电脑端
1. Obsidian remotely save 插件
2. Obsidian Read Later 插件
3. Chrome markdown-clipper 插件
#### 4.3.2 手机端
1. Obsidian remotely save 插件
2. Instapaper app
### 4.4 配置
主要是电脑端的配置。
#### 4.4.1 Obsidian remotely save 插件配置

![](https://thumbsnap.com/i/rUpWyjJi.png)

图中的服务器地址和用户名来自“安全选项” 框中的服务器地址和用户名，密码是上一步“添加应用”中生成的密码。
“自动运行”选项和“启动后自动运行一次”选项，按需设置，其余保持默认即可。

#### 4.4.2 Obsidian Read Later 插件配置
1. "Read Later Folder" 选项设置为笔记存放的目录。
2. "InstaPaper Integration" 设置用户名和同步频率，用户名来自[Instapaper 注册](#4.2.2-Instapaper-注册)
### 4.5 使用
Obsidian remotely save 插件配置好后，自动运行。
手机端首次运行，可以将电脑端的仓库复制到手机，方法见[华为手机与电脑的连接]({{< ref "/blog/华为手机与电脑的连接.md" >}})。
手机剪藏操作：分享至 InstaPaper。
Obsidian Read Later 插件配置好后，自动运行。

## 5 Obsidian 的图床
### 5.1 准备工作
1. [下载](https://picgo.github.io/PicGo-Doc/zh/guide/#%E4%B8%8B%E8%BD%BD%E5%AE%89%E8%A3%85)并安装 PicGo
2. 安装 node.js
3. 为 PicGo 安装插件 vikadata
4. [vika维格表](https://vika.cn/)注册用户
5. 安装 Obsidian image-auto-upload 插件
### 5.2 操作步骤
1. 登录 vika，+新建空白维格表 
2. 点击右边的API，进入API示例面板 ，点击 “Add新增” 标签
3. 找到API Token。选中 API Token 选项框，如果已经绑定邮箱，会出现
`-H "Authorization: Bearer 字符串" \`的字样，字符串即为API Token。如果未绑定邮箱，会跳入一个新的页面，点击+创建，不过需要绑定你的邮箱，绑定后在打开API Token，就会显示了，点击复制按钮直接复制就行。
4. 找到维格表ID。就是 datasheets/需要复制的内容/records 中的以dst开头的那部分，复制那部分内  
5. 打开PicGo 插件 vikadata 设置，填入 API Token 和 维格表 ID ，然后点击“确定”，并设为默认图床。
![image.png](https://s1.vika.cn/space/2023/03/11/025a055803114a9a98cb05145a8a634b)
6. Obsidian 中，执行复制图片的操作，即自动上传图片至 vika。

## 6 调整图片大小及对齐方式
### 6.1 图片大小的设置
在[]的最后加上“|数字”，如 |200，即将图片宽度调整为200px，图片高度自动调整。
### 6.2 图片的对齐方式
>#pic_right，右对齐
>#pic_left，左对齐
>#pic_center，居中
### 6.3 实例

![image.png#pic_right|200](https://s1.vika.cn/space/2023/03/14/5ee997c780f14014aa448dd7845e85df) 