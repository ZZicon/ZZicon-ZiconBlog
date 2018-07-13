---
layout:     post
title:      "设计模式——外观模式分析"
subtitle:   "交互过程的简化"
date:       2017-05-04 14:08:00
author:     "Zicon"
header-img: "img/post-bg.jpg"
catalog: true
tags:
    - 设计模式
---

> “简化用户与多个对象之间的交互哦”


# 前言

外观模式使用频率非常高，它通过引入一个外观角色，简化了客户端与子系统之间的交互，为复杂的子系统调用提供一个统一的入口，使子系统和客户端的耦合度降低。

---

# 正文

**外观模式包含以下角色：**

***外观角色（Facade）***

外观模式的`核心`，关联所以的子系统，负责将客户端请求发送到相应的子系统。

***子系统角色（SubSystem）***

类或类的集合，处理由外观类传过来的请求，并不知道外观的存在。

**外观模式的优缺点**

***优点***

 - 只是提供了一个访问子系统的统一入口，不影响用户直接使用子系统类。
 - 子系统的修改对其他子系统没有影响，子系统的内部变化也不会影响到外观对象
 - 实现了子系统与客户之间的松耦合关系，子系统的组件变化不会影响到调用它的客户类，只需调整外观类即可。
 - 对客户屏蔽子系统组件，减少了客户处理的对象数目并使子系统更加容易使用。通过引入外观模式，客户端代码变得很简单，与之关联的对象也很少。
 - 降低了大型软件系统中的编译依赖性，并简化了系统在不同平台之间的移植过程，因为编译一个子系统一般不需要编译所有其他的子系统。
 
***缺点***

 - 不能很好地限制客户使用子系统类，如果对客户访问子系统类做太多的限制，则减少了可变性和灵活性。
 - 在不引入抽象外观类的情况下，增加新的子系统可能需要修改外观类或客户端的源代码，违背了开闭原则。
 
**模式适用环境**

 - 当要为一个复杂子系统提供一个简单接口时。该接口可以满足大多数用户的需求，而且用户也可以越过外观类直接访问子系统。
 - 客户程序与多个子系统之间存在很大依赖性。引入外观类将子系统与客户及其他子系统解耦，提高子系统的独立性和可移植性。
 - 在层次化结构中，可以使用外观模式定义系统中每层的入口，层与层之间不直接产生联系，而通过外观类建立联系，降低层之间的耦合度。
 
 
**外观模式的栗子**

某系统需要提供一个文件加密模块，加密流程包括三个操作，分别是读取源文件、加密、保存加密之后的文件。读取文件和保存文件使用流来实现，这三个操作相对独立，其业务代码封装在三个不同的类中。现在需要提供一个统一的加密外观类，用户可以直接使用该加密外观类完成文件的读取、加密和保存三个操作，而不需要与每一个类进行交互，使用外观模式设计该加密模块。
![](https://ZZicon.github.io/ZiconBlog/img/int_post/design_mode11.png)

首先是分类：

 - 外观类EncryptFacade
 - 子系统类FileReader、CipherMachine、FileWriter
 - 客户类Client

**详细代码**

```
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.FileNotFoundException;
import java.io.IOException;

//加密外观类：外观类
class EncryptFacade {
	private FileReader reader;
	private CipherMachine cipher;
	private FileWriter writer;

	public EncryptFacade() {
		reader = new FileReader();
		cipher = new CipherMachine();
		writer = new FileWriter();
	}

	public void fileEncrypt(String fileNameSrc, String fileNameDes) {
		String plainStr = reader.read(fileNameSrc);
		String encryptStr = cipher.encrypt(plainStr);
		writer.write(encryptStr, fileNameDes);
	}

}

// 文件读取类：子系统类
class FileReader {
	public String read(String fileNameSrc) {
		System.out.println("读取文件，获取明文。");
		StringBuffer sb = new StringBuffer();
		try {
			FileInputStream inFS = new FileInputStream(fileNameSrc);
			int data;
			while ((data = inFS.read()) != -1) {
				sb = sb.append((char) data);
			}
			inFS.close();
		} catch (FileNotFoundException e) {
			System.out.println("文件不存在！");
		} catch (IOException e) {
			System.out.println("文件操作错误！");
		}
		return sb.toString();
	}
}

// 数据加密类：子系统类
class CipherMachine {
	public String encrypt(String plainText) {
		System.out.println("数据加密，将明文转换为密文。");
		String es = "";
		for (int i = 0; i < plainText.length(); i++) {
			String c = String.valueOf(plainText.charAt(i) % 7);
			es += c;
		}
		return es;
	}
}

// 文件保存类：子系统类
class FileWriter {
	public void write(String encryptStr, String fileNameDes) {
		System.out.println("保存密文，写入文件。");
		try {
			FileOutputStream outFS = new FileOutputStream(fileNameDes);
			outFS.write(encryptStr.getBytes());
			outFS.close();
		} catch (FileNotFoundException e) {
			System.out.println("文件不存在！");
		} catch (IOException e) {
			System.out.println("文件操作错误！");
		}
	}
}

// 客户端测试类
class Client {
	public static void main(String args[]) {
		EncryptFacade ef = new EncryptFacade();
		ef.fileEncrypt("src.txt", "des.txt");
	}
}
```

结果如图：

![](https://ZZicon.github.io/ZiconBlog/img/int_post/design_mode11.2.png)

**外观模式拓展**

***一个系统可以有多个外观类***

外观模式通常只需要一个外观类同时也只有一个实例，所以一般将其设计为单例类。但是，在一个系统中可以设计多个外观类，负责与一些特定的子系统进行交互。

***不能通过外观类为子系统增加新行为***

外观模式是用来为子系统提供简化的，并不是为了向子系统中加入新的行为，新行为只能通过修改子系统类或增加子系统类实现。

***迪米特法则***

外观类充当了客户类与子系统类的第三方，降低其耦合度，实现了迪米特法则（要求每一个对象与其他对象的相互作用的短程而非长程的）。

# 后记
外观模式使用频率高，而实现和理解较为简单，因此几乎在所有的软件中都有外观模式的应用，例如菜单、工具栏、导航页面等。所以，我们应该认真去学习好这个模式。
