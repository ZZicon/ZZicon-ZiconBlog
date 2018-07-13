---
layout:     post
title:      "设计模式——原型模式分析"
subtitle:   "通过克隆复制多个相同的对象"
date:       2017-03-16 17:56:00
author:     "Zicon"
header-img: "img/post-bg.jpg"
catalog: true
tags:
    - 设计模式
---

> “你猜我是本体呢，还是本体呢”


# 前言

需要多次创建同一类型的对象时，可以通过原型模式复制出相同的对象。

其核心是原型类，Prototype类需具备两个条件：

 - 实现Cloneable接口。Java有个Cloneable接口，实现了此接口的类才能被拷贝，否则会抛出异常。
 - 重写Object的clone方法。Object类clone方法的作用域为protected类型，一般类无法调用，因此需将clone方法作用域改为public类型。

---

# 正文

**原型模式包含以下角色：**

***抽象原型类***

定义具有克隆方法的接口。

***具体原型类***

实现具体的`克隆方法`，在克隆方法中返回一个克隆对象。

***客户类***

直接实例化一个对象，通过调用该对象的克隆方法复制得到多个相同的对象。

**原型模式的优缺点**

***优点***

 - 如果创建新的对象比较复杂时，可以利用原型模式简化对象的创建过程，同时也能够提高创建效率。
 - 可以动态增加或减少产品类。
 - 提供了简化的创建结构。
 - 可以使用深克隆的方式保存对象的状态。
 
 
***缺点***

 - 需要为每一个类配备一个克隆方法，但对已有的类进行改造时必须修改其源代码，违背了开闭原则。
 - 在实现深克隆时需要编写较为复杂的代码。

**模式适用环境**

 - 创建对象成本较大时。
 - 系统要保存对象的状态，而对象的状态变化很小。
 - 避免使用分层次的工厂类来创建的分层次的对象。

**恩，再来个栗子**

在某OA系统中，用户可以创建工作周报，由于某些岗位每周工作存在重复性，因此可以通过复制原有工作周报并进行局部修改来快速创建工作周报。

![](https://ZZicon.github.io/ZiconBlog/img/int_post/design_mode5.png)

首先是分类：

 - 抽象原型类Object
 - 具体原型类WeeklyLog

**详细代码**

***Object 类***

```
public class Object {
	protected native Object clone() throws CloneNotSupportedException;
}

```

***WeeklyLog 类***

```
public class WeeklyLog implements Cloneable {

	private String name;
	private String data;
	private String content;

	public WeeklyLog clone() {
		WeeklyLog clone = null;
		try {
			clone = (WeeklyLog) super.clone();
		} catch (CloneNotSupportedException e) {
			System.out.println("Clone failure");
		}
		return clone;

	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	public String getData() {
		return data;
	}

	public void setData(String data) {
		this.data = data;
	}

	public String getContent() {
		return content;
	}

	public void setContent(String content) {
		this.content = content;
	}

}

```

***Client 类***

```
public class Client {
	public static void main(String args[]) {
		WeeklyLog log = new WeeklyLog();
		log.setName("李四");
		log.setData("2017年第四周");
		log.setContent("这周~~~");

		System.out.println("*****周报*****");
		System.out.println(log.getData());
		System.out.println(log.getName());
		System.out.println(log.getContent());
		System.out.println("---------------");

		WeeklyLog copyWeeklyLog = (WeeklyLog) log.clone();
		System.out.println("*****周报*****");
		System.out.println(copyWeeklyLog.getData());
		System.out.println(copyWeeklyLog.getName());
		System.out.println(copyWeeklyLog.getContent());

	}
}
```

原型设计模式中，有一个`小问题`，使用原型模式复制对象不会调用类的构造方法。因为对象复制是通过调用Object类的clone方法完成的，它直接在内存中复制数据，不会调用到类的构造方法。

**原型模式实现深拷贝**

Java的拷贝情况分为深拷贝和浅拷贝，具体如下：

 - 浅拷贝：拷贝对象时仅拷贝对象本身，不拷贝对象包含的引用指向的对象
 - 深拷贝：不仅拷贝对象本身，而且拷贝对象包含的引用指向的所有对象

**举例说明：**

对象A1中包含对B1的引用，B1中包含对C1的引用。浅拷贝A1得到A2，A2依然包含对B1的引用，B1依然包含对C1的引用。深拷贝则是对浅拷贝的递归，深拷贝A1得到A2，A2中包含对B2（B1的copy）的引用，B2中包含对C2（C1的copy）的引用。
 
***深度克隆***
 
```
public abstract class AbstractSpoon implements Serializable {
	String spoonName;

	public void setSpoonName(String spoonName) {
		this.spoonName = spoonName;
	}

	public String getSpoonName() {
		return this.spoonName;
	}

	public Object deepClone() throws IOException, ClassNotFoundException {
		// 将对象写到流里
		ByteArrayOutputStream bos = new ByteArrayOutputStream();
		ObjectOutputStream oos = new ObjectOutputStream(bos);
		oos.writeObject(this);
		// 从流里读回来
		ByteArrayInputStream bis = new ByteArrayInputStream(bos.toByteArray());
		ObjectInputStream ois = new ObjectInputStream(bis);
		return ois.readObject();
	}
  
}
``` 
 

# 后记
原型模式是一种比较简单的模式，也容易理解，实现一个接口，重写一个方法即完成了原型模式。





