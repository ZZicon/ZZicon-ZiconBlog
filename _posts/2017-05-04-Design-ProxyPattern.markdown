---
layout:     post
title:      "设计模式——代理模式分析"
subtitle:   "通过代理间接访问"
date:       2017-05-04 14:08:00
author:     "Zicon"
header-img: "img/post-bg.jpg"
catalog: true
tags:
    - 设计模式
---

> “写作代理读作中介~”


# 前言

本篇介绍的代理模式是最后一个结构型设计模式了。下一次将给大家介绍行为型模式。

代理模式是常用的结构型设计模式之一，可以保证客户端使用的透明性。当客户不想或不能直接引用一个对象时，可以通过代理实现间接引用，这就是代理模式，
---

# 正文

**代理模式包含以下角色：**

***抽象主题角色（Subject）***

声明真实主题和代理主题的`共同接口`，可以在任何使用真实主题的地方都可以使用代理主题，客户端需要针对抽象主题角色进行编程。

***代理主题角色（Proxy）***

包含对真实主题的`引用`，同时提供一个与真实主题角色相同的`接口`。还可以控制对真实主题对象的使用。

***真实主题角色（RealSubject）***

定义了代理主题角色代表的真实对象，实现了真实的业务操作，客户端可以通过代理主题角色间接调用真实主题角色中定义的方法。

**代理模式的优缺点**

***优点***

 - 协调调用者和被调用者，一定程度降低了系统耦合度。
 - 保护代理可以控制对真实对象的使用权限。
 - 虚拟代理通过使用一个小对象来代表一个大对象，可以减少系统资源消耗，对系统进行优化并提高运行速度。
 - 远程代理使得客户端可以访问远程机器上的对象，远程机器可能具有更好的计算性能与处理速度，可以快速响应并处理客户端请求。
 
***缺点***

 - 实现代理模式需要额外的工作，有些代理模式的实现非常复杂。
 - 由于在客户端和真实主题之间增加了代理对象，因此有些类型的代理模式可能会造成请求的处理速度变慢。
 
**模式适用环境**

 - 防火墙代理：保护目标不让恶意用户接近。
 - 同步化代理：使几个用户能够同时使用一个对象而没有冲突。
 - 远程代理：为一个位于不同地址空间的对象提供一个本地代理对象。
 - 保护代理：控制对对象的访问，可以给不同用户提供不同级别的使用权限
 - 缓冲代理：为某一目标操作的结果提供临时存储空间，以便多个客户端共享这些结果
 - 智能引用代理：当对象被引用时，提供额外的操作，如将此对象被调用的次数记录下来等。
 - 虚拟代理：如果要创建一个资源消耗大的对象，先创建一个消耗较小的对象表示，真实对象只在需要时才被创建
 - Copy-on-Write代理：虚拟代理的一种，把复制操作延迟到客户端真正需要时。一般来说，对象的深克隆是开销较大，Copy-on-Write代理可以让操作延迟，只有对象被用到时才克隆
  
**代理模式的栗子**

某应用软件中需要记录业务方法的调用日志，在不修改现有业务类的基础上为每一个类提供一个日志记录代理类，在代理类中输出日志，如在业务方法method()调用之前输出“方法method()被调用，调用事件为2016-05-01 10：10：10”，调用之后如果没有抛异常则输出“方法method()调用成功”，否则输出“方法method()调用失败”。在代理类中调用真实业务的业务方法，使用代理模式设计该日志记录模块的结构。

![](https://ZZicon.github.io/ZiconBlog/img/int_post/design_mode12.png)

首先是分类：

 - 抽象主题类AbstractLog
 - 真实主题类BusinessClass
 - 代理主题类LoggerProxy

**详细代码**

```
import java.util.*;

//抽象日志记录类：抽象主题
interface AbstractLog {
	public void method();
}

// 业务类：真实主题
class BusinessClass implements AbstractLog {

	public void method() {
		System.out.println("真实业务方法！");
	}

}

// 日志记录代理类：代理主题
class LoggerProxy implements AbstractLog {

	private BusinessClass business = new BusinessClass();

	public void method() {
		Calendar calendar = new GregorianCalendar();
		int year = calendar.get(Calendar.YEAR);
		int month = calendar.get(Calendar.MONTH) + 1;
		int day = calendar.get(Calendar.DAY_OF_MONTH);
		int hour = calendar.get(Calendar.HOUR) + 12;
		int minute = calendar.get(Calendar.MINUTE);
		int second = calendar.get(Calendar.SECOND);
		String dateTime = year + "-" + month + "-" + day + " " + hour + ":"
				+ minute + ":" + second + "！";
		System.out.println("方法method()被调用，调用时间为" + dateTime);
		try {
			business.method();
			System.out.println("方法method()调用成功！");
		} catch (Exception e) {
			System.out.println("方法method()调用失败！");
		}
	}
}

// 客户端测试类
class Client {
	public static void main(String args[]) {
		AbstractLog al;
		al = new LoggerProxy();
		al.method();
	}
}

```

# 后记
代理模式实用性很强，应用范围十分广泛，因此我们应该好好地去学习这个设计模式。