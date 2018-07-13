---
layout:     post
title:      "设计模式——适配器模式分析"
subtitle:   "与结构模式的初见"
date:       2017-04-16 13:24:00
author:     "Zicon"
header-img: "img/post-bg.jpg"
catalog: true
tags:
    - 设计模式
---

> “将一个类的接口转换成客户希望的另外一个接口。Adapter 模式使得原本由于接口不兼容而不能一起工作的那些类可以一起工作。”


# 前言

学习完创建型模式之后，我们一起来接触一下结构型模式。首先给大家讲述适配器模式。
 
---

# 正文

**适配器模式包含以下角色：**

***目标抽象类（Target）***

定义客户要用的`特定领域的接口`，可以是抽象类或者接口，也可以是具体类。但是，在`类适配器中`，由于Java不支持多继承而`只能是接口`。

***适配器类（Adapter）***

作为一个转换器，可以调用其他接口，对Adaptee和Target进行适配。是适配器模式的`核心`。

 - 在类适配器中，它实现Target接口而继承Adaptee。
 - 在对象适配器中，它继承Target并关联Adaptee。

***适配者类（Adaptee）***

定义了一个接口，这个接口需要被适配，适配者类一般是一个具体类，包含了客户希望使用的业务方法。

***客户类（Client）***

针对目标抽象类进行编程，调用目标抽象类中定义的业务方法。

**类适配器与对象适配器**

类适配器模式中，适配器类与适配者类是继承关系。但Java遵循单继承，所以类适配器往往受到限制，在面向对象设计原则中，有条叫做组合/聚合复用原则，指尽可能使用组合和聚合达到复用目的而不是继承，所以一般推荐用对象适配器。

**适配器模式的优缺点**

***优点***

 - 将目标类和适配者类解耦，通过引入一个适配器类来重用现有的适配者类，而无须修改原有代码。
 - 增加了类的透明性和复用性，将具体实现封装在适配者类中，对于客户端类来说是透明的，且提高了适配者的复用性。
 - 灵活性和扩展性好，通过配置文件可以更换适配器，也可以在不修改代码的基础上增加新适配器类，符合开闭原则。
 - 类适配器模式优点：由于适配器类是适配者类子类，故可在适配器类中置换适配者方法，使得适配器灵活性更强。
 - 对象适配器模式优点：一个对象适配器可把多个不同适配者适配到同一目标，即可把适配者类及其子类都适配。
  
***缺点***

 - 类适配器模式缺点：对于不支持多继承的语言，一次只能适配一个适配者类，而且目标抽象类只能为抽象类，不能为具体类，使用有一定局限性，不能将一个适配者类和它的子类都适配到目标接口。
 - 对象适配器模式缺点：与类适配器模式相比，要想置换适配者类的方法不容易。如果一定要置换，只好先做一个适配者类的子类，将适配者类方法置换掉，再把适配者类子类当做真正适配者适配。
 
**模式适用环境**

 - 系统需要使用现有类，而这些类的接口不符合系统需要。
 - 想建立一个可重复使用的类，用于与一些彼此之间没太大关联的一些类，包括一些可能将来引进的类一起工作。
 
**适配器的栗子**

现有一个接口DataOperation,已知类QuickSort有quickSort(int[])方法，类BinarySearch有binarySearch(int[],int)方法。现使用适配器模式设计一个系统，在不修改源代码的情况下将类QuickSort和类BinarySearch的方法适配到DataOperation接口中。

![](https://ZZicon.github.io/ZiconBlog/img/int_post/design_mode7.png)

首先是分类：

 - 目标接口类DataOperation
 - 适配者类QuickSort、BinarySearch
 - 适配器类OperationAdapter
 - 客户类Client

**详细代码**

```
//抽象数据操作类：目标接口
interface DataOperation {
	public int[] sort(int array[]);

	public int search(int array[], int key);
}

// 快速排序类：适配者
class QuickSort {
	public int[] quickSort(int array[]) {
		sort(array, 0, array.length - 1);
		return array;
	}

	public void sort(int array[], int p, int r) {
		int q = 0;
		if (p < r) {
			q = partition(array, p, r);
			sort(array, p, q - 1);
			sort(array, q + 1, r);
		}
	}

	public int partition(int[] a, int p, int r) {
		int x = a[r];
		int j = p - 1;
		for (int i = p; i <= r - 1; i++) {
			if (a[i] <= x) {
				j++;
				swap(a, j, i);
			}
		}
		swap(a, j + 1, r);
		return j + 1;
	}

	public void swap(int[] a, int i, int j) {
		int t = a[i];
		a[i] = a[j];
		a[j] = t;
	}
}

// 二分查找类：适配者
class BinarySearch {
	public int binarySearch(int array[], int key) {
		int low = 0;
		int high = array.length - 1;
		while (low <= high) {
			int mid = (low + high) / 2;
			int midVal = array[mid];

			if (midVal < key) {
				low = mid + 1;
			} else if (midVal > key) {
				high = mid - 1;
			} else {
				return 1; // 找到元素返回1
			}
		}
		return -1; // 未找到元素返回-1
	}
}

// 操作适配器：适配器
class OperationAdapter implements DataOperation {

	private BinarySearch searchObj;
	private QuickSort sortObj;

	public OperationAdapter(QuickSort sortObj, BinarySearch searchObj) {
		this.sortObj = sortObj;
		this.searchObj = searchObj;
	}

	@Override
	public int[] sort(int[] array) {
		return sortObj.quickSort(array);
	}

	@Override
	public int search(int[] array, int key) {
		return searchObj.binarySearch(array, key);
	}

}

// 客户端测试类
public class Client {
	public static void main(String args[]) {
		DataOperation operation;
		QuickSort sortObj = new QuickSort();
		BinarySearch searchObj = new BinarySearch();
		operation = new OperationAdapter(sortObj, searchObj);
		int array[] = { 13, 24, 15, 36, 26, 17, 68, 34 };
		int result[];
		int value;

		System.out.println("result : ");
		result = operation.sort(array);
		for (int i : array) {
			System.out.print(i + ",");
		}

		System.out.print("\nfind 24:");
		System.out.println((operation.search(array, 24)) == 1 ? "YES"
				: "NO");
		System.out.print("find 25:");
		System.out.println((operation.search(array, 25)) == 1 ? "YES"
				: "NO");

	}
}
```

# 后记
适配器模式运用广泛，使得原本由于接口不兼容而不能一起工作的那些类可以在一起工作。




