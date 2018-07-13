---
layout:     post
title:      "数据结构最优解之栈与队列Lab2"
subtitle:   "两个栈构成的队列"
date:       2017-03-20 17:56:00
author:     "Zicon"
header-img: "img/post-bg.jpg"
catalog: true
tags:
    - 数据结构
---

> “程序设计中被广泛使用的两种线性结构“


# 前言

书接上回，跟着给大家介绍第二题。

---

# 正文

**题目：两个栈组成队列**

编写一个类，用两个栈实现队列，支持队列的基本操作（add、poll、peek）。

这个题目初看之下很简单，稍微接触过数据结构的读者都知道对于栈，它是先进后出，而对于队列，它是先进先出。
所以用两个栈实现一个队列最直观的思路就是：`设这两个栈分别为s1，s2，始终把s1作为存储空间，把s2作为临时空间。`

但实际上，有很多细微的地方可以进行优化让代码更简洁。

**入队分析**

 - 入队时，把元素压入s1
 
**出队分析**

出队有很多思路：
 - 第一种思路：先把s1中除最底下所有元素都倒入s2，然后弹出s1的唯一元素，再把s2中所有元素倒回s1
 - 第二种思路：判断s2是否为空，如不为空，则直接弹出s2的栈顶元素，如为空，则把s1的除最底部元素外的所有元素逐个倒入s2，然后弹出s1的唯一元素

大家可以明确的看出来，第二种方法相对第一种方法更细致，不仅做出了对栈为空的判断，更重要的是避免了反复“倒”栈，仅在需要时才“倒”一次。

但是，你们知道是什么原因导致了两种思路的不同吗。可以是`编程经验`、`对数据结构的熟悉程度`等等。

但归根到底是思路的不同。如果你一直把s2只看作是一个临时缓冲的空间，那么你就不可能想到第二种思路。因为在第二种思路里，s2已经不仅仅作为缓冲空间了，而是看作一个与s1对等的存储空间。

**关键点**

那么，根据思路二我们可以做出如下分析： 
 - 如果s1要往s2中压入数据，那么必须一次把s1中数据全部压入。
 - 如果s2不为空，s1绝对不能往s2中压入数据。
 - 而只要不违反上面规定，压入数据操作在add、pull、peek三种方法中任何一种时候进行都是可以的。（在add方法进行代码量相对较少）

**代码实现**

```
public class Lab1_2 {
	public Stack <Integer> stackPop;
	public Stack <Integer> stackPush;

	public Lab1_2() {
		stackPop = new Stack <Integer>();
		stackPush = new Stack <Integer>();
	}

	public void add(int newNum) {
		stackPush.push(newNum);
		// 必须在push后进行压入stackPop，避免第一次压入后stackPop为空
		if (stackPop.isEmpty()) {
			while (!stackPush.isEmpty()) {
				stackPop.push(stackPush.pop());
			}
		}
	}

	public int poll() {
		if (stackPop.isEmpty() && stackPush.isEmpty()) {
			throw new RuntimeException("Your queue is empty");
		}
		return stackPush.pop();
	}

	public int peek() {
		if (stackPop.isEmpty() && stackPush.isEmpty()) {
			throw new RuntimeException("Your queue is empty");
		}
		return stackPush.peek();
	}

}
```
  
 
# 后记
Zicon将相关的程序代码存放在：[点击这里](https://github.com/ZZicon/Algorithm/tree/master/src/%E7%AC%AC%E4%B8%80%E7%AB%A0)
