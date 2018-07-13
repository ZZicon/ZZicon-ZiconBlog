---
layout:     post
title:      "我的Python物语——第一章第四节"
subtitle:   "使用工具爬取JS加载的元素"
date:       2017-04-25 18:25:00
author:     "Zicon"
header-img: "img/post-bg.jpg"
catalog: true
tags:
    - Python
---

> “就算不是单纯的html元素，我也能爬取到哦”

# 前言

继上一节的旅程，这一节将给大家介绍的是：使用selenium＋PhantomJS实现动态数据的处理。

---

# 正文

**本文需要的组件有：selenium & PhantomJS。请自行下载[selenium](https://pypi.python.org/pypi/selenium) [selenium](http://phantomjs.org/download.html)**

简单的介绍一下selenium与PhantomJS：
 
 - PhantomJS，这是一个基于webkit的没有界面的浏览器，也就是它可以像浏览器解析网页，功能非常强大。
 - selenium，这是一个web的自动测试工具，可以模拟人的操作。支持市面上几乎所有的主流浏览器，同时也支持PhantomJS这种无界面浏览器。
 
核心代码是webdriver的初始化以及get方法获取浏览器加载完成后的源码。

```
dirver = webdriver.PhantomJS()
dirver.get(url)
```
 
代码如下：

```
# encoding=utf-8

import xlsxwriter
import time
from selenium import webdriver
from bs4 import BeautifulSoup

class selenium_PhantomJS:
    
    # 初始化方法
    def __init__(self):
        self.pagenum = 3
        self.num = 0
        self.user_agent = "Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/50.0.2661.102 Safari/537.36"
        # 引用PhantomJS
        self.path = 'C:\Program Files (x86)\Python36-32\phantomjs-2.1.1-windows\bin\phantomjs.exe'
        self.driver = webdriver.PhantomJS()
        # 获取时间
        data = time.strftime("%Y-%m-%d", time.localtime())
        # 建立Excel
        self.workbook = xlsxwriter.Workbook('test/bilibili/2017/bilibili-' + data + '.xlsx')
        self.sheet = self.workbook.add_worksheet('Bilibili-2017-4')
        self.sheet.set_column('A:A', 30)
        self.sheet.set_column('B:B', 20)
        self.sheet.set_column('C:C', 20)
        # 标题数据
        self.item = ['番剧名', '追番人数', '更新状态']
        for i in range(3):
            self.sheet.write(0, i, self.item[i])
        
    # 点击下一页
    def clickNext(self):
        # 获取按钮
        element = self.driver.find_element_by_class_name('nextPage')
        element.click()
        
    #判断元素的存在
    def isElementExist(self):
        driver = self.driver
        try:
            self.driver.find_element_by_class_name('nextPage')
            return True
        except:
            return False
        
    # 获取数据    
    def getItem(self):
        url = "http://bangumi.bilibili.com/anime/index#p=1&v=0&area=&stat=1&y=2017&q=0&tag=&t=1&sort=0"
        driver = self.driver
        driver.get(url)
        for x in range(self.pagenum):
            flag = self.isElementExist()
            if flag:
                self.clickNext()  
            soup = BeautifulSoup(driver.page_source, 'html.parser')
            items = soup.select('.preview')
            for item in items:
                title = item.select('.t')[0].text
                favorites = item.select('.sort-info')[0].text
                status = item.select('.num')[0].text
                self.num = self.num + 1 
                # 存入excel
                self.sheet.write(self.num, 0, title)
                self.sheet.write(self.num, 1, favorites)
                self.sheet.write(self.num, 2, status)
#                 print(item)
                print(self.num, "---", title, "---", favorites, "---", status)
        self.workbook.close()
        
spider = selenium_PhantomJS()
spider.getItem()        
```

由于这样获取到的数据是经过网页加工的字符串类型的数据，存放在数据库中的意义不大，所以在这里我是将其存放在Excel中。


# 后记

在这一次的旅程中，我学会了如何爬取动态加载的数据，以及实现了简单的web交互。装备“爬虫的培育手册”增加了新的功能“Excel存储相关”、“MySQL存储相关”以及“爬取动态加载的JS数据”。

参考自：

[python爬虫的最佳实践(五)--selenium+PhantomJS的简单使用](http://www.jianshu.com/p/520749be7377)

[如果网页内容是由javascript生成的，应该怎么实现爬虫呢？](https://www.zhihu.com/question/27734572)





