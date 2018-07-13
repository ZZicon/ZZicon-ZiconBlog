---
layout:     post
title:      "设计模式——抽象工厂模式分析"
subtitle:   "GoF的最后一个工厂模式"
date:       2017-03-13 19:46:00
author:     "Zicon"
header-img: "img/post-bg.jpg"
catalog: true
tags:
    - 设计模式
---

> “工厂模式的最终形式”


# 前言

抽象工厂模式是工厂方法模式的泛化版，工厂方法模式是一种特殊的抽象工厂模式。

工厂方法模式的每个工厂只生产一类具体产品，导致存在大量工厂类，增加系统开销，此时可以考虑使用抽象工厂模式。

---

# 正文

**抽象工厂模式包含以下角色：**

***抽象工厂***

抽象工厂声明生成抽象产品的一组方法，每一个方法对应一个`产品等级`。

***具体工厂***

实现了抽象工厂声明的方法，生成具体产品，每一个产品都位于某个产品`等级结构`中。

***抽象产品***

抽象产品为具体产品声明`接口`，定义了产品的抽象业务方法。

***具体产品***

定义具体工厂生产的具体产品对象，实现抽象产品`接口`定义的业务方法。


**工厂方法模式的优缺点**

***优点***

 - 隔离了具体类的生成，并且实现高内聚低耦合的设计目的。
 
 - 增加新的具体工厂和产品很方便，无需修改已有系统，符合“开闭原则”。
 
***缺点***

 - 添加新的产品对象时，难以扩展抽象工厂来生产新种类的产品。
 
 - 因为在抽象工厂中规定了所有可能被创建的产品集合，要支持新种类的产品意味着要对该接口进行扩展，而这将涉及对抽象工厂及其所有子类的修改，有很大的不便。
 
**简单工厂模式与OOP设计原则**

***简单工厂模式已遵循原则***

 - 依赖倒置原则
 - 迪米特法则
 - 里氏替换原则
 - 接口隔离原则
 - 单一职责原则
 - 开闭原则（在抽象工厂模式中，增加新的产品族很方便，只需增加相应的具体工厂类）
 
 
***简单工厂模式未遵循原则***

 - 开闭原则（增加新的产品等级结构很麻烦，需要修改所有的工厂角色，并且在所有的工厂类中都需要增加生产新产品的方法）
 
 - 称之为`“开闭原则”的倾斜性`，以一种倾斜的方式来满足“开闭原则”，为增加新产品族提供方便，但不能为增加新产品结构提供这样的方便


**模式适用环境**

 - 用户无需关心对象的创建过程，对象的创建和使用解耦。
 
 - 产品中有多于一个产品族，而每次只使用其中某一个产品族。
 
 - 属于同一个产品族的产品将在一起使用。
 
 - 系统提供一个产品类的库，所有的产品以同样的接口出现，从而使客户端不依赖于具体实现。

**又是一个栗子**


某系统为了改进数据库操作的性能，自定义数据库连接对象Connection和语句对象Statement，可针对不同类型的数据库提供不同的连接对象和语句对象

![](https://ZZicon.github.io/ZiconBlog/img/int_post/design_mode3.png)

首先是分类：

 - 抽象产品类Connection、Statement
 - 具体产品类OracleConnection、MySQLConnection、OracleStatement、MySQLStatement
 - 抽象工厂类DBFactory
 - 具体工厂类OracleFactory、MySQLFactory


**详细代码**

***Connection类***

```
public interface Connection {
}
```

***Statement 类***

```
public interface Statement {
}
```

***MySQLConnection 类***

```
public class MySQLConnection implements Connection {
}
```

***MySQLStatement 类***

```
public class MySQLStatement implements Statement {
}
```

***OracleConnection 类***

```
public class OracleConnection implements Connection {
}
```

***OracleStatement 类***

```
public class OracleStatement implements Statement {
}
```

***DBFactory 类***

```
public interface DBFactory {
	Connection createConnection();
	Statement createStatement();
}
```

***MySQLFactory 类***

```
public class MySQLFactory implements DBFactory {
	@Override
	public Connection createConnection() {
		return new MySQLConnection();
	}
	
	@Override
	public Statement createStatement() {
		return new MySQLStatement();
	}
}
```

***OracleFactory类***

```
public class OracleFactory implements DBFactory {
	@Override
	public Connection createConnection() {
		return new OracleConnection();
	}
	
	@Override
	public Statement createStatement() {
		return new OracleStatement();
	}
}
```

***XMLUtils类***

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

***Client类***

```
public class Client {
	public static void main(String[] args) {
		try {
			Class<?> c = Class.forName(XMLUtils.getClassName());
			DBFactory factory = (DBFactory) c.newInstance(); 
			System.out.println(factory.createConnection().getClass().toString());
			System.out.println(factory.createStatement().getClass().toString());
		} catch (ClassNotFoundException e) {
			System.out.println("can not find class!");
		} catch (Exception e) {
			System.out.println(e.getMessage());
		}
	}
}
```

# 后记
抽象工厂模式实现了高内聚低耦合，因此应用广泛，但是还是没能满足开闭原则。看来，在创建模型中工厂模式都不能完美的实现OOP原则。那么之后还有三种创建型模式，他们可以实现OOP原则到哪种程度呢，Zicon接下来再找时间讲解。




