---
layout:     post
title:      "设计模式——工厂方法模式分析"
subtitle:   "真正开始接触第一个GoF设计模式"
date:       2017-03-13 18:46:00
author:     "Zicon"
header-img: "img/post-bg.jpg"
catalog: true
tags:
    - 设计模式
---

> “设计模式的入门”


# 前言

工厂方法模式是对简单工厂模式的进阶与优化，也是最简单的GoF设计模式。

---

# 正文

**工厂方法模式包含以下角色：**

***抽象工厂***

工厂方法模式的`核心`，声明了工厂方法；任何在模式中创建对象的工厂类都`必须实现`该接口。

***具体工厂***

抽象工厂类的子类，实现抽象工厂类中定义的工厂方法，包含与应用程序密切相关的`逻辑`。

***抽象产品***

抽象产品是定义产品的`接口`，也是`产品对象`的`父类或接口`。

***具体产品***

实现抽象产品的`接口`，具体产品由`具体工厂实现`。


**工厂方法模式的优缺点**

***优点***

 - 用户只需要关心所需产品对应的工厂，无需关心创建细节，甚至无需知道具体产品类的类名。

 - 能够使工厂可以自主确定创建何种产品对象，而如何创建这个对象的细节则完全封装在具体工厂内部。
 
 - 在系统中加入新产品时，只需添加一个具体工厂和具体产品即可，符合开闭原则。
 
***缺点***

 - 添加新产品时需要编写新的具体产品类，还要提供与之对应的具体工厂类，在一定程度上增加了系统复杂度。

 - 考虑到系统的扩展性，引入抽象层，在客户端代码中使用抽象层进行定义，增加抽象性与理解难度。

**简单工厂模式与OOP设计原则**

***简单工厂模式已遵循原则***

 - 依赖倒置原则
 - 迪米特法则
 - 里氏替换原则
 - 接口隔离原则
 - 单一职责原则
 - 开闭原则（如只是增加新的产品，而不是简单工厂那样需要修改已有的工厂时，只需增加相应的具体工厂类）
 
***简单工厂模式未遵循原则***

 - 开闭原则（虽然工厂对修改关闭了，但更换产品时，客户代码还是需要修改）


**模式适用环境**

 - 一个类不知道它所需要的对象的类：客户端不需要知道具体产品类的类名，只需要知道创建具体产品的工厂类。

 - 一个类通过子类来指定创建对象。
 
 - 将创建对象的任务委托给多个工厂子类中的某一个，客户端在使用时无需关心是哪一个工厂子类创建产品子类。


**举个例子咯~**

某系统日志记录器要求支持多种日志记录方式，如文件记录、数据库记录等，且用户可以根据要求动态选择日志记录方式。

![](https://ZZicon.github.io/ZiconBlog/img/int_post/design_mode2.png)

首先是分类：

 - 抽象产品类Log
 - 具体产品类FileLog、DatabaseLog
 - 抽象工厂类LogFactory
 - 具体工厂类FileLogFactory、DatabaseLogFactory。


**详细代码**

***Log类***

```
public abstract class Log {
	public abstract void writeLog();
}
```

***FileLog类***

```
public class FileLog extends Log {
	@Override
	public void writeLog() {
		System.out.println("writing file log...");
	}
}
```

***DatabaseLog类***

```
public class DatabaseLog extends Log {
	@Override
	public void writeLog() {
		System.out.println("writing database log...");
	}
}
```

***LogFactory类***

```
public abstract class LogFactory {
	public abstract Log createLog();
}
```

***FileLogFactory类***

```
public class FileLogFactory extends LogFactory {
	@Override
	public Log createLog() {
		return new FileLog();
	}
}
```

***DatabaseLogFactory类***

```
public class DatabaseLogFactory extends LogFactory {
	@Override
	public Log createLog() {
		return new DatabaseLog();
	}
}
```

**XMLUtils类—使用DOM和Java反射机制读取XML信息**

```
public class XMLUtils {
	public static String getClassName() {
		try {
			DocumentBuilderFactory factory = DocumentBuilderFactory.newInstance();
			DocumentBuilder builder = factory.newDocumentBuilder();
			Document doc = builder.parse(new File("src/config.xml"));
			Node classNode = doc.getElementsByTagName("class").item(0).getFirstChild();
			String type = classNode.getNodeValue().trim();
			return type;
		} catch (Exception e) {
			System.out.println(e.getMessage());
		}
		return null;
	}
}
```

***Client类—输出结果***

```
public class Client {
	public static void main(String[] args) {
		try {
			Class<?> c = Class.forName(XMLUtils.getClassName());
			LogFactory factory = (LogFactory) c.newInstance(); 
			factory.createLog().writeLog();
		} catch (ClassNotFoundException e) {
			System.out.println("can not find class!");
		} catch (Exception e) {
			System.out.println(e.getMessage());
		}
	}
}
```


# 后记
工厂方法模式作为GoF的第一个模式，是在简单工厂模式的基础上有了简单的优化，但仍然存在着不少的问题。那GoF之后的设计模式能完善它吗。接下来，Zicon将讲解抽象工厂模式。



