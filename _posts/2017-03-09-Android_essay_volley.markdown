---
layout:     post
title:      "Android随笔之Volley框架"
subtitle:   "网络通信框架Volley的简介"
date:       2017-03-09 18:20:00
author:     "Zicon"
header-img: "img/post-bg.jpg"
catalog: true
tags:
    - Android随笔
---

> “随便写写”


# 前言

我们平时在开发Android应用的时候不可避免地都需要用到网络技术，而多数情况下应用程序都会使用HTTP协议来发送和接收网络数据。Android系统中主要提供了两种方式来进行HTTP通信，HttpURLConnection和HttpClient，几乎在任何项目的代码中我们都能看到这两个类的身影，使用率非常高。

为了让这两个类的使用更加方便简洁，Volley框架应运而生，它不仅可以像AsyncHttpClient一样非常简单地进行HTTP通信，也可以像Universal-Image-Loader一样轻松加载网络上的图片。

它的设计目标就是非常适合去进行数据量不大，但通信频繁的网络操作，而对于大数据量的网络操作，比如说下载文件等，Volley的表现就会非常糟糕。

---

# 正文

当然，网上已经有了很多的关于Volley使用的例子了，所以Zicon也不啰嗦了，直接给出链接。
 * [ Android Volley完全解析(一)，初识Volley的基本用法](http://blog.csdn.net/guolin_blog/article/details/17482095) 
 * [ Android Volley完全解析(二)，使用Volley加载网络图片](http://blog.csdn.net/guolin_blog/article/details/17482165)
 * [ Android Volley完全解析(三)，定制自己的Request](http://blog.csdn.net/guolin_blog/article/details/17612763#t1)
 * [ Android Volley完全解析(四)，带你从源码的角度理解Volley](http://blog.csdn.net/guolin_blog/article/details/17656437) 

# 后记
当时自己做项目，接触网络技术的时候就是从郭霖大神的博客里学习的，虽然在之后也想过以自己的话表达Volley框架的独到之处，但是总是表达地不够好，所以最好还是直接放出链接就好了。





