---
categories: Matplotlib
title: Matplotlib 词云图
tags: [ 教程, python]
date: 2023-03-13
lastmod: 2023-03-16 
thumbnail: https://s1.vika.cn/space/2023/03/25/c9c4077d7c524271b950dc30f155dd0a?attname=u%3D2705972022%2C1718814674%26fm%3D253%26fmt%3Dauto%26app%3D138%26f%3DPNG.webp
published: "true"
slug: 20230313170633
---
 
## 1 纯英文的词云图  

```python
from wordcloud import WordCloud
import matplotlib.pyplot as plt
with open('LICENSE.txt','r') as fp:
    text = fp.read()
wc = WordCloud()#创建WordCloud实例
wc.generate(text)#创建词云图
fig,ax = plt.subplots(figsize=(10,6))
ax.axis('off')
ax.imshow(wc)#显示词云图
plt.savefig('123.jpg')#保存
```

## 2 包含中文的词云图  

### 2.1  jieba分词+WordCloud.generate  

```python
from wordcloud import WordCloud
import matplotlib.pyplot as plt
import jieba  

text = "7月31日，中国空军新闻发言人申进科在空军航空开放活动新闻发布会上表示，捍卫祖国的大好河山，是人民空军的神圣使命。空军多型战机绕飞祖国宝岛，锤炼提升维护国家主权和领土完整的能力。空军有坚定的意志、充分的信心、足够的能力捍卫国家主权和领土完整。"
text_1 = jieba.cut(text)#分词
text_2 = ' '.join(text_1)#连成句子
wc = WordCloud(font_path='c:/windows/fonts/simhei.ttf',min_word_length=2)#中文需要设置字体，min_word_length 参数去除单字，如‘的、个、是’等
wc.generate(text2)
fig,ax = plt.subplots(figsize=(10,6))
ax.axis('off')
ax.imshow(wc)#显示词云图
plt.savefig('123.jpg')#保存
```

### 2.2  jieba.analyse 提取关键词及其权重+WordCloud.generate_from_frequencies  

```python
from wordcloud import WordCloud
import matplotlib.pyplot as plt
import jieba.analyse 

text = "7月31日，中国空军新闻发言人申进科在空军航空开放活动新闻发布会上表示，捍卫祖国的大好河山，是人民空军的神圣使命。空军多型战机绕飞祖国宝岛，锤炼提升维护国家主权和领土完整的能力。空军有坚定的意志、充分的信心、足够的能力捍卫国家主权和领土完整。"
keywords = jieba.analyse.extract_tags(text,topK=50,withWeight=True)#提取权重前50的关键词
keywords = {[i[0]:i[1] for i in keywords if not i[0].isnumeric()}#整合成字典形式，并去除数字关键词
wc = WordCloud(font_path='c:/windows/fonts/simhei.ttf')#中文需要设置字体

wc.generate_from_frequencies(keywords)
fig,ax = plt.subplots(figsize=(10,6))
ax.axis('off')
ax.imshow(wc)#显示词云图
plt.savefig('123.jpg')#保存
```
