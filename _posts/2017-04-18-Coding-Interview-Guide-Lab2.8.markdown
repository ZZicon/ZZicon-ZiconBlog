---
layout:     post
title:      "数据结构最优解之链表Lab2.8"
subtitle:   "链表按值分区"
date:       2017-04-18 17:34:46
author:     "Zicon"
header-img: "img/post-bg.jpg"
catalog: true
tags:
    - 数据结构
---

> “非连续非顺序、动态地进行存储分配的存储结构“


---

# 正文

**题目：将单向链表按某值分成左边小，右边大，中间相等的形式**

要求是：给定一个单向链表头结点head，再给定一个整数pivot。实现调整链表左部分小于pivot，中间部分等于pivot，右部分大于pivot。

**思路分析** 
 
 - 遍历链表将链表打散。
 - 将链表节点分别存入三个代表各个部分的链表。
 - 链表重组。

**代码实现**

***链表节点***

```
class Node{
	public int value;
	public Node next;
	
	public Node(int data) {
		this.value = data;
	}
}
```
  
***过程实现***

```
public class Lab2_8 {
	public Node nodeSort(Node head, int num) {
		if (head == null || head.next == null || num < 1) {
			return head;
		}
		Node lStart = null, lLast = null;
		Node mStart = null, mLast = null;
		Node rStart = null, rLast = null;

		// 分割
		while (head != null) {
			if (head.value < num) {
				if (lStart == null) {
					lStart = head;
					lLast = head;
				} else {
					lLast.next = head;
					lLast = head;
				}
			} else if (head.value == num) {
				if (mStart == null) {
					mStart = head;
					mLast = head;
				} else {
					mLast.next = head;
					mLast = head;
				}
			} else {
				if (rStart == null) {
					rStart = head;
					rLast = head;
				} else {
					rLast.next = head;
					rLast = head;
				}
			}
		}

		// 重连
		if (lStart != null) {
			lLast.next = mStart;
			mLast = mLast == null ? lLast : mLast;
		}
		if (mLast != null) {
			mLast.next = rStart;
		}
		return lStart != null ? lStart : mStart != null ? mStart : rStart;
	}
}
```  
 
# 后记
Zicon将相关的程序代码存放在：[点击这里](https://github.com/ZZicon/Algorithm/tree/master/src/%E7%AC%AC%E4%BA%8C%E7%AB%A0)
