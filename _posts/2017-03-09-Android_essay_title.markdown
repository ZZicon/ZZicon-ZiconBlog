---
layout:     post
title:      "Android随笔之标题栏"
subtitle:   "关于标题栏的去除以及自定义"
date:       2017-03-09 21:36:00
author:     "Zicon"
header-img: "img/post-bg.jpg"
catalog: true
tags:
    - Android随笔
---

> “Android标题栏随笔”


# 前言

相信很多安卓开发者都不喜欢系统默认的标题栏，那么如何让自定义一个自己的标题栏呢？

---

# 正文

首先，在Android中去掉activity的标题栏有两种方法。

 - 一个是在Activity代码里实现
 
```
@Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        //去掉标题栏
        requestWindowFeature(Window.FEATURE_NO_TITLE);
        setContentView(R.layout.activity_main);
    }
```
>记住：该代码要写在setContentView()前面。


 - 一个则是在AndroidManifest.xml文件里实现
 
```
<application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
		//去掉标题栏        
        android:theme="@android:style/Theme.NoTitleBar">
```

当然了，这样是将整个应用设置成无标题栏。如果只需要在一个Activity设置成一个无标题栏的形式，只要把代码写到某一个Activity里面就可以了。如：

```
<activity
            android:name=".MainActivity"
            android:theme="@android:style/Theme.NoTitleBar">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
```

然后，介绍如何自定义一个标题栏。

 - 首先，需要定义一个自定义的标题栏布局 xml文件
 
```
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="50dp"
    android:orientation="horizontal"
    android:background="#f2f8f8">
    //layout_height设置新标题栏的高度

    <TextView
        android:layout_centerInParent="true"
        android:layout_width="wrap_content"
        android:layout_height="match_parent"
        android:text="自定义标题栏"
        android:textSize="20sp"
        android:gravity="center_vertical"
        />

</RelativeLayout>
```

 - 然后，在Activity中声明使用自定义的标题栏
 
```
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        //声明使用自定义的标题栏
        requestWindowFeature(Window.FEATURE_CUSTOM_TITLE);
        setContentView(R.layout.activity_main);
        //将title.xml布局作为自定义标题栏
        getWindow().setFeatureInt(Window.FEATURE_CUSTOM_TITLE, R.layout.title);
    }
```

 - 当然，有时候会出现自定义标题栏两侧没有完全覆盖屏幕导致的不美观
这个时候，我们需要再定义一个style文件 MyStyle.xml

```
<?xml version="1.0" encoding="utf-8"?>
<resources xmlns:android="http://schemas.android.com/apk/res/android">

    <style name="titleBarStyle" parent="android:Theme">
        <item name="android:windowTitleSize">50dp</item><!-- 以你的自定义标题栏的高度为准 -->
        <item name="android:padding">0dp</item><!-- 使新的标题栏完全延伸到对齐到原始标题栏的两边 -->
    </style>
</resources>
```

并且，在 AndroidManifest.xml 中给activity 添加 一个theme属性

```
<activity
            android:name=".MainActivity"
            android:theme="@style/titleBarStyle">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
```

到这里基本就实现了自定义的标题栏。


# 后记
一个APP的美观以及交互性不仅体现在图标、UI界面上，标题栏也是一个很重要的点，因此，对于标题栏的自定义、优化也是必不可少的。




