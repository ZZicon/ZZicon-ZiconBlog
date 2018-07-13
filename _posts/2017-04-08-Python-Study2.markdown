---
layout:     post
title:      "Python物语的第三步"
subtitle:   "Python模块以及I/O"
date:       2017-04-08 18:26:00
author:     "Zicon"
header-img: "img/post-bg.jpg"
catalog: true
tags:
    - Python
---

> “Python模块以及基本的输入输出”

# 前言

Zicon终于进入了新手村，在村长的指引下接到新手任务，“帮助村头的流浪汉，教会他逻辑思维用法”以及“前往老铁匠处，他会教会你如何使用输入输出”。

---

# 正文

# Python 模块

**什么是Python模块？**

Python模块，就是一个Python文件，包含了Python对象定义和Python语句，如下例support.py：

```
def print_func( par ):
    print("Hello : ", par)
    return
```

**模块的引入**

***import语句***

模块定义好后，可以用import引入模块，语法如下：

```
import module1[, module2[,... moduleN]
```

要引用模块 math，就可以在文件最开始的地方用 import math 来引入。在调用 math 模块中的函数时，必须这样引用：

```
模块名.函数名
```

当解释器遇到import时，如果模块在当前搜索路径就会被导入。对模块位置的搜索顺序是：

 - 当前目录
 - 如果不在当前目录，搜索在shell 变量PYTHONPATH下的目录
 - 如果找不到，察看默认路径。UNIX下一般为/usr/local/lib/python/

一个模块只会被导入一次，不管你执行了多少次import。这样可以防止导入模块被一遍又一遍地执行。

***From…import语句***

Python 的 from 语句让你从模块中导入一个指定的部分到当前命名空间中。语法如下：

```
from modname import name1[, name2[, ... nameN]]
```

***From…import*语句***

把一个模块的所有内容全都导入到当前的命名空间也是可行的，只需使用如下声明：

```
from modname import *
```

**关于导入模块的方法**

***dir()函数***

dir()函数返回一个排好序的字符串列表，包含模块定义的所有模块，变量和函数。具体使用如下例：

```
#!/usr/bin/python
# -*- coding: UTF-8 -*-
 
# 导入内置math模块
import math
 
content = dir(math)
 
print content;
```

输出结果：

```
['__doc__', '__file__', '__name__', 'acos', 'asin', 'atan', 
'atan2', 'ceil', 'cos', 'cosh', 'degrees', 'e', 'exp', 
'fabs', 'floor', 'fmod', 'frexp', 'hypot', 'ldexp', 'log',
'log10', 'modf', 'pi', 'pow', 'radians', 'sin', 'sinh', 
'sqrt', 'tan', 'tanh']
```

在这里，特殊字符串变量__name__指向模块的名字，__file__指向该模块的导入文件名。

***globals()和locals()函数***

globals()和locals()用来返回全局和局部命名空间里的名字。

 - 如果在函数内部调用locals()，返回所有能在该函数里访问的命名。
 - 如果调用globals()，返回所有在该函数里能访问的全局名字。

返回类型都是字典。所以能用keys()函数摘取。

***reload() 函数***

当一个模块被导入到一个脚本，模块顶层部分的代码只会被执行一次。

因此，如果你想重新执行模块里顶层部分的代码，可以用 reload() 函数。该函数会重新导入之前导入过的模块。语法如下：

```
reload(module_name)
``` 

在这里，module_name要直接放模块的名字，而不是一个字符串形式。比如想重载 hello 模块，如下：

```
reload(hello)
``` 

**Python中的包**
 
包是分层次的文件目录结构，简单说就是文件夹，但该文件夹必须存在__init__.py文件，用于标识是包，文件内容可为空。具体使用如下例：

***步骤一、设置目录结构***
 
```
test.py
package_runoob
|-- __init__.py
|-- runoob1.py
|-- runoob2.py
```

***步骤二、编写package_runoob/runoob1.py***

```
#!/usr/bin/python
# -- coding: UTF-8 -- 
def runoob1():   
    print("I'm in runoob1")
```

***编写package_runoob/runoob2.py***

```
#!/usr/bin/python
# -- coding: UTF-8 -- 
def runoob1():   
    print("I'm in runoob2")
``` 

***步骤三、编写__init__.py***

```
#!/usr/bin/python
# -- coding: UTF-8 -- 
if __name__ == '__main__':
    print '作为主程序运行'
else:
    print 'package_runoob 初始化'
```

***步骤四、编写test.py***

```
#!/usr/bin/python
# -*- coding: UTF-8 -*-
 
# 导入 Phone 包
from package_runoob.runoob1 import runoob1
from package_runoob.runoob2 import runoob2
 
runoob1()
runoob2()
```

***步骤五、查看输出结果***

```
package_runoob 初始化
I'm in runoob1
I'm in runoob2
```

**Python内置属性及特殊模块**
 - __doc__：模块的docstring，文档字符串。
 - __file__：模块文件在磁盘上的绝对路径
 - __name__：模块的名称。独立运行时值是__main__，被import时值是模块的名称
 - __init__：用于初始化的特殊模块
 - __main__：常用__name__ == ’__main__‘语句来判断是否单独运行

#  Python基本的I/O

**输入**

***input函数***

```
#!/usr/bin/python3

str = input("请输入：");
print ("你输入的内容是: ", str)
```

上述内容转载自[RUNOOB.COM](http://www.runoob.com/python3/python3-tutorial.html)


# 后记
以上便是关于笔者的Python物语的第三步，新手教学就已经结束了，在这一章冒险中新手装备“Python模块以及I/O” 也是入手了，集齐了新手套装，激活称号“Python的小菜鸟” 吼吼吼。

在下一章的冒险中，Zicon将完成新手村出村任务，正式踏上打怪升级走上人生巅峰的道路 蛤~蛤~蛤~。




