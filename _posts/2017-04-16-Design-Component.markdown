---
layout:     post
title:      "设计模式——组合模式分析"
subtitle:   "结构模式中的树形结构"
date:       2017-04-16 14:08:00
author:     "Zicon"
header-img: "img/post-bg.jpg"
catalog: true
tags:
    - 设计模式
---

> “组合多个对象形成树形结构”


# 前言

在桥接模式之后，对象结构型模式仍未结束，这一篇将给大家介绍组合模式。

组合模式把一组相似对象当作一个单一对象，依据树形结构组合对象，用来表示部分以及整体层次。

---

# 正文

**组合模式包含以下角色：**

***抽象构件（Component）***

可以是接口或抽象类，为叶子构件与容器构件对象声明接口，可以包含`所有的`子类共有行为的声明与实现。同时定义了`访问及管理子构件`的方法。

***叶子构件（Leaf）***

表示叶子节点对象，实现了在抽象构件中定义的行为。

***容器构件（Composite）***

表示容器节点对象，包含子节点，既实现在抽象构件中定义的行为，也实现了访问及管理子构件的方法。

***客户类（Client）***

通过抽象构件接口访问和控制组合构件中的对象。

**组合模式的优缺点**

***优点***

 - 客户端调用简单，可以一致的使用组合结构或单个对象。
 - 清楚定义分层次的复杂对象，表示对象的全部或部分层次，使得增加新构件更容易。
 - 更容易在组合体内加入对象构件，客户端不必因为加入了新的对象构件而更改原有代码。
 - 定义了包含叶子对象和容器对象的类层次结构，叶子对象可以被组合成更复杂的容器对象，而这个容器对象又可以被组合，不断递归，形成复杂的树形结构。
 
***缺点***

 - 使设计更抽象，如果对象业务规则很复杂，则实现组合模式具有挑战性，且不是所有方法都与叶子对象子类有关联。
 
**模式适用环境**

 - 让客户能够忽略不同对象层次的变化，可以针对抽象构件编程，无须关心对象层次结构的细节。
 - 需要表示对象整体或部分层次，在具有整体和部分的层次结构中，希望忽略整体与部分的差异，可以一致对待。
 
**也给组合模式分个栗子**

使用组合模式设计一个杀毒(AntiVirus)软件的框架，该软件既可以对某个文件夹(Folder)杀毒，也可以对某个指定的文件(File)进行杀毒。文件的种类包括图像文件(ImageFile)、视频文件（VideoFile）和文本文件(TextFile)。

![](https://ZZicon.github.io/ZiconBlog/img/int_post/design_mode9.png)

首先是分类：

 - 抽象构件类AbstractFile
 - 叶子构建类ImageFile、VideoFile、TextFile
 - 容器类Folder
 - 客户类Client

**详细代码**

```
import java.util.ArrayList;

//抽象构件类
abstract class AbstractFile {
	public abstract void add(AbstractFile element);

	public abstract void remove(AbstractFile element);

	public abstract void killVirus();
}

// 叶子构建类
class ImageFile extends AbstractFile {

	private String fileName;

	public ImageFile(String fileName) {
		this.fileName = fileName;
	}

	public void add(AbstractFile element) {
		System.out.println("该类无法实现Add操作");
	}

	public void remove(AbstractFile element) {
		System.out.println("该类无法实现Remove操作");
	}

	public void killVirus() {
		System.out.println("对图像文件‘" + fileName + "’进行消毒");
	}
}

class VideoFile extends AbstractFile {

	private String fileName;

	public VideoFile(String fileName) {
		this.fileName = fileName;
	}

	public void add(AbstractFile element) {
		System.out.println("该类无法实现Add操作");
	}

	public void remove(AbstractFile element) {
		System.out.println("该类无法实现Remove操作");
	}

	public void killVirus() {
		System.out.println("对视频文件‘" + fileName + "’进行消毒");
	}
}

class TextFile extends AbstractFile {

	private String fileName;

	public TextFile(String fileName) {
		this.fileName = fileName;
	}

	public void add(AbstractFile element) {
		System.out.println("该类无法实现Add操作");
	}

	public void remove(AbstractFile element) {
		System.out.println("该类无法实现Remove操作");
	}

	public void killVirus() {
		System.out.println("对文本文件‘" + fileName + "’进行消毒");
	}
}

// 容器类
class Folder extends AbstractFile {

	private ArrayList fileList = new ArrayList<>();
	private String fileName;

	public Folder(String fileName) {
		this.fileName = fileName;
	}

	public void add(AbstractFile file) {
		fileList.add(file);
	}

	public void remove(AbstractFile file) {
		fileList.remove(file);
	}

	public void killVirus() {
		System.out.println("对文件夹‘" + fileName + "’进行消毒");
		for(Object object : fileList) {
			((AbstractFile)object ).killVirus();
		}
	}
}

// 客户端测试类
public class Client {
	public static void main(String args[]) {
		AbstractFile file1, file2, file3, file4, folder1, folder2;
		
		folder1 = new Folder("视频文件");
		folder2 = new Folder("其他文件");
		
		file1 = new VideoFile("笑傲江湖.rmvb");
		file2 = new VideoFile("射雕英雄传.avi");
		file3 = new TextFile("九阳神功.txt");
		file4 = new ImageFile("美女.jpg");
		
		folder1.add(file1);
		folder1.add(file2);
		folder2.add(file3);
		folder2.add(file4);
		folder2.remove(file4);
		
		folder1.killVirus();
		folder2.killVirus();		
	}
}
```

**组合模式的扩展**

***透明组合模式***

透明组合模式中，抽象构件声明了所有用于管理成员对象的方法，例如add()、remove()和getChild()方法。

 - 好处：确保所有的构件类都有相同的接口，叶子对象与容器对象提供一致的方法，客户端可以相同地对待所有的对象。
 - 缺点：不够安全，叶子构件包含的管理成员对象没有意义，甚至，在运行阶段如果误调用这些方法会出错。

***安全组合模式***

安全组合模式中，抽象构件没有声明任何用于管理成员对象的方法，而是在容器构件类中声明。

 - 好处：不向叶子对象提供这些管理成员的方法，保证实现过程中的安全。
 - 缺点：不够透明，客户端不能完全针对抽象编程，并一致地使用叶子构件与容器构件。

# 后记
组合模式与数据结构中的树形结构有关联，同时树形结构在软件开发中广泛存在。因此，组合模式也是十分常用的设计模式之一，需要我们去好好的学习。

