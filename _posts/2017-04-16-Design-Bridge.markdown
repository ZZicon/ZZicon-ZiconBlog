---
layout:     post
title:      "设计模式——桥接模式分析"
subtitle:   "再遇结构模式"
date:       2017-04-16 13:44:00
author:     "Zicon"
header-img: "img/post-bg.jpg"
catalog: true
tags:
    - 设计模式
---

> “抽象与实现分离、独立”


# 前言

继上一篇的适配器模式后，我们一起来认识桥接模式吧。

桥接模式将`抽象部分与实现部分分离`，使它们独立变化。桥接模式将继承关系转换为关联关系，降低耦合减少代码编写量。
 
---

# 正文

**桥接模式包含以下角色：**

***抽象类（Abstraction）***

用于定义抽象类的接口，定义了实现类接口的对象，与其有关联关系，可以包含`抽象的`业务方法，也可以包含`具体的`业务方法。

***扩充抽象类（RefinedAbstraction）***

扩充由抽象类定义的接口，是具体类，实现了在抽象类中定义的抽象业务方法，可以调用实现抽象类的业务方法。

***实现类接口（Implementor）***

定义了实现类的接口，这个接口不一定与抽象类的接口一致。该接口仅提供`基本操作`，而具体实现交给其子类实现。

***具体实现类（ConcreteImplementor）***

实现Implementor类接口并且具体实现它，在不同的具体实现类中提供基本操作的`不同实现`。

**桥接模式的优缺点**

***优点***

 - 分离抽象接口及其实现部分。
 - 桥接模式有时类似多继承，但多继承违背类的单一职责原则（即一个类只有一个变化），复用性较差，且类个数庞大，桥接模式比多继承更好。
 - 桥接模式提高系统的可扩充性，在两个变化维度中任意扩展一个维度，都不需修改原有系统。
 - 实现细节对客户透明，可对用户隐藏实现细节。
 
***缺点***

 - 桥接模式引入会增加系统的理解与设计难度，由于聚合关联关系建立在抽象层，要求开发者针对抽象设计与编程。
 - 桥接模式要求正确识别出系统中两个独立变化的维度，因此使用范围具有一定局限性。
 
**模式适用环境**

 - 一个类存在两个独立变化的维度，且这两个维度都需要扩展。
 - 如果系统需要在构件的抽象化角色和具体化角色之间增加更多灵活性，避免在两个层次之间建立静态的继承联系，通过桥接模式可以使它们在抽象层建立一个关联关系。
 - 抽象化角色和实现化角色可以以继承的方式独立扩展而互不影响，在程序运行时可以动态将一个抽象化子类的对象和一个实现化子类的对象进行组合，即系统需要对抽象化角色和实现化角色进行动态耦合。
 - 虽然系统中使用继承没有问题，但由于抽象化角色和具体化角色需要独立变化，设计要求需要独立管理这两者。
 - 对于那些不希望使用继承或因为多继承导致系统类的个数急剧增加的系统，桥接模式尤为适用。
 
**桥接~栗子**

如果需要开发一个跨平台视频播放器，可以在不同操作系统平台（如Windows、Linux、Unix等）上播放多种格式的视频文件，常见的视频格式包括MPEG、RMVB、AVI、WMV等。现使用桥接模式设计该播放器。

![](https://ZZicon.github.io/ZiconBlog/img/int_post/design_mode8.png)

首先是分类：

 - 抽象类OperatingSystemVersion
 - 扩充抽象类WindowsVersion、UnixVersion、LinuxVersion
 - 实现类接口VideoFile
 - 具体实现类MPEGFile、RMVBFile、AVFile、WMVFile
 - 客户类Client

**详细代码**

```
//具体实现类
class MPEGFile implements VideoFile {
	public void decode(String osType, String fileName) {
		System.out.println("在" + osType + "播放MPEG格式的" + fileName);
	}
}

class RMVBFile implements VideoFile {
	public void decode(String osType, String fileName) {
		System.out.println("在" + osType + "播放RMVB格式的" + fileName);
	}
}

class AVFile implements VideoFile {
	public void decode(String osType, String fileName) {
		System.out.println("在" + osType + "播放AV格式的" + fileName);
	}
}

class WMVFile implements VideoFile {
	public void decode(String osType, String fileName) {
		System.out.println("在" + osType + "播放WMV格式的" + fileName);
	}
}

// 实现类接口
interface VideoFile {
	void decode(String osType, String fileName);
}

// 抽象类
abstract class OperatingSystemVersion {
	protected VideoFile video;

	public void setVideoFile(VideoFile vf) {
		this.video = vf;
	}

	public abstract void play(String fileName);
}

// 扩充抽象类
class WindowsVersion extends OperatingSystemVersion {
	public void play(String fileName) {
		String osType = "Windows平台";
		this.video.decode(osType, fileName);
	}
}

class UnixVersion extends OperatingSystemVersion {
	public void play(String fileName) {
		String osType = "Unix平台";
		this.video.decode(osType, fileName);
	}
}

class LinuxVersion extends OperatingSystemVersion {
	public void play(String fileName) {
		String osType = "Linux平台";
		this.video.decode(osType, fileName);
	}
}

// 客户端测试类
public class Client {
	public static void main(String args[]) {
		VideoFile vf;
		OperatingSystemVersion os;

		vf = new MPEGFile();
		os = new WindowsVersion();
		os.setVideoFile(vf);
		os.play("《软件设计模式》");
	}
}
```

# 后记
桥接模式可以有效地扩展两个独立变化的维度，是改善多继承的很好方法。而实际应用中，适配器模式可以与桥接模式联合使用。

桥接模式用于系统的初步设计，将两个独立变化的维度的类分为抽象化与实现化两个角色；而在初步设计完成后，如果发现系统与已有类无法协同工作时，可以采用适配器模式。



