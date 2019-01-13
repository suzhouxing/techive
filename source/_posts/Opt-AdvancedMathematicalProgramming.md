---
title: 数学规划进阶
date: 2019-01-13 11:14:59
categories:
- 技术
tags:
- 建模
- 优化
- 技术
- 数学
---
虽然目前最顶尖的商业求解器已经非常高效, 直接求解完整的问题模型往往比普通人自己实现算法性能更好, 但是面对特定的问题, 仍然有改进空间.
另一方面, 先贤们告诉我们要站在巨人的肩膀上, 要重用前人的优秀成果.
于是, 我们可以得到一个在完全自行设计算法和完全使用通用求解器之间的折中的方案, 发挥多种方法各自的优势, 更好地解决问题.



# 线性规划与混合整数规划 (进阶技巧)

## 非线性表达式

- 可以转化为线性表达式的非线性表达式
  - 最大值最小值
  - 绝对值
  - 分段线性函数
  - ...
- 可以较高效求解的非线性表达式
  - Quadratic Program (QP)
  - Special Ordered Set (SOS)
  - Mixed Integer Program (MIP)
  - ...

详见 `线性化非线性表达式.docx`.

### 最小化最大值

- 使用一个辅助变量限制决策变量的界
- 约束的可行区域应与优化方向 "相反"
  - 最小化一个表达式 $l = f(x)$, 应该确定 $l$ 的下界, 即 $g(x) \le l \le +\infty$
  - 最大化一个表达式 $u = f(x)$, 如果只约束其下界, 将导致目标无限增大

### 最小化最小值

- 最小值的下界? 让较大的值都可以忽略
- Big-M (充分大的 M)

### 课堂练习

- 最大化最大值?
- 最大化绝对值?


## 多目标

- 线性加权模式
  - 简单, 本质上就是单目标
  - 各目标量纲不同时难以确定权重系数
- 优先级模式
  - 高优先级对低优先级有压倒性优势
  - 高优先级目标有一定的绝对/相对容差范围

### 实现优先级模式

- 使用线性加权模式模拟
  - 难以确定各目标的取值范围以形成隔离
  - 目标过多容易溢出或者产生巨大的数值误差
- 迭代求解
  - 按优先级逐个优化
  - 每个目标算到最优解后根据容差范围添加约束限制目标函数的取值
  - 求解性能依赖热启动 (warm start) 效果


## 约束类型

| 类型              | 特征             | 最优性    | 完整性      | 检查时间       | 被忽略  |
| --------------- | -------------- | ------ | -------- | ---------- | ---- |
| user cut        | 排除显然不可能最优的解向量  | 不改变最优解 | 不影响模型的完整性 | 可能在任意时刻被检查 | 可能   |
| lazy constraint | 最优解不太可能违反的约束  | 会改变最优解 | 影响模型的完整性 | 仅在找到解时才被检查 | 不会   |
| constraint      | 容易与优化目标产生冲突的约束 | 会改变最优解 | 影响模型的完整性 | 在任意时刻都会被检查 | 不会   |
注: 在 MIP 中, 整数解才是原始问题的解, 故在找到松弛的实数解时不会触发惰性约束.

以下为 CPLEX 对惰性约束和用户割平面的介绍:

>In contrast to the cuts that IBM ILOG CPLEX may automatically add while solving a problem, user cuts are those cuts that a user defines based on information already implied about the problem by the constraints; user cuts may not be strictly necessary to the problem, but they tighten the model. Lazy constraints are constraints that the user knows are unlikely to be violated, and in consequence, the user wants them applied lazily, that is, only as necessary or not before needed. User cuts can be grouped together in a pool of user cuts. Likewise, lazy constraints can also be grouped into a pool of lazy constraints.
>
>Cuts may resemble ordinary constraints, but are conventionally defined to mean those which can change the feasible space of the continuous relaxation but do not rule out any feasible integer solution that the rest of the model permits. A collection of cuts, therefore, involves an element of freedom: whether or not to apply them, individually or collectively, during the optimization of a MIP model; the formulation of the model remains correct whether or not the cuts are included. This degree of freedom means that if valid and necessary constraints are mis-identified by the user and passed to CPLEX as user cuts, unpredictable and possibly incorrect results could occur.
>
>By contrast, lazy constraints represent simply one portion of the constraint set, and the model would be incomplete (and possibly would deliver incorrect answers) in their absence. CPLEX always makes sure that lazy constraints are satisfied before producing any solution to a MIP model. Needed lazy constraints are also kept in effect after the MIP optimization terminates, for example, when you change the problem type to fixed-integer and re-optimize with a continuous optimizer.
>
>Another important difference between pools of user cuts and pools of lazy constraints lies in the timing by which these pools are applied. CPLEX may check user cuts for violation and apply them at any stage of the optimization. Conversely, it does not guarantee to check them at the time an integer-feasible solution candidate has been identified. Lazy constraints are only (and always) checked when an integer-feasible solution candidate has been identified, and of course, any of these constraints that turn out to be violated will then be applied to the full model.
>
>Cuts that are based on **optimality** and that **remove** integer feasible solutions without removing all optimal solutions are known as **optimality-based cuts**. Optimality-based cuts do not fit the definition of either a user cut nor a lazy constraint. For example, **symmetry-breaking constraints** are sometimes known as optimality-based cuts because symmetry-breaking constraints can remove integer feasible solutions without removing all optimal solutions. Symmetry-breaking constraints are **not** user cuts in the sense addressed here. Symmetry-breaking constraints are not necessarily lazy constraints either. However, CPLEX can support optimality-based cuts as lazy constraints. If you add an optimality-based cut as a lazy constraint in your model, you can also add it to the user cut pool. This practice of adding an optimality-based cut as a lazy constraint and simultaneously adding it to the user cut pool makes sure that CPLEX checks the optimality-based cut at each node relaxation as well as when CPLEX finds an integer feasible solution.
>
>Another way of comparing these two types of pools is to note that the user designates constraints as lazy in the strong hope and expectation that they will not need to be applied, thus saving computation time by their absence from the working problem. In practice, it is relatively costly (for a variety of reasons) to apply a lazy constraint after a violation is identified, and so the user should err on the side of caution when deciding whether a constraint should be marked as lazy. In contrast, user cuts may be more liberally added to a model because CPLEX is not obligated to use any of them and can apply its own rules to govern their efficient use.

user cut 与约束编程 (Constraint Programming) 中的 surrogate constraints 功能类似:

>Since constraint propagation decreases the size of the search space by reducing the domains of variables, it is obviously important to express all necessary constraints. In some cases, it is even a good idea to introduce implicit constraints to reduce the size of the search space by supplementary propagation.
Processing supplementary constraints inevitably slows down execution. However, this slowing down may be negligible in certain problems when it is compared with the efficiency gained from reducing the size of the search space.
>
>A surrogate constraint makes explicit a property that satisfies a solution implicitly. Such a constraint should not change the nature of the solution, but its propagation should delimit the general shape of the solution more quickly.
>Of course, there is no need to express grossly obvious redundant constraints since the highly optimized algorithms that CP Optimizer uses to insure arc consistency already work well enough. For example, given this system of equations:
>$x = y + z$
>$z = a + b$
>no efficiency whatsoever is gained by adding this constraint:
>$x = y + a + b$
>However, in any case where an implicit property makes good sense, or derives from experience, or satisfies formal computations, its explicit implementation as a surrogate constraint can be beneficial.
>
>Consider the problem of the magic sequence. Assume that there are n+1 unknowns, namely, $x_0, x_1, . . . , x_n$. These $x_i$ must respect the following constraints:
>0 appears $x_0$ times in the solution.
>1 appears $x_1$ times.
>In general, $i$ appears $x_i$ times.
>$n$ appears $x_n$ times.
>The constraint of this problem can easily be written, using the specialized distribute constraint. However, the search for a solution can be greatly accelerated by introducing the following surrogate constraint that expresses the fact that $n+1$ numbers are counted.
>$1*x_1 + 2*x_2 + . . . + n*x_n = n+1$.


## 列生成 (Column Generation)

如果说 TSP 中经典的子回路消除 (割平面法) 是一种逐步添加惰性约束的 "行生成" 算法, 那么其对偶算法就是逐步添加决策变量的 "列生成" 算法.
前者适用于原始问题约束非常多, 但是真正对限制最优解的取值发挥作用的重要约束很少的情况;
后者则恰好相反, 适用于决策变量非常多, 但大多数决策变量的子集的取值组合不可能出现在最优解中的情况.


## 线性分数规划 (Linear-Fractional Programming)

目标函数为分数形式.
可以转换成线性规划.
