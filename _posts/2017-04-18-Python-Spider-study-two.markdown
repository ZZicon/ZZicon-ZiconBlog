---
layout:     post
title:      "我的Python物语——第一章第二节"
subtitle:   "尝试爬取音乐文件"
date:       2017-04-18 18:25:00
author:     "Zicon"
header-img: "img/post-bg.jpg"
catalog: true
tags:
    - Python
---

> “你以为它就只能爬取文本吗？”

# 前言

虽然Zicon做出了第一条爬虫，但是村长明显不很满意。他觉得这条爬虫的能力太弱了，仅靠它Zicon很难在村外生存下去。

村长交给了Zicon一件装备“爬虫的培育手册”，希望Zicon能够在翻阅手册后改良我的爬虫。

在仔细阅读“爬虫培育手册”后，Zicon认为自己的爬虫也许应该增强挖掘能力了。例如，爬取网页中的MP3文件。

---

# 正文

同样是选择了音乐期刊-落网作为来源。通过开发者模式查找到落网期刊中每一首歌曲的mp3文件存放地址。

![](https://ZZicon.github.io/ZiconBlog/img/int_post/Python/python_example1.png)

之后，仍然使用BeautifulSoup(bs4)第三方库实现解析HTML结构并提取内容。

代码如下：

```
import os
import requests
from urllib import request
from bs4 import BeautifulSoup

class DownloadMusic:
    
    # 初始化方法
    def __init__(self):
        self.user_agent = "Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/50.0.2661.102 Safari/537.36"
        self.target = [680, 721, 725, 720]
        #创建总文件夹
        os.mkdir("test")
       
    # mp3下载    
    def download(self): 
        for x in range(len(self.target)):
            #创建文件夹一一对应期刊号
            os.mkdir("test/%d" % self.target[x])
            
            #期刊主页
            url = "http://www.luoo.net/vol/index/" + str(self.target[x])
            req = request.Request(url)
            req.add_header("User-Agent", self.user_agent)
            
            #获取音乐标题
            resp = request.urlopen(req).read().decode("utf-8")
            soup = BeautifulSoup(resp, 'html.parser')
            tracks = soup.select('.track-item > .track-wrapper > .trackname')
            for track in tracks: 
                _id = track.text[:2]
                _name = track.text
                #读取音乐地址
                music_url = "http://mp3-cdn.luoo.net/low/luoo/radio" + str(self.target[x]) +"/%s.mp3" % _id
                music_req = requests.get(music_url, self.user_agent)  
                with open("test/%d/%s.mp3" %(self.target[x], track.text), 'wb+') as f:
                    f.write(music_req.content)
                    f.close() 
                print("%s.mp3 done!" % track.text)
       
spider = DownloadMusic()
spider.download()       
```

运行后会在Python项目当前文件夹下创建test文件夹，然后在该文件夹中创建对应期刊号的文件夹，之后将MP3文件爬取到对应文件夹中，结果如图：

![](https://ZZicon.github.io/ZiconBlog/img/int_post/Python/python_musicDownload.png)

至此，对爬虫的知识又更进一步，用爬虫不仅可以获得网页布局上的一些文本数据。还可以通过网页上给出的文件地址，对文件进行爬取，例如图片文件与音乐文件都可以。

# 后记
完善了我的第一条爬虫，同时完成了老村长的出村任务，接下来不仅可以到达到更广阔的Python大陆，向更多的高手请教学习。同时获得了村长赠送的成长型职业装备“爬虫的培育手册”，可以让我能够在这片大路上通过冒险获得爬虫进阶的信息。

那接下来会有着什么样的冒险等着我呢？





