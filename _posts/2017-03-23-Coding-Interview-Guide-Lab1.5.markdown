---
layout:     post
title:      "数据结构最优解之栈与队列Lab5"
subtitle:   "以栈排栈"
date:       2017-03-23 13:07:10
author:     "Zicon"
header-img: "img/post-bg.jpg"
catalog: true
tags:
    - 数据结构
---

> “程序设计中被广泛使用的两种线性结构“

---

# 正文

**题目：用一个栈实现另一个栈的排序**

要求是：只使用一个辅助栈将另一个栈中整数元素按从大到小的顺序进行排序。

**解题思路：**

假设要排序的栈为stack，辅助栈为stack2，在stack上进行pop操作，弹出的元素记为num。进行如下判断：

 - 当num小于或等于stack2的栈顶元素或stack2为空，num压入stack2。
 - 当num大于stack2的栈顶元素时，将stack2的元素逐一弹出压入stack，直到符合判断一。
 - 当stack为空且stack2不为空时，将stack2所有元素压入stack。

这道题目的实现难度很低，关键就在于代码的优化，如何最简洁的实现才是真正要考虑的。
 
**代码实现**

```
public class Lab1_5 {
	public static void sortStackByStack(Stack <Integer> stack) {
		Stack <Integer> stack2 = new Stack <Integer>();
		while (!stack.isEmpty()) {
			int num = stack.pop();
			while (num > stack2.peek() && !stack2.isEmpty()) {
				stack.push(stack2.pop());
			}
			stack2.push(num);
		}
		while (!stack2.isEmpty()) {
			stack.push(stack2.pop());
		}
	}
```
  
 
# 后记
Zicon将相关的程序代码存放在：[点击这里](https://github.com/ZZicon/Algorithm/tree/master/src/%E7%AC%AC%E4%B8%80%E7%AB%A0)
