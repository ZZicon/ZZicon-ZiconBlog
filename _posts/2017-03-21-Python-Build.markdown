---
layout:     post
title:      "Python物语的第一步"
subtitle:   "编写Python程序的环境搭建"
date:       2017-03-21 18:36:00
author:     "Zicon"
header-img: "img/post-bg.jpg"
catalog: true
tags:
    - Python
---

> “接触一下Python”

# 前言

本系列博客是Zicon对Python的爬虫感兴趣进行学习的总结。

学习Python，先从学会搭建开始。

---

# Python的下载

Zicon在这里学习的是Python3.0。下载网址[点击这里](https://www.python.org/)

在官网中鼠标移至Downloads，而后点击下拉菜单的Python 3.6.0即可下载Python。

![](https://ZZicon.github.io/ZiconBlog/img/int_post/Python/PythonDownloads.png)

Python的安装请[参考这里](http://blog.csdn.net/qq_30706581/article/details/56666522)

# 编写Python的环境

毕竟Zicon之前已经使用Eclipse进行过很长时间的编程了，所以优先考虑的是Eclipse有没有能满足Python编程使用的插件。果然，稍微查了一下就发现了PyDev这个插件。

**连网下载**

在Eclipse中点击Help -> Install New Software。

![](https://ZZicon.github.io/ZiconBlog/img/int_post/Python/PyDev1.1.png)

然后，在弹出的窗口中点击Add，然后在Name处随意填写，如PyDev，在Location处填写http://pydev.org/updates。点击ok。

![](https://ZZicon.github.io/ZiconBlog/img/int_post/Python/PyDev1.2.png)

在Install窗口里选择PyDev，只用勾选里面的PyDev for Eclipse即可。

![](https://ZZicon.github.io/ZiconBlog/img/int_post/Python/PyDev1.3.png)

等待下载完成，中途会有提示框问是否信任下载，框选后选择ok。

![](https://ZZicon.github.io/ZiconBlog/img/int_post/Python/PyDev1.4.png)

下载完成之后重启Eclipse。在Eclipse中点击Windows -> Preferences，如果出现PyDev选项则说明安装成功。

**本地安装**

如果你安装PyDev的过程中下载速度正常，安装成功，请跳过本段信息~

***下载速度慢...***

可以到[这个网站](https://sourceforge.net/projects/pydev/files/pydev/)直接下载资源。

进入网站后选择PyDev。

![](https://ZZicon.github.io/ZiconBlog/img/int_post/Python/PyDevnDownloads1.png)

点击选择对应的PyDev版本后进入选择下载页面，只要下载名称中不带sources的即可。

![](https://ZZicon.github.io/ZiconBlog/img/int_post/Python/PyDevnDownloads.png)

下载后把压缩包里面的plugins中的文件解压到Eclipse安装目录下plugins文件夹中，压缩包里面features中的文件目录也是同样操作。之后重启Eclipse。

***安装不成功！***

Zicon第一次安装时也出现了没有PvDev出现的情况。经过一系列的百度...大概是说JDK版本在1.7以下的就不要下载太新的PvDev了，也有可能与java版本或Jre等问题。不过下载pydev2.8.2可行，在尝试了之后果然可以。

不过，要注意的是，在下载新的PyDev插件之前，一定要把旧的PyDev插件删除，否则即使成功也不会显示PyDev。至于怎么删除，[请看](http://jingyan.baidu.com/article/54b6b9c02dcd5a2d583b4704.html)

# 配置Python

PyDev安装好之后，需要配置解释器。前提是你已经下载好Python了。

在 Eclipse 菜单栏中，选择Window -> Preferences -> Pydev -> Python Interpreter -> New 。

![](https://ZZicon.github.io/ZiconBlog/img/int_post/Python/PyDev1.5.png)

进入对话框。Interpreter Name可以随便命名，Interpreter Executable选择python.exe。

![](https://ZZicon.github.io/ZiconBlog/img/int_post/Python/PyDev1.6.png)

点击OK确定后跳出一个窗口Selection needed，点击OK即可。然后在Python Interpreters的窗口，再次点击OK，即完成了Python解释器的配置。

![](https://ZZicon.github.io/ZiconBlog/img/int_post/Python/PyDev1.7.png)

![](https://ZZicon.github.io/ZiconBlog/img/int_post/Python/PyDev1.8.png)

至此，在Eclipse编写Python程序的准备已经完成了。

# Python程序

新建第一个工程 File -> New -> Other 。

![](https://ZZicon.github.io/ZiconBlog/img/int_post/Python/PythonCreate.png)

弹出Select a wizard对话框，选Pydev Project，点Next。

![](https://ZZicon.github.io/ZiconBlog/img/int_post/Python/PythonCreate1.png)

弹出一个对话框，我们填写上新建工程的名称，选择Interpreter的版本，然后点Finish。

![](https://ZZicon.github.io/ZiconBlog/img/int_post/Python/PythonCreate2.png)

按创建普通项目的步骤，创建包，创建Python文件。

然后，第一个Python程序。hello python哈哈哈。

![](https://ZZicon.github.io/ZiconBlog/img/int_post/Python/PythonCreate4.png)


# 后记
以上便是关于笔者的Python物语的第一步--新手教学，在这一章冒险中新手装备“第一个Python程序” Get！。




