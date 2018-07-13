---
layout:     post
title:      "Android随笔之页面跳转"
subtitle:   "页面跳转 保持状态不变"
date:       2017-04-06 14:21:23
author:     "Zicon"
header-img: "img/post-bg.jpg"
catalog: true
tags:
    - Android随笔
---

> “Android页面跳转随笔”


# 前言

相信部分安卓开发者在刚开始接触页面跳转的时候，都会对页面跳转之间传值、返回操作又爱又恨。

---

# 正文

在这里，Zicon就讲一下自己在项目中遇到了一些小问题。

 - 1、Android如何让App在切换到后台后，再次打开App显示的是退出时的页面。
 
`在Manifest.xml中application标签内设置该页面的启动模式为singleInstance`

```
android:launchMode="singleInstance"
```  

 - 2、在页面跳转之后，重新返回跳转前的页面保证里面的数据不要重新加载。
 
`在第一个页面写跳转操作时不要销毁该页，且在Manifest.xml中application标签内设置该页面的启动模式为singleInstance或singleTask`

```
android:launchMode="singleInstance"

android:launchMode="singleTask"
```

`当然，最简单的就是你在第二个页面返回第一个页面时不适用跳转操作，而是直接关闭第二个页面，也可实现数据不重新加载。` 

上面就是我对于页面跳转的一些见解。


# 后记
当然了这些都只是一些简单的方法，也许会有着其他的方法，如果在之后的学习中掌握到新方法的话我也会进行更新。




