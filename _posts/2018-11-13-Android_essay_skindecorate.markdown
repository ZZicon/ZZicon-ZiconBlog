---
layout:     post
title:      "Android随笔之应用换肤"
subtitle:   "关于Android的暴力换肤"
date:       2018-11-13 9:17:00
author:     "Zicon"
header-img: "img/post-bg.jpg"
catalog: true
tags:
    - Android随笔
---

> “Android换肤哈”


# 前言

今天刚起来就又看到一条坏消息，斯坦李老爷子逝世了。8102年就这么艰难吗，即没了江湖，还要没了宇宙。

---

# 正文

这次，笔者将要对Android应用换肤的功能进行记录。

换肤，其实可以分为：切换app颜色风格以及少量图片的“切换主题”，和更换大量图片背景的“换肤”。

 - 切换主题：在代码中实现多套<style>主题，然后通过setTheme(int id)进行切换
 - 换肤：打包皮肤包，解析文件对相关文件进行更换
 
 大家想更详细了解，推荐[Android 主题切换](https://blog.csdn.net/utomi/article/details/72898180)

 而笔者本次打算记录下来的是一个暴力切换方法，主要是应用仅仅需要两套主题的对比，以及相关资源文件不多，所以才出此下策，如果想更好的实现换肤并且减少资源大小，最好还是使用框架。
 
 **第一步**
 
 在应用中放入两个主题的图片资源，并且对一些控件的selector.xml文件也创建两份。
 
 **第二步**
 
 新建换肤的类，在类里面实现view设置背景或者selector效果。例如：
 
 ```
 public void decorateRL(Context context, View view, TextView textView) {
        if (主题一) {
			textView.setTextColor(context.getResources().getColor(R.color.主题一_color));
            view.setBackgroundResource(R.drawable.主题一.switch_bg_selector);
        } else {
			textView.setTextColor(context.getResources().getColor(R.color.主题二_color));
            view.setBackgroundResource(R.drawable.主题二.switch_bg_selector);
        }
    }
 ```
 
 **第三步**
 
 在控件初始化或调用时，对其进行换肤操作。当然，如果是对于自定义的控件，可以在自定义类里增加一个设置资源的方法，然后在换肤类中对齐进行调用。
 
---

# 后记
好了，这次主要是记录下自己在进行Android开发时换肤的实现方式。本来想同时记录一些换肤框架的，最近心态有点不好，就到这里结束吧。
