---
title: SmartLab Challenge 2022 - Obstacle-Avoiding Rectilinear Steiner Minimum Tree
date: 2022-03-25 16:19:42
categories:
- 算法挑战
tags:
- Challenge
- 算法挑战
- 组合优化
---

斯坦纳树问题是芯片设计, 管道规划与网络设计等领域中的重要问题, 应用场景十分广泛.
而避障直线最小斯坦纳树问题作为一类特殊的斯坦纳树问题, 是数字电路物理设计中全局布线环节的核心问题之一.
避障直线最小斯坦纳树问题主要研究如何用不穿过障碍的正交线段将平面上的一系列节点连接起来, 使得线段总长度最短的问题.
比如在 ASIC 的经典设计流程中, 需要在所有元件布局已经固定的情况下为每个网规划无短路和断路的导线连接方案.
比如在 FPGA 的物理综合过程中, 需要确定逻辑门阵列间的数据通路以实现目标电路的功能.
因此, 高效的避障直线最小斯坦纳树问题的求解算法具有极其重要的理论与应用价值.



# 矩形装箱算法训练

## 问题概述

在平面直角坐标中, 给定一系列节点和矩形障碍, 请确定一系列正交线段 (与 x 轴或 y 轴平行) 的起点和终点, 使得在所有线段均不穿过障碍且任意两点均存在由连续线段构成的路径的情况下, 线段的总长度最短.

- 参考文献.
  - [1] T. Huang, L. Li, and E. F. Y. Young, “On the Construction of Optimal Obstacle-Avoiding Rectilinear Steiner Minimum Trees,” IEEE Transactions on Computer-Aided Design of Integrated Circuits and Systems, vol. 30, no. 5, pp. 718–731, May 2011, doi: 10.1109/TCAD.2010.2098930.
  - [2] C. Liu, S. Kuo, D. T. Lee, C. Lin, J. Weng, and S. Yuan, “Obstacle-Avoiding Rectilinear Steiner Tree Construction: A Steiner-Point-Based Algorithm,” IEEE Transactions on Computer-Aided Design of Integrated Circuits and Systems, vol. 31, no. 7, pp. 1050–1060, Jul. 2012, doi: 10.1109/TCAD.2012.2185050.
  - [3] H. Zhang and D.-Y. Ye, “Key-node-based local search discrete artificial bee colony algorithm for obstacle-avoiding rectilinear Steiner tree construction,” Neural Computing and Applications, vol. 26, no. 4, pp. 875–898, May 2015, doi: 10.1007/s00521-014-1760-4.
  - [4] H. Zhang, D. Ye, and W. Guo, “A heuristic for constructing a rectilinear Steiner tree by reusing routing resources over obstacles,” Integration, vol. 55, pp. 162–175, Sep. 2016, doi: 10.1016/j.vlsi.2016.06.001.



## 命令行参数

请大家编写程序时支持两个命令行参数, 依次为运行时间上限 (单位为秒) 和随机种子 (0-65535).
算例文件已重定向至标准输入 `stdin`/`cin`, 标准输出 `stdout`/`cout` 已重定向至解文件 (如需打印调试信息, 请使用标准错误输出 `stderr`/`cerr`).
例如, 在控制台运行以下命令表示调用可执行文件 `oarsmt.exe` 在限时 300 秒, 随机种子为 12345 的情况下求解路径为 `../data/rc01.n10o10.txt` 的算例, 解文件输出至 `sln.rc01.n10o10.txt`:
```
oarsmt.exe 300 123456 <../data/rc01.n10o10.txt >sln.rc01.n10o10.txt
```

- 运行时间上限.
  - 超出运行时间上限后测试程序会强行终止算法, 请确保在此之前已输出解 (最好还能自行正常退出).
- 随机种子设置.
  - 使用 C 语言随机数生成器请用 `srand`.
  - 使用 C++ 随机数生成器 (如 `mt19937`) 请在构造时传参或调用 `seed()` 方法设置.


## 输入的算例文件格式

所有算例的节点和障碍分别从 0 开始连续编号.
所有坐标均为整数, 所有区间均为闭区间 (线段包含两个端点, 障碍包含边框).

第一行给出 2 个由空白字符分隔的整数, 分别表示节点数量 N 和障碍数量 O.
接下来连续 N 行, 每行包含 2 个由空白字符分隔的整数, 分别表示节点的横坐标和纵坐标.
接下来连续 O 行, 每行包含 4 个由空白字符分隔的整数, 分别表示障碍的最小横坐标, 最小纵坐标, 最大横坐标, 最大纵坐标.

例如, 以下算例文件表示有 4 个节点和 3 个障碍; 其中,  
节点 0 的坐标为 (1, 3);  
节点 1 的坐标为 (3, 1);  
节点 2 的坐标为 (0, 2);  
节点 3 的坐标为 (0, 3);  
障碍 0 为 (0, 0) 与 (1, 1) 的包络矩形;  
障碍 1 为端点为 (1, 1) 与 (2, 1) 的线段;  
障碍 2 为 (1, 1) 所在位置:
```
4 3
1 3
3 1
0 2
0 3
0 0 1 1
1 1 2 1
1 1 1 1
```


## 输出的解文件格式

输出 K 行整数表示 K 条路径的形状, 第 i 行表示第 i 条路径的起点, 转折点, 终点坐标.
每行包含至少 4 个由空白字符分隔的整数或字母, 第一个整数表示第 i 条路径的起点横坐标, 第二个整数表示第 i 条路径的起点纵坐标.
随后连续若干由空白字符分隔的二元组, 二元组 (O, D) 表示路径沿方向 O 延长距离 D, 其中方向 O 的取值为字母 x 或 y.

例如, 以下解文件表示 3 条路径的形状; 其中,  
路径 0 的起点坐标为 (0, 2), 先纵坐标 +1 到达途经点 (0, 3), 然后横坐标 +1 到达终点 (1, 3);  
路径 1 的起点坐标为 (1, 3), 横坐标 +2 到达终点坐标为 (3, 3);  
路径 2 的起点坐标为 (3, 3), 纵坐标 -2 到达终点坐标为 (3, 1):
```
0 2 y 1 x 1
1 3 x 2
3 3 y -2

```


## 提交要求

- 发送至邮箱 [szx@duhe.tech](mailto:szx@duhe.tech).
- 邮件标题格式为 "**Challenge2022OARSMT-姓名-学校-专业**".
- 邮件附件为单个压缩包 (文件大小 2M 以内), 文件名为 "**姓名-学校-专业**", 其内包含下列文件.
  - **必要** 算法的可执行文件 (Windows 平台).
    - 建议基于官方 SDK 开发 ([https://gitee.com/suzhouxing/npbenchmark/tree/main/SDK.OARSMT](https://gitee.com/suzhouxing/npbenchmark/tree/main/SDK.OARSMT)).
    - 用 g++ 的同学编译时请静态链接, 即添加 `-static-libgcc -static-libstdc++` 编译选项.
  - **必要** 算法源码.
    - 可重入 (可在同一线程内反复调用而不会出现数据初始化错误或内存泄漏).
    - 可并发 (可在同一进程内的多个线程同时运行多个算法求解实例而互不干扰, 满足此要求一般不能有全局的非只读变量).
    - 可伸缩 (数据结构可以根据算例规模动态申请内存, 而非根据预先指定的编译期常量进行内存分配).
  - **可选** 算法在各算例上的运行情况概要, 至少包括以下几项信息 (仅在无法成功调用算法输出可通过检查程序的解时作为参考).
    - 算例名.
    - 总线长.
    - 计算耗时.
  - **可选** 算法在各算例上求得包络矩形面积最小的解文件 (仅在无法成功调用算法输出可通过检查程序的解时作为参考).
- 其他所有问题通用的要求见 {% post_link Contest-ReadMe 'SmartLab Challenge - ReadMe' %}.

例如:
```
苏宙行-华科-计科.zip
|   packing.exe
|   results.csv
|
+---src
|       main.cpp
|       algorithm.cpp
|       algorithm.h
|
+---results
        rc01.n10o10.txt
        rc02.n30o10.txt
        ...
```


## 算例清单

下载地址: [https://gitee.com/suzhouxing/npbenchmark.data/tree/data/OARSMT/Instance](https://gitee.com/suzhouxing/npbenchmark.data/tree/data/OARSMT/Instance)

ind1.n10o32  
ind2.n10o43  
ind3.n10o50  
ind4.n25o79  
ind5.n33o71  
rt01.n10o500  
rt02.n50o500  
rt03.n100o500  
rt04.n100o1000  
rt05.n200o2000  
rc01.n10o10  
rc02.n30o10  
rc03.n50o10  
rc04.n70o10  
rc05.n100o10  
rc06.n100o500  
rc07.n200o500  
rc08.n200o800  
rc09.n200o1000  
rc10.n500o100  
rc11.n1000o100  
rc12.n1000o10000  
rl01.n5000o5000  
rl02.n10000o500  
rl03.n10000o100  
rl04.n10000o10  
rl05.n10000o0  
bonn.n109o101  
bonn.n23292o54  
bonn.n35574o158  
bonn.n46269o127  
bonn.n108500o141  
bonn.n129399o210  
bonn.n639639o382  
bonn.n783352o175  
