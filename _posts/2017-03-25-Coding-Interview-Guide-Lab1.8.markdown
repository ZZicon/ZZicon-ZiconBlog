---
layout:     post
title:      "数据结构最优解之栈与队列Lab8"
subtitle:   "最大子矩阵"
date:       2017-03-25 15:10:26
author:     "Zicon"
header-img: "img/post-bg.jpg"
catalog: true
tags:
    - 数据结构
---

> “程序设计中被广泛使用的两种线性结构“

---

# 正文

**题目：求最大子矩阵的大小**

要求是：给定一个整数矩阵map，其中值只有1与0，求其中全是1的所有矩形区域中最大的子矩阵中1的数量。当矩阵大小为N * W时，时间复杂度要求为O(N * W)。

**思路分析**

 - 矩阵行数为N，以每一行做切割，统计以当前行为底的情况下，每个位置往上的1的数量。使用数组heigh表示。
 - 而在每一次切割后都利用更新后的height数组求出当前最大的矩形，那么在多次切割后最大的矩形就是要求的。
 
例如：

map = |1|0|1|1|
	  |-|-|-|-|
	  |1|1|1|1|
	  |1|1|1|0|
 
 - 第一次切割后，height={1,0,1,1}；
 - 第二次切割后，height={2,1,2,2}；
 - 第三次切割后，height={3,2,3,0}；
 - height[i]表示目前的底上，i位置往上有多少连续的1.
 
第二步的寻找当前最大值有很对方式，在这里只介绍两种以供参考。

***第一种，暴力循环***

 - 遍历height数组，将height[i]记录为num，height[i]可延伸的长度记为l，可延伸指的是height[i]左右两边小于等于其自身的范围。
 - 获得num与l后求num*l。
 - 返回一个height数组中最大的num*l赋值max。

***第二种，以栈实现***

 - 遍历height数组，当栈为空或height[i]大于栈顶元素，将height[i]入栈。
 - 而如果栈顶元素大于或等于height[i]则弹出栈顶元素直到栈顶小于height[i]。
 - 对于弹出的栈顶元素height[j]，将新栈顶记为k，则j延伸的最大矩形为(i-k-1)*height[j]。
 
**代码实现**

```
public class Lab1_9 {
	//第一步
	public static int maxRecSize(int[][] map) {
		// map.lehgth是图的高度（行数）;map[0].length是图的宽度（列数）
		if (map == null || map.length == 0 || map[0].length == 0) {
			return 0;
		}
		int Max = 0;
		int height[] = new int[map[0].length];
		for (int i = 0; i < map.length; i++) {
			for (int j = 0; j < map[0].length; j++) {
				height[j] = map[i][j] == 0 ? 0 : height[j] + 1;
			}
			Max = maxNumFromHeight(height) > Max ? maxNumFromHeight(height)
					: Max;
		}
		return Max;
	}

	//暴力循环
	public static int maxNumInHeight(int[] height) {
		if (height == null || height.length == 0) {
			return 0;
		}
		int l = 0, num = 0, max = 0;
		for (int i = 0; i < height.length; i++) {
			l = 0;
			num = height[i];
			for (int j = i; j >= 0; j--) {
				if (num <= height[j]) {
					l++;
				} else {
					break;
				}
			}
			for (int k = i + 1; k < height.length; k++) {
				if (num <= height[k]) {
					l++;
				} else {
					break;
				}
			}
			max = Math.max(num * l, max);
		}
		return max;
	}

	//以栈实现
	public static int maxNumFromHeight(int[] height) {
		if (height == null || height.length == 0) {
			return 0;
		}
		int MaxArea = 0;
		Stack <Integer> stack = new Stack <Integer>();
		for (int i = 0; i < height.length; i++) {
			while (!stack.isEmpty()
					&& height[i] <= height[stack.peek()]) {
				int j = stack.pop();
				int k = stack.isEmpty() ? -1 : stack.peek();
				int Area = (i - k - 1) * height[j];
				MaxArea = Math.max(Area, MaxArea);
			}
			stack.push(i);
		}
		while (!stack.isEmpty()) {
			int j = stack.pop();
			int k = stack.isEmpty() ? -1 : stack.peek();
			int Area = (height.length- k - 1) * height[j];
			MaxArea = Math.max(Area, MaxArea);
		}
		return MaxArea;
	}

	public static void main(String args[]) {
		int[][] map = { { 1, 0, 1, 1 }, { 1, 1, 1, 1 }, { 1, 1, 1, 0 } };
		int max = maxRecSize(map);
		System.out.println(max);
	}
}	
```

暴力循环时间复杂度为O(N*W^2)，因为暴力循环中对于height数组的每个元素都要进行一次判断寻找最大矩形。

  
# 后记
Zicon将相关的程序代码存放在：[点击这里](https://github.com/ZZicon/Algorithm/tree/master/src/%E7%AC%AC%E4%B8%80%E7%AB%A0)
