---
layout:     post
title:      "数据结构最优解之栈与队列Lab3"
subtitle:   "逆序一个栈"
date:       2017-03-22 15:40:00
author:     "Zicon"
header-img: "img/post-bg.jpg"
catalog: true
tags:
    - 数据结构
---

> “程序设计中被广泛使用的两种线性结构“


# 前言

今天记录第三、四题。

---

# 正文

**题目：仅用递归函数和栈操作逆序一个栈**

题目要求使用递归函数，因此不考虑用两个栈的压入弹出实现。

分析题目，可以得出题目的关键一个是得到栈底元素，另一个是对栈底元素的逆序重新压入栈。

这些都可以通过递归函数实现。

**代码实现**

```
public class Lab1_3 {
	// 递归获取并移除栈底元素
	public static int getAndRemoveLastElement(Stack <Integer> stack) {
		int result = stack.pop();
		if (stack.isEmpty()) {
			return result;
		} else {
			int last = getAndRemoveLastElement(stack);
			stack.push(result);
			return last;
		}
	}

	// 逆序的递归操作
	public static void reverse(Stack <Integer> stack) {
		if (stack.isEmpty()) {
			return;
		} else {
			int last = getAndRemoveLastElement(stack);
			reverse(stack);
			stack.push(last);
		}
	}
}
```
 
# 后记
Zicon将相关的程序代码存放在：[点击这里](https://github.com/ZZicon/Algorithm/tree/master/src/%E7%AC%AC%E4%B8%80%E7%AB%A0)
