---
layout:     post
title:      "数据结构最优解之栈与队列Lab7"
subtitle:   "构造最大生成树"
date:       2017-03-25 14:53:06
author:     "Zicon"
header-img: "img/post-bg.jpg"
catalog: true
tags:
    - 数据结构
---

> “程序设计中被广泛使用的两种线性结构“

---

# 正文

**题目：构造数组的最大生成树MaxTree**

要求是：给定一个没有重复元素的数组arr，满足生成树上每一个子节点都比父节点小。并且如果数组长度为n，时间复杂度为O(N)，空间复杂度为O(N)。

树的节点如下：

```
class Node {
	public int value;
	public Node left;
	public Node right;

	public Node(int data) {
		this.value = data;
	}
}
```

**思路分析**

 - 每一个数的父节点是它`左边第一个比它大`的数和它`右边第一个比它大`的数中，`较小`的那个。
 - 如果一个数的左边没有比它大的数，右边也没有，说明它是整个数组中`最大值`。

注意，生成的数不一定是二叉树。

那么我们要去寻找一个数左右两边第一个最大的数应该使用栈。

 - 遍历数组，新加入的数入栈时不断利用pop出栈顶元素进行比较，直到栈顶元素比新数大或栈空。此时，新数左边第一个比它大的数就是栈顶元素。
 - 同理，可求新数右边第一个比它大的数。
 - 保存栈内元素左右第一个比它大的数，生成最大生成树。

**代码实现**

```
public class Lab1_8 {
	public Node getMaxTree(int[] queue) {
		Node[] arr = new Node[queue.length];
		// 初始化节点数组存储数组元素
		for (int i = 0; i < queue.length; i++) {
			arr[i] = new Node(queue[i]);
		}
		Stack <Node> stack = new Stack <Node>();
		
		// Map保存左右节点联系
		HashMap <Node, Node> lBigMap = new HashMap <Node, Node>();
		HashMap <Node, Node> rBigMap = new HashMap <Node, Node>();
		
		//所有数的左边第一个比它大的数
		for (int i = 0; i < arr.length; i++) {
			Node curNode = arr[i];
			while ((!stack.isEmpty())
					&& stack.peek().value < curNode.value) {
				StackToMap(stack, lBigMap);
			}
			stack.push(curNode);
		}
		while (!stack.isEmpty()) {
			StackToMap(stack, lBigMap);
		}
		
		//所有数的右边第一个比它大的数
		for (int i = arr.length - 1; i > -1; i--) {
			Node curNode = arr[i];
			while ((!stack.isEmpty())
					&& stack.peek().value < curNode.value) {
				StackToMap(stack, rBigMap);
			}
			stack.push(curNode);
		}
		while (!stack.isEmpty()) {
			StackToMap(stack, rBigMap);
		}
		Node head = null;
		
		//生成树
		for (int i = 0; i < arr.length; i++) {
			Node curNode = arr[i];
			Node left = lBigMap.get(curNode);
			Node right = rBigMap.get(curNode);
			if (left == null && right == null) {
				head = curNode;
			} else if (left == null) {
				if (right.left == null) {
					right.left = curNode;
				} else {
					right.right = curNode;
				}
			} else if (right == null) {
				if (left.left == null) {
					left.left = curNode;
				} else {
					left.right = curNode;
				}
			} else {
				Node parent = left.value < right.value ? left
						: right;
				if (parent.left == null) {
					parent.left = curNode;
				} else {
					parent.right = curNode;
				}
			}
		}
		return head;
	}
	
	private void StackToMap(Stack <Node> stack, HashMap <Node, Node> map) {
		Node node = stack.pop();
		if (!stack.isEmpty()) {
			map.put(node, null);
		} else {
			map.put(node, stack.peek());
		}
	}
}
```

  
 
# 后记
Zicon将相关的程序代码存放在：[点击这里](https://github.com/ZZicon/Algorithm/tree/master/src/%E7%AC%AC%E4%B8%80%E7%AB%A0)
