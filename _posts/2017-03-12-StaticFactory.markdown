---
layout:     post
title:      "设计模式——简单工厂模式分析"
subtitle:   "先来聊聊简单的设计模式"
date:       2017-03-12 18:36:00
author:     "Zicon"
header-img: "img/post-bg.jpg"
catalog: true
tags:
    - 设计模式
---

> “设计模式的门扉”


# 前言

简单工厂模式是学习设计模式的基础，是最简单的一种设计模式，因此也先从其开始学习吧。

---

# 正文

**简单工厂模式包含以下角色：**

***工厂类***

简单工厂模式的`核心`，负责实现创建所有实例的`内部逻辑`；工厂类可以被`外界直接调用`，创建所需的产品对象。

***抽象产品角色***

简单工厂模式所创建的所有对象的`父类`，负责`描述`所有实例共有的`公共接口`，提高系统灵活性。

***具体产品角色***

简单工厂模式的`创建目标`，继承·抽象类，需要`实现`定义在抽象类中的`抽象方法。`

**简单工厂模式的优缺点**

***优点***

 - 客户端无需知道所创建的具体产品类的类名，只需要知道具体产品类所对应的参数即可。

 - 通过引入配置文件，可以在不修改客户端代码的情况下更换和增加新的具体产品类，提高系统灵活度。

***缺点***

 - 工厂类集中了所有产品创建逻辑，一旦工厂类出现问题，整个系统都要受到影响。

 - 该模式会增加系统的类的个数，增加了系统复杂度和理解难度。

 - 当定义时使用了父类，即使实例化的是子类也无法访问子类的静态方法。例如：

 ```
class First {
	public static void display() {
		System.out.println("这是父类");
	}
}

class Second extends First {
	public static void display() {
		System.out.println("这是子类");
	}
}

public class Client{
	public static void main(String args[]){
		First demo;
		demo = new Second();
		demo.display();
	}
}
```

输出结果显示，子类的方法无法被访问。

**简单工厂模式与OOP设计原则**

***简单工厂模式已遵循原则***

 - 依赖倒置原则
 - 迪米特法则
 - 里氏替换原则
 - 接口隔离原则

***简单工厂模式未遵循原则***

 - 开闭原则（利用配置文件+反射或注解可避免这一点）
 - 单一职责原则（工厂类即要负责逻辑判断又要负责实例创建）

**模式适用环境**

 - 工厂类负责创建的对象比较少，不会造成工厂方法中业务逻辑过于复杂。
 
 - 客户端只知道传入工厂类的参数，对于如何创建对象不关心。

***举个例子咯~***

使用简单工厂模式设计一个可以创建不同几何形状的绘图工具。 

![](https://ZZicon.github.io/ZiconBlog/img/int_post/design_mode1.png)

首先是分类:

 - 工厂类ShapeFactory
 
 - 抽象类Shape
 
 - 具体类Circle、Rectangle、Triangle。

***详细代码***

PS：简单工厂模式中抽象类可以以接口或抽象类实现，笔者在这里以接口形式实现。

***Shape类***

```
public interface Shape {
	void draw();
	void erase();
}
```

***Circle类 实现Shape接口***

```
public class Circle implements Shape {
	@Override
	public void draw() {
		System.out.println("draw Circle...");	
	}
	
	@Override
	public void erase() {
		System.out.println("erase Circle...");	
	}
}
```

***Rectangle类 实现Shape接口***

```
public class Rectangle implements Shape {
	@Override
	public void draw() {
		System.out.println("draw Rectangle...");	
	}
	
	@Override
	public void erase() {
		System.out.println("erase Rectangle...");	
	}
}
```

***Triangle类 实现Shape接口***

```
public class Triangle implements Shape {
	@Override
	public void draw() {
		System.out.println("draw Triangle...");	
	}
	
	@Override
	public void erase() {
		System.out.println("erase Triangle...");	
	}
}
```

***ShapeFactory类实现产品创建***
***以及自定义异常类UnsupportedShapeException ***

```
public class ShapeFactory {
	public static Shape createShape(String type) throws UnsupportedShapeException {
		if ("circle".equalsIgnoreCase(type)) {
			return new Circle();
		}
		if ("rectangle".equalsIgnoreCase(type)) {
			return new Rectangle();
		}
		if ("triangle".equalsIgnoreCase(type)) {
			return new Triangle();
		}
		
		throw new UnsupportedShapeException("unsupported shape!");
	}
	
	public static void main(String[] args) {
		String[] types = { "circle", "rect", "rectangle", null, "triangle" };
		for (String type : types) {
			try {
				Shape shape = createShape(type);
				shape.draw();
				shape.erase();
			} catch (UnsupportedShapeException e) {
				System.out.println(e.getMessage());
			}
		}
	}
}

class UnsupportedShapeException extends Exception {
	public UnsupportedShapeException(String msg) {
		super(msg);
	}
}
```

# 后记
由于这是第一个设计模式，介绍相对十分详细而啰嗦，在之后更新其他设计模式时会尽量高效简洁。



