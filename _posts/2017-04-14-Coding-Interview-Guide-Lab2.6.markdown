---
layout:     post
title:      "数据结构最优解之链表Lab2.6"
subtitle:   "链表处理约瑟夫问题"
date:       2017-04-14 14:56:46
author:     "Zicon"
header-img: "img/post-bg.jpg"
catalog: true
tags:
    - 数据结构
---

> “非连续非顺序、动态地进行存储分配的存储结构“

# 前言

约瑟夫问题：

据说著名犹太历史学家 Josephus有过以下的故事：在罗马人占领乔塔帕特后，39 个犹太人与Josephus及他的朋友躲到一个洞中，39个犹太人决定宁愿死也不要被敌人抓到，于是决定了一个自杀方式，41个人排成一个圆圈，由第1个人开始报数，每报数到第3人该人就必须自杀，然后再由下一个重新报数，直到所有人都自杀身亡为止。

---

# 正文

**题目：环形单链表的约瑟夫问题**

要求是：一个环形单链表头结点head，和报数的值m。假设链表长度为N，要求时间复杂为O(N)。

**思路分析**
 
 - 链表为空或节点数为1，或m小于1，不用调整，直接返回。
 - 不断遍历节点进行报数
 - 报数到达m时删除当前报数节点，重新把剩下节点链成环。
 - 重复直到剩下一个节点。
 
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
// 普通解法
	public Node josephusNormal(Node head, int m) {
		if (head == null || head.next == head || m < 1) {
			return head;
		}
		Node last = head;
		int kill = 0;
		// 得到头节点的前一个节点
		while (last.next != head) {
			last = last.next;
		}
		while (last != head) {
			if (++kill == m) {
				last.next = head.next;
				kill = 0;
			} else {
				last = last.next;
			}
			head = last.next;
		}
		return head;
	}
```  

上述解法实现不难，但是每删除一个节点都需要遍历m次，一共需要删除n-1个节点，所以其时间复杂度为O(n * m)。

一般解法之所以耗时长，是因为我们一开始并不知道到底哪一个节点最后会留下来。但如果我们可以从开始就确定最后可以留下来的节点呢？

**进阶思路分析** 
 
 - 已知Num(i)=1，只要确定Num(i)和Num(i-1)之间的关系就可以通过递归求出Num(n)。
 - 假设在圈内报A的是节点B，B=(A-1)%i+1。
 - 假设环大小为i的节点s编号为old，环大小为i-1的对应节点编号为new，则old = (new + s - 1) % i + 1。
 - 又每次都是报数m的节点被删除，当A=m，可得被删除的节点是(m-1)%i+1，即s=(m-1)%i+1。
 - 将s代入old，得old=(new+m-1)%i+1。得到Num(i-1)和Num(i)的关系。
 
***过程实现***

```
	// 进阶解法
	public Node josephusUp(Node head, int m) {
		if (head == null || head.next == head || m < 1) {
			return head;
		}
		Node cur = head.next;
		int key = 1;
		// 获取链表长度
		while (cur != head) {
			key++;
			cur = cur.next;
		}
		key = getLive(key, m);
		while (--key != 0) {
			head = head.next;
		}
		head.next = head;
		return head;
	}

	// 递归求最后存活的号数
	public int getLive(int i, int m) {
		if (i == 1) {
			return 1;
		}
		return (getLive(i - 1, m) + m - 1) % i + 1;
	}
``` 
 
# 后记
Zicon将相关的程序代码存放在：[点击这里](https://github.com/ZZicon/Algorithm/tree/master/src/%E7%AC%AC%E4%BA%8C%E7%AB%A0)
