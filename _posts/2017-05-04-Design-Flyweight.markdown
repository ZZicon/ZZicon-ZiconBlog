---
layout:     post
title:      "设计模式——享元模式分析"
subtitle:   "降低开销提高性能的设计模式"
date:       2017-05-04 14:08:00
author:     "Zicon"
header-img: "img/post-bg.jpg"
catalog: true
tags:
    - 设计模式
---

> “重复的东西多了就该好好整理才行啊”


# 前言

面向对象技术可以解决一些灵活性或可扩展性的问题，但需要在系统中增加类和对象的个数。当对象数量太多时，将导致运行代价过高，带来性能下降等问题。为了解决这个问题，我们引入享元模式这一结构型模式。

在享元模式中：将对象之间封装好的，并且可以进行共享的这部分相同内容称为`内部状态`;而将那些需要外部环境来设置的不能共享的内容称为`外部状态`；一般享元模式中需要创建一个享元工厂维护一个享元池。

---

# 正文

**享元模式包含以下角色：**

***抽象享元类（Flyweight）***

声明了一个可以`作用于外部状态`的接口。同时定义了具体享元类`公共`的方法，这些方法可以向外界提供享元对象的内部数据（状态），也可以通过这些方法设置外部数据（状态）。

***具体享元类（ConcreteFlyweight）***

实现抽象享元类的接口。为内部状态提供`存储空间`。

***非共享具体享元类（UnsharedConcreteFlyweight）***

当需要非共享具体享元类的对象时直接通过`实例化创建`。非共享具体享元对象还可以将具体享元对象作为子节点。

***享元工厂类（FlyweightFactory）***

用于`创建并管理`享元对象。将各类型具体享元对象存储在一个享元池中。

**享元模式的优缺点**

***优点***

 - 极大减少内存中对象的数量，使得相同对象或相似对象在内存中只保存一份。
 - 享元模式的外部状态相对独立，且不影响其内部状态，使得享元对象可以在不同环境中被共享。
 
***缺点***

 - 享元模式使系统更加复杂，需要分离出内部状态和外部状态，使得程序的逻辑复杂化。
 - 为了使对象可以共享，享元模式需将享元对象的状态外部化，而读取外部状态使得运行时间变长。
 
**模式适用环境**

 - 一个系统有大量相同或相似对象，由于这类对象的大量使用，造成内存大量耗费。
 - 对象的大部分状态都可以外部化，可以将这些外部状态传入对象中。
 - 使用享元模式需维护一个存储享元对象的享元池，而这要耗费资源，因此应当在多次使用享元对象时才使用享元模式。
  
**享元模式栗子吼**

一个围棋软件，在系统中只存在一个白棋对象和一个黑棋对象，但是他们可以在棋盘的不同位置显示多次。请使用享元模式设计该软件，并写出相应Java代码。

使用简单工厂模式和单例模式实现享元工厂类的设计。

首先是分类：

 - 抽象享元类IgoChessman
 - 外部状态类Coordinate
 - 具体享元类BlackIgoChessman、WhiteIgoChessman
 - 工厂类IgoChessmanFactory

**详细代码**

```
import java.util.Hashtable;

//抽象享元类
interface IgoChessman {
	public String getColor();

	public void display(Coordinate cood);
}

// 外部状态类
class Coordinate {
	private String color;
	private int x, y;

	public Coordinate(int x, int y) {
		this.x = x;
		this.y = y;
	}

	public void setCoordinate(int x, int y) {
		this.x = x;
		this.y = y;
	}

	public String getCoordinate() {
		return "棋子的位置：" + x + "," + y;
	}
}

// 具体享元类
class BlackIgoChessman implements IgoChessman {

	private String color;

	public BlackIgoChessman() {
		this.color = "黑色";
	}

	public String getColor() {
		return this.color;
	}

	public void display(Coordinate cood) {
		System.out
				.println("棋子颜色：  " + this.color + ", " + cood.getCoordinate());
	}

}

class WhiteIgoChessman implements IgoChessman {

	private String color;

	public WhiteIgoChessman() {
		this.color = "白色";
	}

	public String getColor() {
		return this.color;
	}

	public void display(Coordinate cood) {
		System.out
				.println("棋子颜色：  " + this.color + ", " + cood.getCoordinate());
	}

}

// 工厂类
class IgoChessmanFactory {

	private static IgoChessmanFactory instance = new IgoChessmanFactory();
	private Hashtable ht = new Hashtable();

	public IgoChessmanFactory() {
		BlackIgoChessman black = new BlackIgoChessman();
		WhiteIgoChessman white = new WhiteIgoChessman();
		ht.put("b", black);
		ht.put("w", white);
	}

	public IgoChessman getIgoChessman(String key) {
		if (key.equals("b")) {
			return (IgoChessman) ht.get("b");
		} else if (key.equals("w")) {
			return (IgoChessman) ht.get("w");
		} else {
			return null;
		}
	}

	public static IgoChessmanFactory getInstance() {
		return instance;
	}
}

public class Client {
	public static void main(String[] args) {
		IgoChessman black1, black2, black3, white1, white2;
		IgoChessmanFactory factory = IgoChessmanFactory.getInstance();
		black1 = factory.getIgoChessman("b");
		black2 = factory.getIgoChessman("b");
		black3 = factory.getIgoChessman("b");
		white1 = factory.getIgoChessman("w");
		white2 = factory.getIgoChessman("w");

		System.out.println("两颗黑棋是否相同：" + (black1 == black2));
		System.out.println("两颗白棋是否相同：" + (white1 == white2));

		black1.display(new Coordinate(1, 2));
		black2.display(new Coordinate(1, 3));
		black3.display(new Coordinate(1, 4));
		white1.display(new Coordinate(2, 2));
		white2.display(new Coordinate(2, 3));
	}
}
```

**享元模式拓展**

享元模式与其他模式的联用：

 - 享元工厂类中通常提供静态工厂方法用于返回享元对象，使用简单工厂模式生成享元对象。
 - 在系统中，享元工厂通常唯一，可以使用单例模式设计享元工厂类。
 - 享元模式可以结合组合模式形成复合享元模式，统一对享元对象设置外部状态。

结果如图：

![](https://ZZicon.github.io/ZiconBlog/img/int_post/design_mode13.png)


# 后记
享元模式对于系统性能的提高有很大的帮助作用，我们必须掌握该设计模式的实现思路以及适用情况~