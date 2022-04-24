---
title: SmartLab Challenge 2020 - Graph Coloring
date: 2020-12-09 10:28:35
categories:
- 算法挑战
tags:
- Challenge
- 算法挑战
- 组合优化
---
图着色问题在众多领域中有广泛的应用, 可以用来对各种各样的组合优化问题进行建模.
图着色问题主要研究独占资源的分配问题.
比如在编译器给变量分配寄存器分配时, 如果两个变量的生命周期有交集, 则不能分配至同一寄存器.
比如在机场分配停机位时, 如果两架飞机的过站时间有交集, 则不能分配至同一停机位.
比如在光纤网络中进行波长分配时, 如果两条光路经过同一条链路, 则不能使用同一波长传输.
比如移动基站进行通讯频率分配时, 如果两个终端设备位于同一组天线的覆盖范围内, 则不能使用同一通讯频率.
比如在学校排课表时, 如果两个班级要上同一位教师的课, 则其上课时间不能相同.
高效的图着色问题求解算法具有极其重要的理论与应用价值.



# 图着色算法训练

## 问题概述

给定一个无向图, 请为每个节点染一种颜色, 在任意一条无向边两端的节点颜色不同的情况下, 最小化使用的颜色数.

- 参考文献.
  - [1] Z. Lü and J.-K. Hao, “A memetic algorithm for graph coloring,” European Journal of Operational Research, vol. 203, no. 1, pp. 241–250, 2010, doi: 10.1016/j.ejor.2009.07.016.
  - [2] L. Moalic and A. Gondran, “Variations on memetic algorithms for graph coloring problems,” Journal of Heuristics, vol. 24, no. 1, pp. 1–24, 2018, doi: 10.1007/s10732-017-9354-9.


## 命令行参数

请大家编写程序时支持两个命令行参数, 依次为运行时间上限 (单位为秒) 和随机种子 (0-65535).
算例文件已重定向至标准输入 `stdin`/`cin`, 标准输出 `stdout`/`cout` 已重定向至解文件 (如需打印调试信息, 请使用标准错误输出 `stderr`/`cerr`).
例如, 在控制台运行以下命令表示调用可执行文件 `gcp.exe` 在限时 600 秒, 随机种子为 12345 的情况下求解路径为 `../data/DSJC500.5.txt` 的算例, 解文件输出至 `sln.dsjc500.5.txt`:
```
gcp.exe 600 123456 <../data/DSJC500.5.txt >sln.dsjc500.5.txt
```

- 运行时间上限.
  - 超出运行时间上限后测试程序会强行终止算法, 请确保在此之前已输出解 (最好还能自行正常退出).
- 随机种子设置.
  - 使用 C 语言随机数生成器请用 `srand`.
  - 使用 C++ 随机数生成器 (如 `mt19937`) 请在构造时传参或调用 `seed()` 方法设置.


## 输入的算例文件格式

所有算例的节点从 0 开始连续编号.

第一行给出 3 个由空白字符分隔的整数, 分别表示节点数 N, 无向边数 E (有向边数为 2E), 以及参考颜色数 C (相对容易求得可行解, 非最优颜色数).
接下来连续 E 行, 每行包含 2 个由空白字符分隔的整数, 表示一条无向边的两个端点.

例如, 以下算例文件表示节点数为 4, 无向边数为 3, 参考颜色数为 2; 其中:  
节点 0 分别与 1, 2, 3 相邻.
```
4 3 2
0 1
0 2
3 0
```


## 输出的解文件格式

输出 N 个用空白字符 (建议使用换行符) 分隔整数表示 N 个节点的染色情况, 第 i 个整数表示第 i 个节点的颜色.

颜色可以取 `int` 范围内任意非负整数, 检查程序自动统计不同的非负整数的数量.

例如, 以下解文件表示节点 0 染颜色 0, 节点 1 染颜色 1, 节点 2 染颜色 1, 节点 3 染颜色 1:
```
0
1
1
1

```


## 提交要求

- 发送至邮箱 [szx@duhe.tech](mailto:szx@duhe.tech).
- 邮件标题格式为 "**Challenge2020GCP-姓名-学校-专业**".
- 邮件附件为单个压缩包 (文件大小 2M 以内), 文件名为 "**姓名-学校-专业**", 其内包含下列文件.
  - **必要** 算法的可执行文件 (Windows 平台).
    - 建议基于官方 SDK 开发 ([https://gitee.com/suzhouxing/npbenchmark/tree/main/SDK.GCP](https://gitee.com/suzhouxing/npbenchmark/tree/main/SDK.GCP)).
    - 用 g++ 的同学编译时建议静态链接, 即添加 `-static-libgcc -static-libstdc++` 编译选项.
  - **必要** 算法源码.
    - 可重入 (可在同一线程内反复调用而不会出现数据初始化错误或内存泄漏).
    - 可并发 (可在同一进程内的多个线程同时运行多个算法求解实例而互不干扰, 满足此要求一般不能有全局的非只读变量).
    - 可伸缩 (数据结构可以根据算例规模动态申请内存, 而非根据预先指定的编译期常量进行内存分配).
  - **可选** 算法在各算例上的运行情况概要, 至少包括以下几项信息 (可选, 仅在无法成功调用算法输出可通过检查程序的解时作为参考).
    - 算例名.
    - 颜色数.
    - 计算耗时.
  - **可选** 算法在各算例上求得的颜色数最少的解文件 (可选, 仅在无法成功调用算法输出可通过检查程序的解时作为参考).
- 若成功提交, 在收到邮件时以及测试完成后系统均会自动发送邮件反馈提交情况.
  - 若测试结果较优, 可在排行榜页面看到自己的运行情况 ([https://gitee.com/suzhouxing/npbenchmark.data/tree/data](https://gitee.com/suzhouxing/npbenchmark.data/tree/data)).

例如:
```
苏宙行-华科-计科.zip
|   gc.exe
|   results.csv
|
+---src
|       main.cpp
|       algorithm.cpp
|       algorithm.h
|
+---results
        DSJC125.1.txt
        DSJC125.5.txt
        ...
```


## 算例清单

下载地址: [https://gitee.com/suzhouxing/npbenchmark.data/tree/data/GCP/Instance](https://gitee.com/suzhouxing/npbenchmark.data/tree/data/GCP/Instance)

算例规模从小到大依次为 (求解难度不一定随规模增加, 但 DSJC500.5 以前的算例应该都很容易求解):

DSJC0125.1  
DSJC0125.5  
DSJC0125.9  
DSJC0250.1  
DSJC0250.5  
DSJC0250.9  
DSJC0500.1  
DSJC0500.5  
DSJC0500.9  
DSJC1000.1  
DSJC1000.5  
DSJC1000.9  
