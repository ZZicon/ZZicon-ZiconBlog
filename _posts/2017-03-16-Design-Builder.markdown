---
layout:     post
title:      "设计模式——建造者模式分析"
subtitle:   "GoF最复杂的创建型模式"
date:       2017-03-16 14:46:00
author:     "Zicon"
header-img: "img/post-bg.jpg"
catalog: true
tags:
    - 设计模式
---

> “将复杂对象的构建与表示分离”


# 前言

建造者模式针对复杂对象，将复杂对象构建与它的表示分离，使得同样的构建过程可以创建不同的表示。

---

# 正文

**建造者模式包含以下角色：**

***抽象建造者***

抽象建造者为对象的各个`属性`指定抽象接口，并声明属性的build()与get()方法。

***具体建造者***

实现了抽象建造者的接口，实现属性的具体build()与get()方法，`定义并明确`其创建的复杂对象。

***产品***

被构建的复杂对象，包含多个属性，由具体建造者创建其`内部表示`并定义其`装配过程`。

***指挥者***

安排复杂对象的建造次序，负责调用适当建造者组建产品。


**建造者模式的优缺点**

***优点***

 - 客户端不必知道产品内部组成细节，将产品本身与产品创建过程解耦，使得相同创建过程可以创建不同产品。
 - 具体建造者相对独立，可以很方便地替换具体建造者或增加新的具体建造者。
 - 将复杂产品的创建步骤分解在不同方法中，使得创建过程更加清晰，也方便使用程序来控制创建过程。
 - 增加新的具体建造者无须修改原有类库代码，指挥者类针对抽象建造者类编程，系统扩展方便，符合开闭原则。
 
 
***缺点***

 - 创建产品一般具有较多共同点，组成部分相似，如果产品差异性大，则不适合用建造者模式，因此使用范围有限。
 - 如果产品内部变化复杂，可能会导致需要定义很多具体建造者类来实现这种变化，导致系统庞大。
 
**建造者模式与OOP设计原则**

***建造者模式已遵循原则***

 - 依赖倒置原则
 - 迪米特法则
 - 里氏替换原则
 - 接口隔离原则
 - 单一职责原则
 - 开闭原则
 
**模式适用环境**

 - 需要生成的产品对象有复杂的内部结构，包含多个成员属性。
 - 需要生成的产品对象的属性相互依赖，需要指定生成顺序。
 - 对象创建过程独立于创建对象的类。建造者模式引入了指挥者类，将创建过程封装在指挥者类中，而不在建造者类中。
 - 隔离复杂对象的创建和使用，并使得相同创建过程可以创建不同产品。

**恩，再来个栗子**


某游戏软件中人物角色包括多种类型，不同类型的人物角色，其性别、脸型、服装、发型等外部特征有所差异，使用建造者模式创建人物角色对象

![](https://ZZicon.github.io/ZiconBlog/img/int_post/design_mode4.png)

首先是分类：

 - 产品类Actor
 - 抽象建造者类ActorBuilder
 - 具体建造者类HeroBuilder、AngelBuilder、GhostBuilder
 - 指挥者类ActorController

**详细代码**

***Actor 类***

```
public class Actor {
	private String type;
	private String sex;
	private String face;
	private String costume;
	private String hairstyle;

	public void setType(String type) {
		this.type = type;
	}

	public void setSex(String sex) {
		this.sex = sex;
	}

	public void setFace(String face) {
		this.face = face;
	}

	public void setCostume(String costume) {
		this.costume = costume;
	}

	public void setHairstyle(String hairstyle) {
		this.hairstyle = hairstyle;
	}

	public String getType() {
		return type;
	}

	public String getSex() {
		return sex;
	}

	public String getFace() {
		return face;
	}

	public String getCostume() {
		return costume;
	}

	public String getHairstyle() {
		return hairstyle;
	}
}
```

***ActorBuilder 类***

```
public abstract class ActorBuilder {
	protected Actor actor = new Actor();

	public Actor createActor() {
		return actor;
	}

	// 引入钩子方法
	public boolean isBareheaded() {
		return false;
	}

	public abstract void buildType();

	public abstract void buildSex();

	public abstract void buildFace();

	public abstract void buildCostume();

	public abstract void buildHairstyle();
}
```

***AngelBuilder 类***

```
public class AngelBuilder extends ActorBuilder {
	public void buildType() {
		actor.setType("天使");
	}

	public void buildSex() {
		actor.setSex("女");
	}

	public void buildFace() {
		actor.setFace("漂亮");
	}

	public void buildCostume() {
		actor.setCostume("白色长裙");
	}

	public void buildHairstyle() {
		actor.setHairstyle("披肩长发");
	}
}
```

***GhostBuilder 类***

```
public class GhostBuilder extends ActorBuilder {
	public void buildType() {
		actor.setType("魔鬼");
	}

	public void buildSex() {
		actor.setSex("妖");
	}

	public void buildFace() {
		actor.setFace("丑陋");
	}

	public void buildCostume() {
		actor.setCostume("黑衣");
	}

	public void buildHairstyle() {
		actor.setHairstyle("光头");
	}

	// 覆盖钩子方法
	public boolean isBareheaded() {
		return true;
	}
}
```

***HeroBuilder 类***

```
public class HeroBuilder extends ActorBuilder {
	public void buildType() {
		actor.setType("英雄");
	}

	public void buildSex() {
		actor.setSex("男");
	}

	public void buildFace() {
		actor.setFace("英俊");
	}

	public void buildCostume() {
		actor.setCostume("盔甲");
	}

	public void buildHairstyle() {
		actor.setHairstyle("飘逸");
	}
}
```

***ActorController 类***

```
public class ActorController {
	public Actor construct(ActorBuilder ab) {
		Actor actor;
		ab.buildType();
		ab.buildSex();
		ab.buildFace();
		ab.buildCostume();
		// 通过钩子方法来控制产品的构建
		if (!ab.isBareheaded())
			ab.buildHairstyle();
		actor = ab.createActor();
		return actor;
	}
}
```

***Client类***

```
public class Client {
	public static void main(String args[]) {
		ActorController ac = new ActorController();
		//创建角色对象
		ActorBuilder ab = new GhostBuilder();
		Actor ghost = ac.construct(ab);
		String type = ghost.getType();
		System.out.println(type + "的外观：");
		System.out.println("性别：" + ghost.getSex());
		System.out.println("面容：" + ghost.getFace());
		System.out.println("服装：" + ghost.getCostume());
		System.out.println("发型：" + ghost.getHairstyle());
	}
}
```

建造者设计模式中，有一个`小问题`。

那就是当某具体建造者对应的产品属性为空时应该怎么办呢？

仔细看的读者应该就发现了在ActorBuilder类中，我们引入了一个钩子方法。钩子方法，可以设置产品属性是否为空，解决了产品属性为空的问题。

# 后记
当系统中只需要一个具体建造者的情况下，该模式可以省略掉抽象建造者。

而在具体建造者只有一个的情况下，还可以省略掉指挥者角色。



