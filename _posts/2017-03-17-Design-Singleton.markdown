---
layout:     post
title:      "设计模式——单例模式分析"
subtitle:   "最后一个创建型模式了呢"
date:       2017-03-17 13:24:00
author:     "Zicon"
header-img: "img/post-bg.jpg"
catalog: true
tags:
    - 设计模式
---

> “真相只有一个”


# 前言

单例模式确保类只有一个实例，而且自行实例化并向系统提供这个实例。它包括以下要素：

 - 私有的构造方法
 - 指向自己实例的私有静态引用
 - 以自己实例为返回值的静态的公有的方法
 
---

# 正文

**单例模式包含以下角色：**

***单例类***

单例类内部只生成一个实例，提供一个静态的工厂方法，让客户可以使用它的唯一实例。构造函数私有。

**单例模式的优缺点**

***优点***

 - 提供了对唯一实例的受控访问。单例类封装了它的唯一实例，所以它可以严格控制客户怎样以及何时访问它，并为设计及开发团队提供了共享的概念。
 - 由于内存中只存在一个对象，因此可以节约资源，对于需要频繁创建和销毁的对象，单例模式可以提高系统性能。
 - 允许可变数目的实例。我们可以基于单例模式进行扩展，使用与单例控制相似的方法来获得指定个数的对象实例。
 
***缺点***

 - 由于单例模式中没有抽象层，因此单例类扩展有很大困难。
 - 单例类职责过重，一定程度违背了单一职责原则。（单例类既充当了工厂角色，提供了工厂方法，又充当了产品角色，包含业务方法，将产品创建和本身功能融合到一起）
 - 滥用单例将带来负面问题，如为了节省资源将数据库连接池对象设计为单例类，可能会导致共享连接池对象的程序过多而出现连接池溢出。
 
**模式适用环境**

 - 系统只需要一个实例对象，如要求一个唯一序列号生成器，或需要考虑资源消耗而只允许创建一个对象。
 - 客户调用类的单个实例只允许使用一个公共访问点，除了该公共访问点，不能通过其他途径访问该实例。
 - 要求类只有一个实例时应用单例模式。如果类可以有几个实例共存，就需对单例模式改进，使之成为多例模式。

**恩，再来个栗子**

在操作系统中，打印池是一个用于管理打印任务的应用程序，通过打印池用户可以删除、中止或者改变任务的优先级，在一个系统中只允许运行一个打印池对象，如果重复创建打印池则抛出异常。现使用单例模式来模拟实现打印池的设计。

![](https://ZZicon.github.io/ZiconBlog/img/int_post/design_mode6.png)

首先是分类：

 - 单例类PrintSpoolerSingleton
 - 报错类PrintSpoolerException

**详细代码**

***PrintSpoolerSingleton 类***

```
public class PrintSpoolerSingleton {
	private static PrintSpoolerSingleton instance = null;

	private PrintSpoolerSingleton() {

	}

	// 运用双重检查锁定
	public static PrintSpoolerSingleton getInstance()
			throws PrintSpoolerException {
		// 第一重判断
		if (instance == null) {
			// 锁定代码块
			synchronized (PrintSpoolerSingleton.class) {
				// 第二重判断
				if (instance == null) {
					System.out.println("创建打印池。");
					instance = new PrintSpoolerSingleton();
				}
			}
		} else {
			throw new PrintSpoolerException("打印池正在工作...");
		}
		return instance;
	}

	public void manageJobs() {
		System.out.println("管理打印任务..");
	}

}
```

***PrintSpoolerException 类***

```
public class PrintSpoolerException extends Exception {
	public PrintSpoolerException(String message) {
		super(message);
	}
}
```

***Client 类***

```
public class Client {
	public static void main(String args[]) {
		PrintSpoolerSingleton ps1 = null, ps2 = null;
		try {
			ps1 = PrintSpoolerSingleton.getInstance();
			ps1.manageJobs();
		} catch (PrintSpoolerException e) {
			System.out.println(e.getMessage());
		}
		System.out.println("------------");
		try {
			ps2 = PrintSpoolerSingleton.getInstance();
			ps2.manageJobs();
		} catch (PrintSpoolerException e) {
			System.out.println(e.getMessage());
		}
	}
}
```

单例设计模式中，需要注意的是一个具有自动编号主键的表可以有多个用户同时使用，但数据库中只能有一个地方分配下一个主键编号，否则会出现主键重复，因此该主键编号生成器必须具备唯一性

因此我们需要程序使用同步锁。可参考PrintSpoolerSingleton 类的双重检查锁定。

**单例模式的使用**

单例模式主要有两种：饿汉式单例和懒汉式单例。前者在类加载时实例化单例对象；后者调用取得实例方法时实例化，使用同步化机制。

***饿汉式单例***

```
public class EagerSingleton {
	// 静态变量初始化的同时实例化单例
	private static final EagerSingleton instance = new EagerSingleton();

	private EagerSingleton() {

	}

	public static EagerSingleton getInstance() {
		return instance;
	}
}
``` 
 
***懒汉式单例***
 
```
public class LazySingleton {
	private static LazySingleton instance = null;

	private LazySingleton() {

	}

	// 同步锁-用于多线程
	synchronized public static LazySingleton getInstance() {
		if (instance == null) {
			instance = new LazySingleton();
		}
		return instance;
	}

}
```

饿汉式单例和懒汉式单例由于构造方法是private的，所以都不可继承，但是很多单例模式是可继承的，如登记式单例。


# 后记
单例模式是一种比较简单的模式，同时也是使用最广泛的模式之一，需要好好掌握。





