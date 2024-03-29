---
title: 组合优化 (一) 简介
date: 2019-01-13 11:07:22
categories:
- 组合优化
tags:
- 组合优化
- 建模
- 优化
- 技术
- 数学
---
组合优化也叫离散优化, 是运筹优化的重要组成部分, 其中 "组合" 是排列组合的组合.
从字面上理解这个名词, 组合优化是要从呈组合数复杂度爆炸式增长的解空间中, 寻找最优的解向量, 即制定最优决策方案.
组合优化问题涉及了分配, 调度, 指挥, 路由等众多类型的问题.
作为人工智能的重要分支, 组合优化与时下大热的统计学习存在着千丝万缕的联系.
统计学习更侧重于预测和单步决策, 比如预测出了某件商品的销量, 就可以知道需要进多少货; 预测出了某个区域的人流量, 就可以知道需要分配多少保安巡逻; 检测出患者有某种疾病, 就可以知道要开什么药.
相比之下, 组合优化更注重涉及多方的, 全局的, 系统性的序列决策.
与此同时, 部分统计学习种的模型训练算法与求解组合优化问题的方法往往有异曲同工之妙, 因为离散优化与连续优化在思想上有很多相通之处.



# 组合优化基础

## 组合优化包含哪些问题

- 路由问题: 用开销最小的路径覆盖所有目的地
  - 车辆路由
  - 数据流量路由
- 指派问题: 在有限的时间和空间中合理使用软硬件资源创造更多的收益
  - 时间指派
    - 先后序调度
      - 单机作业调度
      - 车间流水线调度
    - 时间槽分配
      - 航班与列车时刻表
      - 人员排班表
      - 选修课表
  - 空间指派
    - 哪个背包装哪些物品: 背包问题
    - 哪个处理器处理作业: 多机作业调度
    - 哪个中心服务哪些客户: 中心选址
- ...
- NP 完全 (NP-Complete) 问题可以在多项式时间内相互规约


## 如何定义一个问题

- 基本要素
  - 已知: 输入数据
  - 决策: 输出结果
  - 约束: 输出结果可行还是不可行
  - 目标: 输出结果好还是坏
- 观察问题的不同角度举例
  - 图着色问题
    - 每个节点染什么颜色
    - 每种颜色的节点集合包含了哪些节点
  - 布尔表达式可满足性问题
    - 保证每个布尔变量在所有子句中取值一致, 最大化为真的子句数量
    - 保证每个子句均为真, 最大化布尔变量的一致性


## 基本求解方法分类

- 贪心算法: 在保证求解速度的前提下提升优度
  - 部分可以保证最优性的贪心算法往往也可以归类为动态规划 (例如 Dijkstra 最短路算法)
- 近似算法: 离最优解的差距有保障的贪心算法
- 精确算法: 在确保最优性的前提下降低复杂度
  - 深度/广度/优度优先树搜索
  - 动态规划
  - 混合整数规划的求解算法
- 启发式算法: 在优度和复杂度之间寻找平衡点
  - 基于邻域动作: 元启发式算法
    - 单个解 (Trajectory): 局部搜索
    - 多个解 (Population): 种群算法
  - 基于树搜索
    - A*
      - 启发函数可接受 (Admissible) 时为精确算法
    - 向前看树搜索 (Lookahead Tree Search)
    - 线搜索 (Beam Search)
    - 蒙特卡洛树搜索 (Monte-Carlo Tree Search)



# 问题归约与转换

## 经典问题到现实问题

- 图着色
  - 寄存器分配
    - 寄存器 => 颜色
    - 变量 => 节点
    - 两个变量生命周期有交集 => 不能使用同一个寄存器 => 不能染同一种颜色 => 两个节点间有一条边
  - 多业务波长分配
    - 波长 => 颜色
    - 路径 => 节点
    - 两条路径有交集 => 不能使用同一个波长 => 不能染同一种颜色 => 两个节点间有一条边
  - 停机位分配
    - 停机位 => 颜色
    - 飞机 => 节点
    - 两架飞机过站时间有交集 => 不能停在同一停机位 => 不能染同一种颜色 => 两个节点间有一条边
  - 宿舍分配
    - 宿舍 => 颜色
    - 学生 => 节点
    - 两个学生作息规律差异很大 => 不能住同一间宿舍 => 不能然同一种颜色 => 两个节点间有一条边

- 旅行销售员
  - 快递与外卖配送
  - 物资采购
  - 人类基因组计划


## 经典问题相互转换

### 独立集 <=> 最大团 <=> 顶点覆盖 => 支配集 <=> 集合覆盖 <= 中心选址

.

### 非对称旅行商 <=> 对称旅行商

.

### 必经点最短简单路 => 非对称旅行商 <=> 最短简单路 <=> 最长简单路

- 必经点最短路 => 非对称旅行商
  - 基本思路
    - 增加一条无代价的旁路让所有非必经点能够通过该旁路被访问
    - 从起点出发, 经过最短路上的实际节点序列, 到达终点, 到达旁路起点, 通过旁路依次经过不在最短路上的实际节点
  - 具体实现
    - 假设起点为 $s$, 终点为 $t$, 共有 $k$ 个非必经节点 $n_{1}, n_{2}, ..., n_{k}$
    - 增加 $k + 1$ 个虚拟节点 $v_{0}, v_{1}, ..., v_{k}$
    - 增加以下有向边
      - $t \rightarrow v_{k}$
      - $v_{i} \rightarrow v_{i-1}, \quad \forall i \in [1, k]$
      - $v_{i} \rightarrow n_{i}, \quad \forall i \in [1, k]$
      - $n_{i} \rightarrow v_{i-1}, \quad \forall i \in [1, k]$
      - $v_{0} \rightarrow s$
    - 上述有向边应满足
      - $cost(v_{i} \rightarrow v_{i-1}) = cost(v_{i} \rightarrow n_{i}) + cost(n_{i} \rightarrow v_{i-1})$
      - $cost(t \rightarrow v_{k}) = cost(v_{0} \rightarrow s) = 0$


## 经典问题分解

### 图着色 = 集合覆盖 + 独立集

.
