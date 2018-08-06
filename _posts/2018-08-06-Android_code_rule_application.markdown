---
layout:     post
title:      "Android代码规范的实际使用"
subtitle:   "Android开发编程规范的相关知识点"
date:       2018-08-06 10:17:00
author:     "Zicon"
header-img: "img/post-bg.jpg"
catalog: true
tags:
    - 规范约束
---

> “编程规范的实际使用”


# 前言

笔者之前就做过一些代码规范的总结，自己也在按照规范进行代码编程。

 - [编程语言格式规范](https://zzicon.github.io/ZiconBlog/2017/03/10/Java_code_rule/)
 - [Java代码命名规范](https://zzicon.github.io/ZiconBlog/2017/03/10/Java_name_rule/)

但以往都是在学校里自己编写代码，虽然也有遵循一定的规范，可是工作之后才发现自己在一些小细节方面还有是待优化滴。

在这里稍微记录一下被导师指出的疏漏。

---

# 正文

**常量的使用**
 
笔者以往在使用常量时都没有太在意常量的定义，直到被导师指出随意使用常量不仅不安全，而且还会让代码可读性变差。
 
例如：
 
```
int flag = 0;
if (true) {
    flag = 1;
} else {
    flag = 2;
}
if (flag == 1) {
    ...
} else {
    ...
}
```

大概就是这样的一个判断。这是笔者以前的写法，在导师指点后改为：

```
final int INSIDE = 1;
final int OUTSIDE = 2;
int position = 0;
if (true) {
    position = INSIDE;
} else {
    position = OUTSIDE;
}
if (position == INSIDE) {
    ...
} else {
    ...
}
```

这样编写不仅可以保证常量的稳定，不会被变更，同时会提高代码可读性，方便以后的维护工作。

**思维僵化 haha**

其实思维僵化说白了就是没有完全把学过的知识变成自己的东西，在做设计时没能灵活的实现功能模块。

比如有这么一个模块，要求是在一系列的布局控件中，当你点击其中一个控件时，隐藏其他的控件。你可以得到点击控件的id。

笔者马上想到的就是if else，在触发点击事件中，先隐藏全部控件，然后根据获得的id去显示对应的控件。大概就是这样：

```
a.setVisibility(View.GONE);
b.setVisibility(View.GONE);
c.setVisibility(View.GONE);
if (id = R.id.a_id) {
    a.setVisibility(View.VISIBILITY);
} else if (id = R.id.b_id) {
    b.setVisibility(View.VISIBILITY);
} else {
    c.setVisibility(View.VISIBILITY);
}
```

这样看上去还行，但是在实际界面上看起来会有一个，控件先集体消失再出现的、用户体验不好的情况。同时，代码也比较冗余。

因此，经导师引导后使用 “？：” 解决：

```
a.setVisibility(id = R.id.a_id ? View.VISIBILITY : View.GONE);
b.setVisibility(id = R.id.b_id ? View.VISIBILITY : View.GONE);
c.setVisibility(id = R.id.c_id ? View.VISIBILITY : View.GONE);
```

是不是突然有一种茅塞顿开的感觉，笔者当时心里想的也是：卧槽，居然没想起来还有这种操作。

这就是思维僵化的表现，“？：”方法是最最基础的方法，基本上在任何一本Java教材中都是属于基础知识，但是很多时候笔者都习惯性使用if、while而没有考虑到它。


笔者主要是对自己学习过的知识点的整理，所以之后应该会补充的。

---

# 后记
好了，这次主要是记录下自己在进行Android开发时经历的一些实际的代码规范、代码优化。

之后如果可能会更新的~