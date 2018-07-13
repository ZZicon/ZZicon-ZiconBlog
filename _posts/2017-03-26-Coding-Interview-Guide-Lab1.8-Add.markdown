---
layout:     post
title:      "数据结构最优解之栈与队列Lab8附加"
subtitle:   "最大子矩阵"
date:       2017-03-26 14:10:26
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

要求是：给定一个矩阵map，其中值可正可负，求其中所有矩形区域中元素之和最大的子矩阵。

**思路分析**

 - 假定原始矩阵的行数为M，那么对于子矩阵，它的行数可以是1到M的任何一个数，而且，对于一个K行（K < M）的子矩阵，它的第一行可以是原始矩阵的第1行到 M - K + 1 的任意一行。
 - 在每一种情况里找出一个最大的子矩阵，原理与求“最大子段和问题” 是一样的。
 
例如：

map = |9|2|-6|2|
	  |-|-|-|-|
	  |-4|1|-4|1|
	  |-1|8|0|-2|

	  
**代码实现**

```
public class Lab_add {

	// 一维数组获取最大值
	public static int maxqueue(int[] queue) {
		if (queue.length == 0) {
			return 0;
		}
		int max = 0;
		int[] maxque = new int[queue.length];
		maxque[0] = queue[0];
		for (int i = 1; i < queue.length; i++) {
			maxque[i] = (maxque[i - 1] > 0) ? (maxque[i] + queue[i])
					: queue[i];
			if (max < maxque[i]) {
				max = maxque[i];
			}
		}
		return max;
	}

	// 把多维数组降为一维
	public static int[][] down(int[][] map) {
		int[][] res = map;
		for (int i = 1; i < map[0].length; i++) {
			for (int j = 0; j < map.length; j++) {
				res[i][j] += res[i - 1][j];
			}
		}
		return res;
	}

	// 获取最大子矩阵
	public static int maxMatrix(int[][] map) {
		int[][] res = down(map);
		int max = 0;
		for (int i = 0; i < map.length; i++) {
			for (int j = i; j < map.length; j++) {
				int[] result = new int[map[0].length];
				for (int k = 0; k < map[0].length; k++) {
					if (i == 0) {
						result[k] = res[j][k];
					} else {
						result[k] = res[j][k] - res[i - 1][k];
					}
				}
				max = maxqueue(result) > max ? maxqueue(result) : max;
			}
		}
		return max;
	}

	public static void main(String args[]) {
		int map[][] = { { 9, 2, -6, 2 }, { -4, 1, -4, 1 },
				{ -1, 8, 0, -2 } };
		int max = maxMatrix(map);
		System.out.println(max);
	}
}
```

# 后记
Zicon将相关的程序代码存放在：[点击这里](https://github.com/ZZicon/Algorithm/tree/master/src/%E7%AC%AC%E4%B8%80%E7%AB%A0)
