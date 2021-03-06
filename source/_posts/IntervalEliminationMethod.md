---
title: 二分法与黄金分割法的区别
date: 2018-01-14 09:19:00
categories:
- 数学
tags:
- 数学
- 优化
- 高中
---
高中数学选修课学过区间消去法, 用于求解简单的单变量无约束的优化问题.
其中的典型有二分法和黄金分割法 (0.618法), 后来还知道黄金分割法其实是斐波那契法的简化.
而很多人似乎并不清楚这两种方法的区别和适用场景.

读研之后一直在做运筹学相关的问题, 遇到的优化问题都是上百万维的多约束多目标的组合优化问题.
偶然间遇到了一个简单的一维的情况, 突然来了一种莫名的自信觉得自己可以比当时的高中数学老师讲得更清楚...

二分法主要用于寻找阶跃函数的突变点.

![阶跃函数](BisectionMethod.png)

例如二分查找, 定义比待查找元素小的元素目标函数值为 0, 比待查找元素大的元素目标函数值为 1, 则待查找元素为目标函数的突变点.

黄金分割法和斐波那契法用于寻找单峰函数的极值点.

![单峰函数](FibonacciMethod.png)

例如确定药物的最佳剂量, 定义治疗效果和副作用为目标函数, 则用量比最佳剂量大或者小时综合效果都更差.