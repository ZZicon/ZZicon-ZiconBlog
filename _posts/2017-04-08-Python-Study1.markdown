---
layout:     post
title:      "Python物语的第二步"
subtitle:   "学习Python程序的基础"
date:       2017-04-08 14:26:00
author:     "Zicon"
header-img: "img/post-bg.jpg"
catalog: true
tags:
    - Python
---

> “Python入门”

# 前言

本系列博客是Zicon对Python的爬虫感兴趣进行学习的总结。

既然已经搭建完了Python环境，那接下来得去了解它咯。

---

# 正文

学习Python首先要知道Python的基础知识，比如Python有多少种数据类型、常用的数据结构是哪些、Python的函数如何定义等。

在这篇文章中Zicon就主要讲述这些知识点。

# Python变量类型

Python中变量的赋值不需要声明类型，但是每个变量在使用前都必须赋值。值得一提的是在Python里，双引号与单引号的作用是一样。

例如：实例中，1，10.0和"Zicon"分别赋值给num，miles，name变量。

```
num = 1
miles = 10.0
name = 'Zicon'
```

# Python标准数据类型

 - Numbers（数字）
 - String（字符串）
 - List（列表）
 - Tuple（元组）
 - Dictionary（字典）
 - Set（集合）
 
**数字**

Python支持四种不同的数字类型：int（有符号整型）、long（长整型[也可以代表八进制和十六进制]）、float（浮点型）、complex（复数）

**字符串**

字符串或串(String)是由数字、字母、下划线组成的一串字符。

**列表**

List是 Python 中使用最频繁的数据类型。它支持字符，数字，字符串甚至可以包含列表。具体操作如下：

```
list1 = ['Zicon', 123 , 4.56, 'python']
list2 = [789, 'john']

print (list1)               # 输出完整列表
print (list2 * 2)           # 输出列表两次
print (list1 + list2)       # 打印组合的列表
print (list1[0])            # 输出列表的第一个元素
print (list1[1:3])          # 输出第二个至第三个的元素 
print (list1[2:])           # 输出从第三个开始至列表末尾的所有元素
print (list1[-1])           # 输出列表的最后一个元素

list1.insert(1, 'hello')    #在列表第2位插入“hello”
list1.append('hi')          #在列表最后插入“hi”
print(list1)
list1.pop()                 #弹出列表尾元素
print(list1)
list1.pop(2)                #弹出指定列表元素
print(list1)
list1[1] = 'bye'
print(list1)                #修改指定列表元素
```

**元组**

tuple和list非常类似，但是tuple一旦初始化就不能修改，具体操作如下：

```
tuple = ('Zicon', 123 , 4.56, 'python')

print(tuple)            
tuple[1] = 'who'        #报错，无法修改
```


**字典**

dict（字典）使用键-值（key-value）存储，具有极快的查找速度。具体操作如下：

```
dict = {}
dict['one'] = "This is one"
dict[2] = "This is two"
 
tinydict = {'name': 'john', 'code': 6734, 'dept': 'sales'}

print (dict['one'])         # 输出键为'one' 的值
print (dict[2])             # 输出键为 2 的值
print (tinydict)            # 输出完整的字典
print (tinydict.keys())     # 输出所有键
print (tinydict.values())   # 输出所有值
tinydict.pop('dept')        #删除指定元素
print (tinydict)   
tinydict['name'] = 'Zicon'  #修改指定元素
print (tinydict) 
```

**集合**

set和dict类似，也是一组key的集合，但不存储value。由于key不能重复，所以，在set中，没有重复的key。set是无序的。set可以看成数学意义上的无序和无重复元素的集合，因此，两个set可以做数学意义上的交集、并集等操作。具体操作如下：

```
set1 = set([1,3,5,7,9])
set2 = set([2,4,6,8,10])

set1.add(4)                 # 在集合最后添加元素
set2.remove(6)              # 删除集合元素
print(set1)                 # 输出完整集合
print(set2)                 # 输出完整集合
print(set1 & set2)          # 输出集合交集
print(set1 | set2)          # 输出集合并集
```

**可变对象与不可变对象**

可变对象与不可变对象的区别在于对象内容是否可变。不可变对象可以减少重复的值对内存空间的占用，但是在修改时需要重新开辟一块内存，把新地址与变量名绑定，执行效率带来一定的降低。

 - 可变对象：字典型(dictionary)、列表型(list)、集合型(set)
 - 不可变对象：int、float、long、string、字符串(string)、数值型(number)、元组(tuple)

**PS：变量赋值 a=5 后再赋值 a=10，这里实际是新生成一个 int 值对象 10，再让 a 指向它，而 5 被丢弃，不是改变a的值，相当于新生成了a。**

# Python函数

**函数定义**

 - 函数代码块以 def 关键词开头，后接函数标识符名称和圆括号()。
 - 任何传入参数和自变量必须放在圆括号中间。圆括号之间可以用于定义参数。
 - 函数的第一行语句可以选择性地使用文档字符串—用于存放函数说明。
 - 函数内容以冒号起始，并且缩进。
 - return [表达式] 结束函数，选择性地返回一个值给调用方。不带表达式的return相当于返回 None。

**函数调用**
 
定义一个函数只给了函数一个名称，指定了函数里包含的参数，和代码块结构。
这个函数的基本结构完成以后，你可以通过另一个函数调用执行。 
具体操作如下：

```
def printsth(str):
    print('函数创建')
    print(str)
    return

printsth('函数调用')
```

# 后记
以上便是关于笔者的Python物语的第二步，嗯，还是新手教学，在这一章冒险中新手装备“Python的基础知识” Get！。

在下一章的冒险中，Zicon终于可以在新手村领取任务，接触更多Python的相关知识了，敬请期待。




