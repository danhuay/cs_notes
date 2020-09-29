---
title: "8_memorization_search_and_dp"
date: 2020-02-06T21:05:15-05:00
draft: true
---


#### 动态规划的组成部分

1. 确定状态
	- 研究最优策略的最后一步（最后的一个决策）
	- 转化为子问题：完成最后一步的问题（问题一样，规模变小的问题叫做子问题）
	- 所以状态`$f(x)$`=子问题
2. 转移方程：用数组表示子问题
3. 初始条件和边界情况
	- 要解决数组越界的情况
	- 解决停止条件
	- 初始条件：一开始可以人为规定的情况，比方f(0)：用转移方程无法计算，需要手工定义的值
4. 计算顺序
	- 一般而言从小到大
	- 二维数组从上到下，从左到右
	- 根据转移方程，等式右边的值都已经计算完毕

#### 动态规划和递归的区别

递归解法做了很多重复计算，效率低下。

动态规划将结果保存下来，并改变了计算顺序。