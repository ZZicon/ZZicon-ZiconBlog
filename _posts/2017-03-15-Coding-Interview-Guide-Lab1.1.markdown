---
layout:     post
title:      "数据结构最优解之栈与队列Lab1"
subtitle:   "一个有getMin功能的栈"
date:       2017-03-15 17:56:00
author:     "Zicon"
header-img: "img/post-bg.jpg"
catalog: true
tags:
    - 数据结构
---

> “程序设计中被广泛使用的两种线性结构“


# 前言

首先，简单地介绍一下栈与队列。

从"数据结构"的角度看，它们都是线性结构，即数据元素之间的关系相同。但它们是完全不同的数据类型。除了它们各自的基本操作集不同外，主要区别是对插入和删除操作的"限定"。

 - 栈（Stack）是限定只能在表的一端进行插入和删除操作的线性表。
 - 队列（Queue）是限定只能在表的一端进行插入和在另一端进行删除操作的线性表。

栈必须按"后进先出"的规则进行操作，而队列必须按"先进先出"的规则进行操作。

然后，这个系列都是Zicon在学习左程云的程序员代码面试指南时进行的整理。

---

# 正文

**题目：设计一个有getMin功能的栈**

实现一个特殊的栈，在实现栈的基本功能的基础上，再实现返回栈中最小元素的操作。

**要求**

 - pop、push、getMin操作的时间复杂度都是O(1) 
 - 设计的栈类型可以输用现成的栈结构

**思路分析**

 - 使用两个栈实现，一个栈stackData用来保存``当前栈``的元素。
 - 另一个栈stackMin保存``每一步``的最小值。

***栈的压入规则***

 - 当前数据为newNum，将其压入stackData，判断stackMin是否为空。
 - 为空，newNum压入stackMin。
 - 不为空，比较newNum与stackMin的栈顶元素哪一个更小。
 - newNum更小或相等，newNum压入stackMin。
 - stackMin栈顶元素小，stackMin不压入任何该数据。

***栈的弹出规则***

 - stackData弹出栈顶元素，记为value。比较value与stackMin栈顶元素的大小。
 - value等于stackMin栈顶元素时，stackMin弹出栈顶元素；
 - value大于stackMin栈顶元素时，stackMin不弹出栈顶元素。
 - 返回value。

**代码实现**

```
	private Stack <Integer> stackData;
	private Stack <Integer> stackMin;

	public Lab1_1() {
		this.stackData = new Stack<Integer>();
		this.stackMin = new Stack<Integer>();
	}

	public void push(int newNum) {
		if (this.stackMin.isEmpty()) {
			this.stackMin.push(newNum);
		} else if (newNum <= this.getMin()) {
			this.stackMin.push(newNum);
		}
		this.stackData.push(newNum);
	}

	public int pop() {
		if (this.stackData.isEmpty()) {
			throw new RuntimeException("Your stack is empry");
		}
		int value = this.stackData.pop();
		if (value == this.getMin()) {
			this.stackMin.pop();
		}
		return value;
	}

	public int getMin() {
		if (this.stackData.isEmpty()) {
			throw new RuntimeException("Your stack is empry");
		}
		return this.stackMin.peek();
	}
```
  
 
# 后记
该方法是以stackMin栈保存stackData每一步的最小值，其所有操作的时间复杂度均为O(1)。

核心操作是比较每个stackData栈的弹出元素与stackMin栈的栈顶元素，确保stackMin中存有最小项。

相对来说，压入操作省下了空间，代价是弹出操作用时稍长。

Zicon将相关的程序代码存放在：[点击这里](https://github.com/ZZicon/Algorithm/tree/master/src/%E7%AC%AC%E4%B8%80%E7%AB%A0)
