---
layout:     post
title:      "在Cygwin下Hadoop安装经历"
subtitle:   "正式说明Hadoop的安装咯"
date:       2017-03-19 21:16:00
author:     "Zicon"
header-img: "img/post-bg.jpg"
catalog: true
tags:
    - Cygwin
---

> “偷懒的最后一步”


# 前言

之前已经通过 [Windows下Cygwin的安装经历](https://zzicon.github.io/ZiconBlog/2017/03/19/Cygwin/) 搞定了Cygwin的安装、配置了，接下来就是正式安装Hadoop了。

---

# 正文

**第一步准备**

解压Hadoop，放到Cygwin的/home目录下。

**Hadoop的配置**

需要修改hadoop的配置文件，它们位于conf子目录下，分别是hadoop-env.sh、core-site.xml、hdfs-site.xml 和mapred-site.xml

***core-site.xml***

```
<configuration>     
<property>  
  <name>fs.default.name</name>  
  <value>hdfs://localhost:9000</value>  
</property>  
</configuration>  
```

***hdfs-site.xml***

```
<configuration>     
<property>  
  <name>dfs.replication</name>  
  <value>1</value>  
</property>  
</configuration>  
```

***mapred-site.xml***

```
<configuration>     
<property>  
  <name>mapred.job.tracker</name>  
  <value>localhost:9001</value>  
</property>  
</configuration> 
```

**Hadoop的格式化并启动**

 - 输入cd hadoop/bin
 - 输入 ./hadoop namenode –format
 - 输入 ./start-all.sh
 - 输入 jps查看Hadoop进程


# 后记
这下，在Windows下安装Hadoop的说明就到此为止了。


