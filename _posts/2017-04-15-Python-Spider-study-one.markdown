---
layout:     post
title:      "我的Python物语——第一章第一节"
subtitle:   "分析网页结构爬取文本数据"
date:       2017-04-15 18:26:00
author:     "Zicon"
header-img: "img/post-bg.jpg"
catalog: true
tags:
    - Python
---

> “看啊，这是我培养的第一条爬虫呢”

# 前言

Zicon完成村长的新手任务后。村长主动向Zicon搭话了，“新人啊，没想到你这么快就把任务完成了，我这里有一个最后的考验，只要你能完成就可以提前出村哦。” 

“天啊，这么快我就可以出村了吗，我觉得我还没有足够的实力啊” “不用担心，Python的世界没有你想象的那么危险哟，而且在你前面已经有很多的冒险者出发了，你可以向他们请教呀”

“好吧村长，您说，是什么考验？”Zicon终于下定决心向村长问道。“不难，不过你要先告诉我你想在Python的世界获得什么？” “我吗，我想要学会爬虫！” “恩，好，那么你就去培养你的第一只爬虫给我看看吧，如果我满意的话，你的任务就算完成了。” 

---

# 正文

# Python爬虫

**什么是爬虫**

网络爬虫（又被称为网页蜘蛛，网络机器人，在FOAF社区中间，更经常的称为网页追逐者），是一种按照一定的规则，自动地抓取万维网信息的程序或者脚本。

**爬虫原理**

网络爬虫框架主要由控制器、解析器和索引库三大部分组成，而爬虫工作原理主要是解析器这个环节，解析器的主要工作是下载网页，进行页面的处理，主要是将一些JS脚本标签、CSS代码内容、空格字符、HTML标签等内容处理掉，爬虫的基本工作是由解析器完成。所以解析器的具体流程是：

`入口访问->下载内容->分析结构->提取内容`

**正式开工**

第一步，要选取一个网页，第一个爬虫，我们尽量选择简单的实现。经过多方面的考虑，Zicon最后选择了音乐期刊-落网作为来源。其网页地址为http://www.luoo.net/tag/?p=1，其网页数暂时从1到91。

第二步，分析网页结构，主要在于要获取的信息的网页布局。

![](https://ZZicon.github.io/ZiconBlog/img/int_post/Python/python_simple1.png)

在图片上左红框内是要获取的信息，通过通过Chrome的开发者模式可以查看到对应的布局结构，即右侧红框。

可以看到要获取的信息位于div class="vol-list"下每一个div class="item"里，在class="meta rounded clearfix"中存放着title、comments以及favs这些相关信息。

网页结构的分析有很多种方法方式，在这里Zicon使用的是BeautifulSoup(bs4)第三方库实现解析HTML结构并提取内容。

根据上述分析，我们可以得到所有信息的位置路径，接下来我们以Python实现数据爬取。

用了一天时间经历重重困难之后，Zicon成功培养出第一只爬虫，爬取落网中91页所有期刊中每个期刊的标题、喜爱数以及评论数。

代码如下：

```
import urllib
from urllib import request
from bs4 import BeautifulSoup
num = 1
for x in range(91):
    page = x + 1
    url = "http://www.luoo.net/tag/?p=" + str(page)
    '''模拟真实浏览器的用户头传值——User-Agent'''
    head = "Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/50.0.2661.102 Safari/537.36"
    req = request.Request(url)
    req.add_header("User-Agent",head)
    try:
        resp = request.urlopen(req).read().decode("utf-8")
        soup = BeautifulSoup(resp, 'html.parser')
        for list in soup.select('.vol-list'):
            for item in list.select('.item'):
                favs = item.select('.favs')[0].text.strip()
                comments = item.select('.comments')[0].text.strip()
                title = item.select('.name')[0].text.strip()
                if int(favs) > 15000:
                    print(title, favs, comments, num)
                    num = num + 1
                    
    except urllib.error.URLError as e:
        print('Failed to reach s server.')
        print('Reason: ', e.reason)
```

而在Python编程中，面向对象的编程思想运用广泛，因此借鉴网上的相关代码，Zicon做出一些修改，让我培养的爬虫有了些许进步。

代码如下：

```
import urllib
from urllib import request
from bs4 import BeautifulSoup

class luoMusic: 

    # 初始化方法
    def __init__(self):
        self.num = 1
        self.pageNum = 91
        self.user_agent = "Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/50.0.2661.102 Safari/537.36"
        self.role = 15000
    
    # 获取数据    
    def getItem(self):
        for x in range(self.pageNum):
            page = x + 1
            url = "http://www.luoo.net/tag/?p=" + str(page)
            req = request.Request(url)
            req.add_header("User-Agent", self.user_agent)
            try:
                resp = request.urlopen(req).read().decode("utf-8")
                soup = BeautifulSoup(resp, 'html.parser')
                items = soup.select('.vol-list > .item')
                for item in items:     
                    title = item.select('.name')[0].string
                    favs = item.select('.favs')[0].text.strip()
                    comments = item.select('.comments')[0].text.strip()                   
                    if int(favs) > self.role:
                        print(title, ' Favs:', favs, 'Comments:', comments)
                        self.num = self.num + 1 
            except urllib.error.URLError as e:
                print('Failed to connection url.')
                print('Reason: ', e.reason)

spider = luoMusic()
spider.getItem()
```

以上程序获得数据之后只是在控制台输出，而一般情况下我们抓取数据都需要保存下来，留作记录以便之后查看。

Zicon打算是利用Excel保存数据，Python读写Excel的工具有xlwt、openpyxl以及xlsxwriter三种，由于Zicon在这里仅仅是想把数据保存起来而不需要读取已有文件，所以最后决定采用XlsxWriter工具。

所以，最终代码如下：

```
import urllib
import xlsxwriter
import time
from urllib import request
from bs4 import BeautifulSoup

class luoMusic: 

    # 初始化方法
    def __init__(self):
        self.num = 0
        self.pageNum = 91
        self.user_agent = "Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/50.0.2661.102 Safari/537.36"
        self.role = 15000
        #获取时间
        data = time.strftime("%Y-%m-%d", time.localtime())
        #建立Excel
        self.workbook = xlsxwriter.Workbook('test/LuoMusic-' + data + '.xlsx')
        self.sheet = self.workbook.add_worksheet('luoMusic')
        self.sheet.set_column('A:A', 40)
        #标题数据
        self.item = ['期刊名', '喜欢人数', '评论数']
        for i in range(3):
            self.sheet.write(0, i, self.item[i])
            
    # 获取数据    
    def getItem(self):
        for x in range(self.pageNum):
            page = x + 1
            url = "http://www.luoo.net/tag/?p=" + str(page)
            req = request.Request(url)
            req.add_header("User-Agent", self.user_agent)
            try:
                resp = request.urlopen(req).read().decode("utf-8")
                soup = BeautifulSoup(resp, 'html.parser')
                items = soup.select('.vol-list > .item')
                for item in items:     
                    title = item.select('.name')[0].string
                    favs = item.select('.favs')[0].text.strip()
                    comments = item.select('.comments')[0].text.strip()                   
                    if int(favs) > self.role:
                        print(self.num, "----", title, ' Favs:', favs, 'Comments:', comments)
                        self.num = self.num + 1 
                        self.sheet.write(self.num, 0, title)
                        self.sheet.write(self.num, 1, favs)
                        self.sheet.write(self.num, 2, comments)
            except urllib.error.URLError as e:
                print('Failed to connection url.')
                print('Reason: ', e.reason)
        self.workbook.close()
        
spider = luoMusic()
spider.getItem()
```

Python爬虫入门虽然有难度，但是其中的复杂度并没有我想象中难。通过实现爬虫，我收获到了不少乐趣与成就感。希望看到这里的你，也能够去编写属于你的爬虫程序。

# 后记
培养出了我的第一条爬虫了，不仅可以爬取网页数据，还可以做到保存数据。那么，这条爬虫能否让老村长满意呢？我又是否能够走出新手村呢？

请期待我接下来的冒险物语。





