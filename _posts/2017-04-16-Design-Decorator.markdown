---
layout:     post
title:      "设计模式——装饰模式分析"
subtitle:   "对对象进行装饰"
date:       2017-04-16 14:08:00
author:     "Zicon"
header-img: "img/post-bg.jpg"
catalog: true
tags:
    - 设计模式
---

> “获得更加符合用户需求的对象”


# 前言

这一篇将给大家介绍同样是对象结构型模式的装饰模式。装饰模式通过装饰类动态的给对象添加额外的职责。

---

# 正文

**装饰模式包含以下角色：**

***抽象构件（Component）***

定义了对象接口，可以给对象`动态增加方法`。声明了在具体构件中实现的业务方法，可以使客户端以一致的方式处理未被装饰以及装饰之后的对象，实现客户端的`透明`操作。

***具体构件（ConcreteComponent）***

定义了具体的构件对象，不仅实现了在抽象构件中声明的方法，`装饰器`可以给它增加`额外`的方法。

***抽象装饰类（Decorator）***

抽象构件类的子类，给具体构件增加职责方法。维护一个`指向抽象构件对象的引用`，通过引用可以调用装饰前构件对象的方法，并通过子类扩展方法，达到装饰的目的。

***具体装饰类（ConcreteDecorator）***

抽象装饰类的子类，给构件添加附加职责。每一个具体装饰类都定义了新行为

**装饰模式的优缺点**

***优点***

 - 装饰模式与继承关系的目的都是扩展对象功能，但装饰模式可提供比继承更多的灵活性。
 - 通过使用不同具体装饰类以及这些装饰类的排列组合，可以创造出很多不同行为的组合。
 - 可以通过一种动态方式来扩展对象功能，通过配置文件可在运行时选择不同装饰器，从而实现不同行为。
 - 具体构件类与具体装饰类可独立变化，用户可根据需要增加新的具体构件类和具体装饰类，使用时再对其进行组合，原有代码无须改变，符合开闭原则。
 
***缺点***

 - 这种比继承更加灵活的特性，同时意味着更易出错，排错困难，对于多次装饰的对象，寻找错误要逐级排查。
 - 使用装饰模式进行系统设计时将产生很多小对象，这些对象的区别在于它们之间相互连接的方式不同，而不是它们的类或者属性值不同，同时还将产生很多具体装饰类。这些装饰类和小对象的产生将增加系统复杂度。
 
**模式适用环境**

 - 需要动态地给对象增加功能，这些功能也可以动态撤销。
 - 在不影响其他对象的情况下，以动态透明的方式给单个对象添加职责。
 - 当不能采用继承扩充系统或继承不利于系统扩展维护时。
 - 而不能采用继承的情况有两类：一是系统存在大量独立扩展，为支持每一种组合将产生大量子类，使得子类数目爆炸性增长；二是因为类定义不能继承（如final类）。
 
 
**装饰模式需要包装栗子**

某系统提供了一个数据加密功能，可以对字符串进行加密。最简单的加密算法通过对字母进行移位来实现，同时还提供了稍复杂的逆向输出加密，还提供了更为高级的求模加密。用户先使用最简单的加密算法对字符串进行加密，如果觉得还不够可以对加密之后的结果使用其他加密算法进行二次加密，当然也可以进行第三次加密。现使用装饰模式设计该多重加密系统。

![](https://ZZicon.github.io/ZiconBlog/img/int_post/design_mode10.png)

首先是分类：

 - 抽象构件类Cipher
 - 具体构件类SimpleCipher
 - 抽象装饰类CipherDecorator
 - 具体装饰类ComplexCipher、AdvancedCipher
 - 客户类Client

**详细代码**

```
//抽象加密
interface Cipher {
	public String encrypt(String plainText);
}

// 简单加密
class SimpleCipher implements Cipher {

	public String encrypt(String plainText) {
		String str = "";
		for (int i = 0; i < plainText.length(); i++) {
			char c = plainText.charAt(i);
			if (c >= 'a' && c <= 'z') {
				c += 6;
				if (c > 'z') {
					c -= 26;
				}
				if (c < 'a') {
					c += 26;
				}
			}
			if (c >= 'A' && c <= 'Z') {
				c += 6;
				if (c > 'Z') {
					c -= 26;
				}
				if (c < 'A') {
					c += 26;
				}
			}
			str += c;
		}
		return str;
	}
}

// 加密装饰
class CipherDecorator implements Cipher{
	private Cipher cipher;

	public CipherDecorator(Cipher cipher) {
		this.cipher = cipher;
	}

	public String encrypt(String plainText) {
		return cipher.encrypt(plainText);
	}
}

// 复杂加密类
class ComplexCipher extends CipherDecorator {
	public ComplexCipher(Cipher cipher) {
		super(cipher);
	}

	public String encrypt(String plainText) {
		String result = super.encrypt(plainText);
		result = reverse(result);
		return result;
	}

	public String reverse(String text) {
		String str = "";
		for (int i = text.length(); i > 0; i--) {
			str += text.substring(i - 1, i);
		}
		return str;
	}
}

// 复杂加密装饰
class AdvancedCipher extends CipherDecorator {
	public AdvancedCipher(Cipher cipher) {
		super(cipher);
	}

	public String encrypt(String plainText) {
		String result = super.encrypt(plainText);
		result = mod(result);
		return result;
	}

	public String mod(String text) {
		String str = "";
		for (int i = 0; i < text.length(); i++) {
			String c = String.valueOf(text.charAt(i) % 6);
			str += c;
		}
		return str;
	}
}

// 客户端测试类
public class Client {
	public static void main(String args[]) {
		String password = "sunnyLiu";
		String cpassword;
		Cipher sc,cc;
		
		sc = new SimpleCipher();
		cpassword = sc.encrypt(password);
		System.out.println(cpassword);
		System.out.println("--------------------");
		
		cc = new ComplexCipher(sc);
		cpassword = cc.encrypt(password);
		System.out.println(cpassword);
		System.out.println("--------------------");
	}
}
```


# 后记
装饰模式是一种用于替代继承的技术，可以降低系统耦合度，使得需要装饰的具体构件类和具体装饰类独立变化，很容易增加新的类，满足“开闭原则”。

但实际上装饰模式和继承模式都有着各自的优劣，如何根据具体情况，选择合适的解决方法，也是我们需要掌握的。
