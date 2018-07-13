---
layout:     post
title:      "数据结构最优解之链表Lab2.1"
subtitle:   "链表的公共部分"
date:       2017-03-26 15:38:16
author:     "Zicon"
header-img: "img/post-bg.jpg"
catalog: true
tags:
    - 数据结构
---

> “非连续非顺序、动态地进行存储分配的存储结构“


# 前言

接下来，Zicon将正式开展关于链表的学习总结哦。

---

# 正文

**题目：打印两个链表的公共部分**

要求是：对于两个有序链表的头指针head1、head2，打印两个链表的公共部分

**思路分析**
 
对有序链表，所以对两个链表的头指针开始进行遍历判断。

 - 如果head1的值小于head2，head1后移。
 - 如果head1的值大于head2，head1后移。
 - head1的值等于head2的值，打印这个值，然后head1、head2同时后移。
 - 当head1或head2其中一个移动到null，结束。
 
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
public class Lab2_1 {
	public void printCommonPast(Node head1, Node head2) {
		while (head1 != null && head2 != null) {
			if (head1.value > head2.value) {
				head1 = head1.next;
			} else if (head1.value < head2.value) {
				head2 = head2.next;
			} else {
				System.out.println(head1.value + " ");
				head1 = head1.next;
				head2 = head2.next;
			}
		}
	}
}
```  
 
# 后记
Zicon将相关的程序代码存放在：[点击这里](https://github.com/ZZicon/Algorithm/tree/master/src/%E7%AC%AC%E4%BA%8C%E7%AB%A0)
