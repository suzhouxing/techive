---
title: 组合优化 (二) 数学建模基础
date: 2019-01-13 11:10:16
categories:
- 组合优化
tags:
- 组合优化
- 建模
- 优化
- 技术
- 数学
---
面对一个复杂的组合优化问题, 把它描述清楚本身就不是一个简单的问题.
常规开发往往允许模棱两可的描述, 需求修修补补虽然很烦, 但往往只是工作量的问题.
而算法研发由于其专用性, 细微的需求变化可能导致一个算法完全失效, 除非重新设计整个框架不然无法解决新的问题.
所以, 严谨的需求分析或问题描述对于算法研发格外重要.
对于一个组合优化问题, 我们一般使用数学规划的形式化语言对其进行无二义性的定义, 作为算法工程中的需求分析文档.


# 需求分析

- 为什么要做需求分析
  - 研究一个课题的目的: 实现一个好的系统
  - 什么样的系统是好系统: 按事先制定的标准进行评价
  - 标准从哪里来: 需求分析
- 如何评价一个系统的好坏
  - 任务完成质量 (又快又好)
    - 完成速度
    - 完成效果
  - 建设成本
  - 运营成本
  - 安全性
    - 不受外部因素干扰
    - 从错误中恢复
  - 稳定性 (今天好, 明天也好; 这组数据好, 那组也好)
    - 代码逻辑正确性
    - 算法对各种场景的兼容性
      - 静态多样性
      - 动态随机性
- 简化问题
  - 优度和速度的平衡不仅可以在问题求解的过程中, 还可以在问题定义时
  - 简化问题会影响结果的正确性和优度
  - 举例
    - 宏观低速时使用经典力学而不是相对论
    - 同时处理多个业务简化为分多次每次处理一个业务
    - 减小负载和提升安全性转换为不重复经过节点

- 组合优化问题的需求分析
  - 按形式化描述语言分类
    - 数学规划 (Mathematical Programming)
        - 线性规划 (Linear Programming)
        - 混合整数规划 (Mixed-Integer Programming)
        - QP, QCP, SOCP, LFP
    - 约束编程 (Constraint Programming)
    - 布尔表达式可满足性问题 (Boolean Satisfiability Problem)
        - SAT Encoding
        - MAX-SAT Encoding
    - 规约 (Reduction)
        - Karp's 21 NP-complete problems
    - 动态规划
  - 按应用场景分类
    - 静态模型
    - 随机规划 (Stochastic Programming)
    - 鲁棒优化 (Robust Optimization)



# 线性规划与混合整数规划

## 思维方式

### 已知

- 约束和目标中的系数

### 决策

- 确定哪些可控因素需要做决策
  - 定义域
    - 布尔 / 整数 / 实数
    - 上界 / 下界
- 从最根本的需求开始
  - 显示决策 (根本需求)
  - 隐式决策 (客户不关心但十分重要)
  - 辅助决策 (将非线性约束转化为线性约束)
- 不够用再返回来补充
- 思考问题的角度影响决策变量的设置
  - 图着色问题
    - 每个节点染什么颜色
    - 每种颜色的节点集合包含了哪些节点
    - 每两个节点是否染了相同的颜色
  - 旅行销售员问题
    - 每个节点的第几个被访问
    - 第几个被访问的节点是哪个节点
    - 路径包含哪些边
  - 布尔表达式可满足性问题
    - 保证每个布尔变量在所有子句中取值一致，最大化为真的子句数量
    - 保证每个子句均为真，最大化布尔变量的一致性

### 约束

- 决策变量组成的表达式满足的关系
  - 等式或不等式 $f(x) ? g(x)$
  - 下标的取值范围 $\forall i \in V$
- 可以和目标对调顺序, 谁简单先考虑谁
- 如何验证约束是否完备?
  - 经验?
  - 详尽的测试?
  - ...

### 目标

- 不同决策变量取值组合产生的后果的优劣度量



## 冗余约束 (Redundant Constraints)

### 图着色问题的对称性消除 (Symmetric Breaking)

- 参考文献: [A cutting plane algorithm for graph coloring](https://doi.org/10.1016/j.dam.2006.07.010)
- 优化版本
  - O1 一个颜色至少被一个节点使用时才会被选中
    $$
    y_{c} \le \sum_{n \in N} x_{nc}, \quad \forall c \in C
    $$
  - O2 编号更小的颜色未被选中时禁止选中编号更大的颜色
    $$
    y_{c'} \ge y_{c}, \quad \forall c, c' \in C, c' = c - 1
    $$
- 判定版本
  - D1 使用编号更小的颜色的节点数不小于使用编号更大的颜色的节点数 (使用某种颜色的节点数随颜色编号的增长单调变化)
    $$
    \sum_{n \in N} x_{nc'} \ge \sum_{n \in N} x_{nc}, \quad \forall c, c' \in C, c' = c - 1
    $$
    - D1 包含 O2
  - D2 节点不允许使用比其编号更大的颜色
    $$
    x_{nc} = 0, \quad \forall n \in N, c \in C, n < c
    $$
  - D3 编号更小的颜色未被编号更小的节点使用时禁止使用编号更大的颜色 (使用某种颜色的节点集中最小的节点编号随颜色编号的增长单调变化)
    $$
    \sum_{n' \in [0, n)} x_{n'c'} \ge x_{nc}, \quad \forall n \in N, \forall c, c' \in C, c' = c - 1
    $$
    与 D2 配合使用时, 将固定为 0 的项代入可化简为
    $$
    \sum_{n' \in [c', n)} x_{n'c'} \ge x_{nc}, \quad \forall n \in N, \forall c, c' \in C, c' = c - 1, n \ge c
    $$
    - D3 与 D1 冲突


## 编程实现

### 求解器选择

- [Gurobi](http://www.gurobi.com/)
  - 目前最高效的线性规划/混合整数规划求解器
  - 支持免费学术许可证申请
- [CPLEX](https://www.ibm.com/software/commerce/optimization/cplex-optimizer/)
  - IBM旗下的 "行业标准"
- [SCIP](http://scip.zib.de/)
  - 号称最快的开源求解器
- [COIN-OR](https://projects.coin-or.org/Cbc)
  - 功能繁多的开源运筹学工具包
- [OR-Tools](https://github.com/google/or-tools)
  - 开源经典组合优化算法库
  - 提供统一的接口调用各大常见求解器

### 接口选择

- 编程语言接口
  - 开发中最常用的接口
  - 提供更高层次的抽象
  - 更快的模型构建速度
  - C / C++ / Java / C# / Python / Matlab / R
- 模型文件接口
  - 需要自己将模型展开为线性规划的标准型
  - 文件 I/O 速度较慢
- 交互式命令行接口 (Gurobi)
  - 类似于解释型的脚本语言
- OPLIDE (CPLEX)
  - 介于命令式语言与建模语言之间
- Excel 插件
- ...

### 使用编程语言接口的求解过程 (Gurobi)

参考自带的示例工程 diet, facility, mip1.
基本流程如下.

- 初始化环境 GRBEnv()
  - 许可证检测
  - 其他全局参数与数据初始化
- 初始化模型 GRBModel()
- 添加决策变量 GRBModel::addVar()
  - 按照一定规律组织决策变量
  - 使用额外的数据结构将决策变量与用户变量绑定
- 更新模型 GRBModel::update()
  - 惰性更新提升效率 (最新版已无需手动调用该函数)
  - CPLEX 不需要此步骤
- 设置目标 GRBModel::setObjective()
- 添加约束 GRBModel::addConstr()
- 设置其他参数 GRBModel::set(), GRBEnv::set()
  - 运行时间
  - 输出日志
- 求解 GRBModel::optimize()
- 检查求解状态 GRBModel::get(GRB_IntAttr_Status)
- 获取目标函数值 GRBModel::get(GRB_DoubleAttr_ObjVal)
- 获取解向量 GRBVar::get()

### 使用模型文件接口的求解过程 (Gurobi)

参考自带的示例工程 lp.
基本流程如下.

- 准备好 .mps 或 .lp 等模型文件 GRBModel::write()
  - 一般由另外的程序输出
- 读取模型文件 GRBModel(), GRBModel::read()
- 求解并输出
