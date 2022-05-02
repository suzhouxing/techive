---
title: SmartLab Challenge 2021 - Vehicle Routing Problem with Time Windows
date: 2021-09-27 09:47:34
categories:
- 算法挑战
tags:
- Challenge
- 算法挑战
- 组合优化
---
带时间窗的车辆路由问题是交通运输与物流配送等领域中的重要问题, 其应用场景在生产生活中随处可见.
带时间窗的车辆路由问题主要研究如何确定一系列从同一个仓库出发的车辆的行驶路径, 在每辆车均不超载且每个客户都在给定时间窗内被访问的前提下, 最小化所有车辆的行驶时间之和.
比如快递配送过程中, 配送员能携带的物品重量有限, 同时每个客户只在特定的时段内有空收件, 快递公司需要调度配送员用最短的工时完成所有配送任务.
相反地, 在物流公司提供上门取件服务时, 揽件人员也会面临同样的优化问题.
因此, 高效的带时间窗的车辆路由问题的求解算法在理论上与实践上均意义重大.



# 带时间窗的车辆路由算法训练

## 问题概述

给定一个有向完全图, 图中包含一个仓库节点和若干客户节点.
每个客户节点都对应一个配送请求, 需要一个在指定的时间窗内将给定重量的物品一次性送达, 交接过程花费固定的时间.
此外, 每辆车的最大载重量相同且均需从仓库出发最后回到仓库, 同时任意两点间的最短行驶时间已知 (超时视为到达节点后原地等待).
请确定使用几辆车参与配送, 并给出每辆车依次访问的有序节点列表, 使得所有车辆的总行驶时间最短.

- 参考文献.
  - [1] M. M. Solomon, “Algorithms for the Vehicle Routing and Scheduling Problems with Time Window Constraints,” Operations Research, vol. 35, no. 2, pp. 254–265, 1987, doi: 10.1287/opre.35.2.254.
  - [2] Y. Nagata and O. Bräysy, “A powerful route minimization heuristic for the vehicle routing problem with time windows,” Operations Research Letters, vol. 37, no. 5, pp. 333–338, 2009, doi: 10.1016/j.orl.2009.04.006.
  - [3] Y. Nagata, O. Bräysy, and W. Dullaert, “A penalty-based edge assembly memetic algorithm for the vehicle routing problem with time windows,” Computers & Operations Research, vol. 37, no. 4, pp. 724–737, 2010, doi: 10.1016/j.cor.2009.06.022.
  - [4] R. Baldacci, A. Mingozzi, and R. Roberti, “New Route Relaxation and Pricing Strategies for the Vehicle Routing Problem,” Operations Research, vol. 59, no. 5, pp. 1269–1283, 2011, doi: 10.1287/opre.1110.0975.
  - [5] http://dimacs.rutgers.edu/programs/challenge/vrp/vrptw
  - [6] http://dimacs.rutgers.edu/files/7616/3155/5530/VRPTW_Competition_Rules.pdf
  - [7] https://www.sintef.no/projectweb/top/vrptw/homberger-benchmark/200-customers


## 命令行参数

请大家编写程序时支持两个命令行参数, 依次为运行时间上限 (单位为秒) 和随机种子 (0-65535).
算例文件已重定向至标准输入 `stdin`/`cin`, 标准输出 `stdout`/`cout` 已重定向至解文件 (如需打印调试信息, 请使用标准错误输出 `stderr`/`cerr`).
例如, 在控制台运行以下命令表示调用可执行文件 `vrptw.exe` 在限时 300 秒, 随机种子为 12345 的情况下求解路径为 `../data/solomon.c101.txt` 的算例, 解文件输出至 `sln.solomon.c101.txt`:
```
vrptw.exe 300 123456 <../data/solomon.c101.txt >sln.solomon.c101.txt
```

- 运行时间上限.
  - 超出运行时间上限后测试程序会强行终止算法, 请确保在此之前已输出解 (最好还能自行正常退出).
- 随机种子设置.
  - 使用 C 语言随机数生成器请用 `srand`.
  - 使用 C++ 随机数生成器 (如 `mt19937`) 请在构造时传参或调用 `seed()` 方法设置.


## 输入的算例文件格式

所有算例的节点和车辆分别从 0 开始连续编号, 所有节点分布于平面直角坐标系中.
节点 0 为仓库, 即车辆的起点和终点, 其物品重量, 最短停留时间, 时间窗开始时间均为 0.

第一行给出 3 个由空白字符分隔的整数, 分别表示节点数 N, 最大可用车辆数 K, 以及各车辆的载重量上限 M.
接下来连续 N 行, 每行包含 6 个由空白字符分隔的整数, 第 i 行的 6 个整数依次表示第 i 个节点的 x 坐标, y 坐标, 物品重量, 最短停留时间 (服务时间), 最早到达时间, 最晚到达时间.

例如, 以下算例文件表示有 4 个节点和 3 辆车, 每辆车的容量为 10; 其中,  
节点 0 的坐标为 (1, 5), 物品重量为 0, 最短停留时间为 0, 时间窗开始和结束时间分别为 0 和 8;  
节点 1 的坐标为 (2, 1), 物品重量为 6, 最短停留时间为 3, 时间窗开始和结束时间分别为 4 和 5;  
节点 2 的坐标为 (3, 7), 物品重量为 2, 最短停留时间为 4, 时间窗开始和结束时间分别为 5 和 6;  
节点 3 的坐标为 (8, 8), 物品重量为 9, 最短停留时间为 5, 时间窗开始和结束时间分别为 6 和 7:
```
4 3 10
1 5 0 0 0 8
2 1 6 3 4 5
3 7 2 4 5 6
8 8 9 5 6 7
```

注意, 本问题所用算例集中假设行驶时间等于两点间的距离, 目前的距离计算遵循文献与 DIMACS 竞赛中的约定, 使用截断保留一位小数的欧氏距离 ($t = \lfloor 10 d \rfloor / 10$), 具体计算方式以 SDK 中的代码为准.


## 输出的解文件格式

输出 V 行整数表示 V 辆车 (V < K) 的行驶路径, 第 i 行第 j 个整数表示第 i 辆车经过的第 j 个客户节点.

例如, 以下解文件表示 2 辆车的行驶路径; 其中,  
车辆 0 从仓库 0 出发后依次经过节点 3, 最后回到仓库 0;  
车辆 1 从仓库 0 出发后依次经过节点 1 和 2, 最后回到仓库 0:
```
3
1 2
```


## 提交要求

- 发送至邮箱 [szx@duhe.tech](mailto:szx@duhe.tech).
- 邮件标题格式为 "**Challenge2021VRPTW2d-姓名-学校-专业**".
- 邮件附件为单个压缩包, 文件名为 "**姓名-学校-专业**", 其内包含下列文件.
  - **必要** 算法的可执行文件 (Windows 平台).
    - 建议基于官方 SDK 开发 ([https://gitee.com/suzhouxing/npbenchmark/tree/main/SDK.VRPTW2d](https://gitee.com/suzhouxing/npbenchmark/tree/main/SDK.VRPTW2d)).
    - 用 g++ 的同学编译时请静态链接, 即添加 `-static-libgcc -static-libstdc++` 编译选项.
  - **必要** 算法源码.
    - 可重入 (可在同一线程内反复调用而不会出现数据初始化错误或内存泄漏).
    - 可并发 (可在同一进程内的多个线程同时运行多个算法求解实例而互不干扰, 满足此要求一般不能有全局的非只读变量).
    - 可伸缩 (数据结构可以根据算例规模动态申请内存, 而非根据预先指定的编译期常量进行内存分配).
  - **可选** 算法在各算例上的运行情况概要, 至少包括以下几项信息 (仅在无法成功调用算法输出可通过检查程序的解时作为参考).
    - 算例名.
    - 使用的车辆数.
    - 所有车辆的行驶时长总和.
    - 计算耗时.
  - **可选** 算法在各算例上求得的路径总长度最短的解文件 (仅在无法成功调用算法输出可通过检查程序的解时作为参考).
- 若成功提交, 在收到邮件时以及测试完成后系统均会自动发送邮件反馈提交情况.
  - 若测试结果较优, 可在排行榜页面看到自己的运行情况 ([https://gitee.com/suzhouxing/npbenchmark.data/tree/data#vrptw2d](https://gitee.com/suzhouxing/npbenchmark.data/tree/data#vrptw2d)).

例如:
```
苏宙行-华科-计科.zip
|   vrptw2d.exe
|   results.csv
|
+---src
|       main.cpp
|       algorithm.cpp
|       algorithm.h
|
+---results
        solomon.c101.n101v25c200.txt
        solomon.c102.n101v25c200.txt
        ...
```


## 算例清单

下载地址: [https://gitee.com/suzhouxing/npbenchmark.data/tree/data/VRPTW2d/Instance](https://gitee.com/suzhouxing/npbenchmark.data/tree/data/VRPTW2d/Instance)

solomon.c101.n101v25c200  
solomon.c102.n101v25c200  
solomon.c103.n101v25c200  
solomon.c104.n101v25c200  
solomon.c105.n101v25c200  
solomon.c106.n101v25c200  
solomon.c107.n101v25c200  
solomon.c108.n101v25c200  
solomon.c109.n101v25c200  
solomon.c201.n101v25c700  
solomon.c202.n101v25c700  
solomon.c203.n101v25c700  
solomon.c204.n101v25c700  
solomon.c205.n101v25c700  
solomon.c206.n101v25c700  
solomon.c207.n101v25c700  
solomon.c208.n101v25c700  
solomon.r101.n101v25c200  
solomon.r102.n101v25c200  
solomon.r103.n101v25c200  
solomon.r104.n101v25c200  
solomon.r105.n101v25c200  
solomon.r106.n101v25c200  
solomon.r107.n101v25c200  
solomon.r108.n101v25c200  
solomon.r109.n101v25c200  
solomon.r110.n101v25c200  
solomon.r111.n101v25c200  
solomon.r112.n101v25c200  
solomon.r201.n101v25c1000  
solomon.r202.n101v25c1000  
solomon.r203.n101v25c1000  
solomon.r204.n101v25c1000  
solomon.r205.n101v25c1000  
solomon.r206.n101v25c1000  
solomon.r207.n101v25c1000  
solomon.r208.n101v25c1000  
solomon.r209.n101v25c1000  
solomon.r210.n101v25c1000  
solomon.r211.n101v25c1000  
solomon.rc101.n101v25c200  
solomon.rc102.n101v25c200  
solomon.rc103.n101v25c200  
solomon.rc104.n101v25c200  
solomon.rc105.n101v25c200  
solomon.rc106.n101v25c200  
solomon.rc107.n101v25c200  
solomon.rc108.n101v25c200  
solomon.rc201.n101v25c1000  
solomon.rc202.n101v25c1000  
solomon.rc203.n101v25c1000  
solomon.rc204.n101v25c1000  
solomon.rc205.n101v25c1000  
solomon.rc206.n101v25c1000  
solomon.rc207.n101v25c1000  
solomon.rc208.n101v25c1000  
homberger.c10201.n201v50c200  
homberger.c10202.n201v50c200  
homberger.c10203.n201v50c200  
homberger.c10204.n201v50c200  
homberger.c10205.n201v50c200  
homberger.c10206.n201v50c200  
homberger.c10207.n201v50c200  
homberger.c10208.n201v50c200  
homberger.c10209.n201v50c200  
homberger.c10210.n201v50c200  
homberger.c10401.n401v100c200  
homberger.c10402.n401v100c200  
homberger.c10403.n401v100c200  
homberger.c10404.n401v100c200  
homberger.c10405.n401v100c200  
homberger.c10406.n401v100c200  
homberger.c10407.n401v100c200  
homberger.c10408.n401v100c200  
homberger.c10409.n401v100c200  
homberger.c10410.n401v100c200  
homberger.c10601.n601v150c200  
homberger.c10602.n601v150c200  
homberger.c10603.n601v150c200  
homberger.c10604.n601v150c200  
homberger.c10605.n601v150c200  
homberger.c10606.n601v150c200  
homberger.c10607.n601v150c200  
homberger.c10608.n601v150c200  
homberger.c10609.n601v150c200  
homberger.c10610.n601v150c200  
homberger.c10801.n801v200c200  
homberger.c10802.n801v200c200  
homberger.c10803.n801v200c200  
homberger.c10804.n801v200c200  
homberger.c10805.n801v200c200  
homberger.c10806.n801v200c200  
homberger.c10807.n801v200c200  
homberger.c10808.n801v200c200  
homberger.c10809.n801v200c200  
homberger.c10810.n801v200c200  
homberger.c11001.n1001v250c200  
homberger.c11002.n1001v250c200  
homberger.c11003.n1001v250c200  
homberger.c11004.n1001v250c200  
homberger.c11005.n1001v250c200  
homberger.c11006.n1001v250c200  
homberger.c11007.n1001v250c200  
homberger.c11008.n1001v250c200  
homberger.c11009.n1001v250c200  
homberger.c11010.n1001v250c200  
homberger.c20201.n201v50c700  
homberger.c20202.n201v50c700  
homberger.c20203.n201v50c700  
homberger.c20204.n201v50c700  
homberger.c20205.n201v50c700  
homberger.c20206.n201v50c700  
homberger.c20207.n201v50c700  
homberger.c20208.n201v50c700  
homberger.c20209.n201v50c700  
homberger.c20210.n201v50c700  
homberger.c20401.n401v100c700  
homberger.c20402.n401v100c700  
homberger.c20403.n401v100c700  
homberger.c20404.n401v100c700  
homberger.c20405.n401v100c700  
homberger.c20406.n401v100c700  
homberger.c20407.n401v100c700  
homberger.c20408.n401v100c700  
homberger.c20409.n401v100c700  
homberger.c20410.n401v100c700  
homberger.c20601.n601v150c700  
homberger.c20602.n601v150c700  
homberger.c20603.n601v150c700  
homberger.c20604.n601v150c700  
homberger.c20605.n601v150c700  
homberger.c20606.n601v150c700  
homberger.c20607.n601v150c700  
homberger.c20608.n601v150c700  
homberger.c20609.n601v150c700  
homberger.c20610.n601v150c700  
homberger.c20801.n801v200c700  
homberger.c20802.n801v200c700  
homberger.c20803.n801v200c700  
homberger.c20804.n801v200c700  
homberger.c20805.n801v200c700  
homberger.c20806.n801v200c700  
homberger.c20807.n801v200c700  
homberger.c20808.n801v200c700  
homberger.c20809.n801v200c700  
homberger.c20810.n801v200c700  
homberger.c21001.n1001v250c700  
homberger.c21002.n1001v250c700  
homberger.c21003.n1001v250c700  
homberger.c21004.n1001v250c700  
homberger.c21005.n1001v250c700  
homberger.c21006.n1001v250c700  
homberger.c21007.n1001v250c700  
homberger.c21008.n1001v250c700  
homberger.c21009.n1001v250c700  
homberger.c21010.n1001v250c700  
homberger.r10201.n201v50c200  
homberger.r10202.n201v50c200  
homberger.r10203.n201v50c200  
homberger.r10204.n201v50c200  
homberger.r10205.n201v50c200  
homberger.r10206.n201v50c200  
homberger.r10207.n201v50c200  
homberger.r10208.n201v50c200  
homberger.r10209.n201v50c200  
homberger.r10210.n201v50c200  
homberger.r10401.n401v100c200  
homberger.r10402.n401v100c200  
homberger.r10403.n401v100c200  
homberger.r10404.n401v100c200  
homberger.r10405.n401v100c200  
homberger.r10406.n401v100c200  
homberger.r10407.n401v100c200  
homberger.r10408.n401v100c200  
homberger.r10409.n401v100c200  
homberger.r10410.n401v100c200  
homberger.r10601.n601v150c200  
homberger.r10602.n601v150c200  
homberger.r10603.n601v150c200  
homberger.r10604.n601v150c200  
homberger.r10605.n601v150c200  
homberger.r10606.n601v150c200  
homberger.r10607.n601v150c200  
homberger.r10608.n601v150c200  
homberger.r10609.n601v150c200  
homberger.r10610.n601v150c200  
homberger.r10801.n801v200c200  
homberger.r10802.n801v200c200  
homberger.r10803.n801v200c200  
homberger.r10804.n801v200c200  
homberger.r10805.n801v200c200  
homberger.r10806.n801v200c200  
homberger.r10807.n801v200c200  
homberger.r10808.n801v200c200  
homberger.r10809.n801v200c200  
homberger.r10810.n801v200c200  
homberger.r11001.n1001v250c200  
homberger.r11002.n1001v250c200  
homberger.r11003.n1001v250c200  
homberger.r11004.n1001v250c200  
homberger.r11005.n1001v250c200  
homberger.r11006.n1001v250c200  
homberger.r11007.n1001v250c200  
homberger.r11008.n1001v250c200  
homberger.r11009.n1001v250c200  
homberger.r11010.n1001v250c200  
homberger.r20201.n201v50c1000  
homberger.r20202.n201v50c1000  
homberger.r20203.n201v50c1000  
homberger.r20204.n201v50c1000  
homberger.r20205.n201v50c1000  
homberger.r20206.n201v50c1000  
homberger.r20207.n201v50c1000  
homberger.r20208.n201v50c1000  
homberger.r20209.n201v50c1000  
homberger.r20210.n201v50c1000  
homberger.r20401.n401v100c1000  
homberger.r20402.n401v100c1000  
homberger.r20403.n401v100c1000  
homberger.r20404.n401v100c1000  
homberger.r20405.n401v100c1000  
homberger.r20406.n401v100c1000  
homberger.r20407.n401v100c1000  
homberger.r20408.n401v100c1000  
homberger.r20409.n401v100c1000  
homberger.r20410.n401v100c1000  
homberger.r20601.n601v150c1000  
homberger.r20602.n601v150c1000  
homberger.r20603.n601v150c1000  
homberger.r20604.n601v150c1000  
homberger.r20605.n601v150c1000  
homberger.r20606.n601v150c1000  
homberger.r20607.n601v150c1000  
homberger.r20608.n601v150c1000  
homberger.r20609.n601v150c1000  
homberger.r20610.n601v150c1000  
homberger.r20801.n801v200c1000  
homberger.r20802.n801v200c1000  
homberger.r20803.n801v200c1000  
homberger.r20804.n801v200c1000  
homberger.r20805.n801v200c1000  
homberger.r20806.n801v200c1000  
homberger.r20807.n801v200c1000  
homberger.r20808.n801v200c1000  
homberger.r20809.n801v200c1000  
homberger.r20810.n801v200c1000  
homberger.r21001.n1001v250c1000  
homberger.r21002.n1001v250c1000  
homberger.r21003.n1001v250c1000  
homberger.r21004.n1001v250c1000  
homberger.r21005.n1001v250c1000  
homberger.r21006.n1001v250c1000  
homberger.r21007.n1001v250c1000  
homberger.r21008.n1001v250c1000  
homberger.r21009.n1001v250c1000  
homberger.r21010.n1001v250c1000  
homberger.rc10201.n201v50c200  
homberger.rc10202.n201v50c200  
homberger.rc10203.n201v50c200  
homberger.rc10204.n201v50c200  
homberger.rc10205.n201v50c200  
homberger.rc10206.n201v50c200  
homberger.rc10207.n201v50c200  
homberger.rc10208.n201v50c200  
homberger.rc10209.n201v50c200  
homberger.rc10210.n201v50c200  
homberger.rc10401.n401v100c200  
homberger.rc10402.n401v100c200  
homberger.rc10403.n401v100c200  
homberger.rc10404.n401v100c200  
homberger.rc10405.n401v100c200  
homberger.rc10406.n401v100c200  
homberger.rc10407.n401v100c200  
homberger.rc10408.n401v100c200  
homberger.rc10409.n401v100c200  
homberger.rc10410.n401v100c200  
homberger.rc10601.n601v150c200  
homberger.rc10602.n601v150c200  
homberger.rc10603.n601v150c200  
homberger.rc10604.n601v150c200  
homberger.rc10605.n601v150c200  
homberger.rc10606.n601v150c200  
homberger.rc10607.n601v150c200  
homberger.rc10608.n601v150c200  
homberger.rc10609.n601v150c200  
homberger.rc10610.n601v150c200  
homberger.rc10801.n801v200c200  
homberger.rc10802.n801v200c200  
homberger.rc10803.n801v200c200  
homberger.rc10804.n801v200c200  
homberger.rc10805.n801v200c200  
homberger.rc10806.n801v200c200  
homberger.rc10807.n801v200c200  
homberger.rc10808.n801v200c200  
homberger.rc10809.n801v200c200  
homberger.rc10810.n801v200c200  
homberger.rc11001.n1001v250c200  
homberger.rc11002.n1001v250c200  
homberger.rc11003.n1001v250c200  
homberger.rc11004.n1001v250c200  
homberger.rc11005.n1001v250c200  
homberger.rc11006.n1001v250c200  
homberger.rc11007.n1001v250c200  
homberger.rc11008.n1001v250c200  
homberger.rc11009.n1001v250c200  
homberger.rc11010.n1001v250c200  
homberger.rc20201.n201v50c1000  
homberger.rc20202.n201v50c1000  
homberger.rc20203.n201v50c1000  
homberger.rc20204.n201v50c1000  
homberger.rc20205.n201v50c1000  
homberger.rc20206.n201v50c1000  
homberger.rc20207.n201v50c1000  
homberger.rc20208.n201v50c1000  
homberger.rc20209.n201v50c1000  
homberger.rc20210.n201v50c1000  
homberger.rc20401.n401v100c1000  
homberger.rc20402.n401v100c1000  
homberger.rc20403.n401v100c1000  
homberger.rc20404.n401v100c1000  
homberger.rc20405.n401v100c1000  
homberger.rc20406.n401v100c1000  
homberger.rc20407.n401v100c1000  
homberger.rc20408.n401v100c1000  
homberger.rc20409.n401v100c1000  
homberger.rc20410.n401v100c1000  
homberger.rc20601.n601v150c1000  
homberger.rc20602.n601v150c1000  
homberger.rc20603.n601v150c1000  
homberger.rc20604.n601v150c1000  
homberger.rc20605.n601v150c1000  
homberger.rc20606.n601v150c1000  
homberger.rc20607.n601v150c1000  
homberger.rc20608.n601v150c1000  
homberger.rc20609.n601v150c1000  
homberger.rc20610.n601v150c1000  
homberger.rc20801.n801v200c1000  
homberger.rc20802.n801v200c1000  
homberger.rc20803.n801v200c1000  
homberger.rc20804.n801v200c1000  
homberger.rc20805.n801v200c1000  
homberger.rc20806.n801v200c1000  
homberger.rc20807.n801v200c1000  
homberger.rc20808.n801v200c1000  
homberger.rc20809.n801v200c1000  
homberger.rc20810.n801v200c1000  
homberger.rc21001.n1001v250c1000  
homberger.rc21002.n1001v250c1000  
homberger.rc21003.n1001v250c1000  
homberger.rc21004.n1001v250c1000  
homberger.rc21005.n1001v250c1000  
homberger.rc21006.n1001v250c1000  
homberger.rc21007.n1001v250c1000  
homberger.rc21008.n1001v250c1000  
homberger.rc21009.n1001v250c1000  
homberger.rc21010.n1001v250c1000  
