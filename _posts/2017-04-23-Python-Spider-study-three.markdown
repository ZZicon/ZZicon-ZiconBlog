---
layout:     post
title:      "我的Python物语——第一章第三节"
subtitle:   "尝试爬取JS加载的元素"
date:       2017-04-23 19:25:00
author:     "Zicon"
header-img: "img/post-bg.jpg"
catalog: true
tags:
    - Python
---

> “如果不是单纯的html元素，你还能爬取到吗？”

# 前言

Zicon出村之后准备前往冒险者森林提升等级，由于路途遥远~ 	Zicon决定在赶路期间根据“爬虫的培育手册”继续好好培育一下爬虫。

---

# 正文

**本文需要的组件有：pymysql。请[自行下载](https://pypi.python.org/pypi/PyMySQL)**

作为一个动漫爱好者、B站常客。Zicon在尝试强化自己的爬虫的时候就自然而然的以B站做挖掘实验啦~

Zicon首先是以“番剧索引”查看“连载”新番的网址作为首次尝试，希望可以爬取新的资讯吧，hh。

然而，在首次爬取的时候，Zicon发现了一件很尴尬的事情。就是下图以开发者模式选取的区域，在网页上有显示，但是爬取当前页面数据后却没有该div元素。

![](https://ZZicon.github.io/ZiconBlog/img/int_post/Python/bilibili1.png)

经过一番度娘之后，Zicon知道这是B站将这些数据由ajax动态生成。那么，接下来就有两个方式来解决这个情况。

 - 第一：找到这个动态加载的数据块，用像以往读取网页数据一样读取该数据。
 - 第二：以selenium＋PhantomJS工具渲染并读取js。
 
那么，我说说我个人对这两种方式的看法。

 - 首先，方式一的读取数据方式与我以往的做法一致，无需其他组件，爬取数据的速度较快。而方式二不仅需要下载两个组件，而且因为要以PhantomJS对页面进行渲染，因此在爬取速度上会比较慢。
 - 然后，方式一的读取方式需要我们清楚的知道数据块的请求地址，直接发送情求，返回的是json数据包，需要转换成Python可处理的数据。而方式二返回的就是你在网页上看到的数据形式。当然，这两种方式各有优劣，一个可以让你获取最原始的数据，一个可以让你得到加工后的数据显示。
 - 最后，方式二最大的优点就是他可以实现页面的简单交互，例如点击按钮、输入搜索框进行搜索等等。
 
下面Zicon会给出两种方式的实现，各位读者可以根据自己的需要进行参考学习~ 

**本篇文章先介绍第一种方法！**

***首先获取数据包的请求路径***

json种类的数据一般存放在Network下XHR处，根据命名可以知道正确的数据包，点击Headers可以查看请求路径，点击Response可以查看返回的数据以及包装方式。

![](https://ZZicon.github.io/ZiconBlog/img/int_post/Python/bilibili2.png)

代码如下：

```
# encoding=utf-8

import requests
import urllib
import json
import os
from urllib import request

class bilibili: 

    # 初始化方法
    def __init__(self):
        self.pageNum = 3
        self.user_agent = "Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/50.0.2661.102 Safari/537.36"
        # 创建存储文件
#         os.mkdir("test/bilibili/2017/4")
            
    # 获取数据    
    def getItem(self):
        for x in range(self.pageNum):
            page = x + 1
            url = "http://bangumi.bilibili.com/web_api/season/index_global?page=" + str(page) + "&page_size=20&version=0&is_finish=1&start_year=2017&tag_id=&index_type=1&index_sort=0&quarter=0"
            req = request.Request(url)
            req.add_header("User-Agent", self.user_agent)
            try:
                resp = request.urlopen(req).read().decode("utf-8")
                datas = json.loads(resp)['result']['list']
                for data in datas:
                    favorites = int(data['favorites'])
                    id = int(data['season_id'])
                    status = int(data['season_status'])
                    title = data['title']
                    img = data['cover']
                    movie = data['url']
                    # 下载图片    
                    with open("test/bilibili/2017/4/%s.jpg" % title.replace(':', '_'), 'wb+') as f:
                        img_req = requests.get(img, self.user_agent) 
                        f.write(img_req.content)
                        f.close() 
            except urllib.error.URLError as e:
                print('Failed to connection url.')
                print('Reason: ', e.reason)
        
spider = bilibili()
spider.getItem()     
```

运行后会在Python项目当前文件夹下创建test/bilibili/2017/4文件夹，之后将JPG文件爬取到对应文件夹中。

做到这一步，Zicon又有了新的想法。既然现在爬取到的原始数据这么清晰，那么除了下载、保存到Excel之外还可以通过什么样的手段进行保存呢？比如MySQL数据库。于是，Zicon搜索资料之后发现，只要安装了pymysql组件，Python就可以与MySQL数据库进行交互了。

改进代码如下：

```
# encoding=utf-8

import pymysql
import requests
import urllib
import json
import os
from urllib import request

class bilibili: 

    # 初始化方法
    def __init__(self):
        self.pageNum = 3
        self.user_agent = "Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/50.0.2661.102 Safari/537.36"
        # 创建存储文件
#         os.mkdir("test/bilibili/2017/4")
        # 连接数据库
        self.db = pymysql.connect("localhost", "root", "root", "python", charset="utf8")
        self.cursor = self.db.cursor() 
            
    # 获取数据    
    def getItem(self):
        for x in range(self.pageNum):
            page = x + 1
            url = "http://bangumi.bilibili.com/web_api/season/index_global?page=" + str(page) + "&page_size=20&version=0&is_finish=1&start_year=2017&tag_id=&index_type=1&index_sort=0&quarter=0"
            req = request.Request(url)
            req.add_header("User-Agent", self.user_agent)
            try:
                resp = request.urlopen(req).read().decode("utf-8")
                datas = json.loads(resp)['result']['list']
                for data in datas:
                    favorites = int(data['favorites'])
                    id = int(data['season_id'])
                    status = int(data['season_status'])
                    title = data['title']
                    img = data['cover']
                    movie = data['url']
                    # 插入数据库
                    sql = 'INSERT INTO BilibiliDrama (season_id, season_name, favorites, season_status, photo) VALUES ("%d", "%s", "%d", "%d", "%s")' % (id, title, favorites, status, img)
                    sql = sql.encode(encoding='utf-8', errors='strict')  
                    try:
                        # 执行sql语句
                        self.cursor.execute(sql)
                        self.db.commit()
                    except pymysql.Error as w :
                        print('Reason: ', w.reason)
                        self.db.rollback()
                    # 下载图片    
                    with open("test/bilibili/2017/4/%s.jpg" % title.replace(':', '_'), 'wb+') as f:
                        img_req = requests.get(img, self.user_agent) 
                        f.write(img_req.content)
                        f.close() 
            except urllib.error.URLError as e:
                print('Failed to connection url.')
                print('Reason: ', e.reason)
        
spider = bilibili()
spider.getItem()
```

代码在原有基础上增加了将数据保存至数据库的功能。保存之后的结果如图：

![](https://ZZicon.github.io/ZiconBlog/img/int_post/Python/bilibili3.png)

这样，我们也可以更直观的查看原始的数据，这一点使用selenium＋PhantomJS不一定能做到，毕竟获取到的数据是经过网页一定程度的加工了。

至此，对爬虫的知识又更进一步，学会了爬虫如何直接爬取动态加载的数据。

# 后记
**特别声明一下：B站是本人非常喜欢的一个中国最大的同性交友网站（大雾）！本文只是拿该网站进行爬虫的技术学习交流之用。读者涉及到的所有侵权问题都与本人无关！**

继续学习“爬虫的培育手册”，以增强自己的爬虫基础~ 下一节将给大家介绍使用selenium＋PhantomJS实现动态数据的处理。