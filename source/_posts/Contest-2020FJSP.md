---
title: SmartLab Challenge 2020 - Flexible Job Shop Scheduling
date: 2021-01-14 10:05:34
categories:
- 算法挑战
tags:
- Challenge
- 算法挑战
- 组合优化
---
柔性作业车间调度问题是智能制造与高性能计算等领域中的重要问题, 具有广泛应用场景.
柔性作业车间调度问题主要研究如何调度有限的资源依次执行多项任务, 使得完成所有任务的完工时间最短的问题.
比如芯片代工厂生产芯片时, 每块晶圆需要在不同机台依次完成光刻与蚀刻等多道工序.
比如在某些大规模并行计算场景中, 计算任务间存在依赖关系, 后继任务的输入为前驱任务的输出.
高效的柔性作业车间调度问题的求解算法具有极其重要的理论与应用价值.



# 柔性作业车间调度算法训练

## 问题概述

给定若干任务, 每个任务可由若干给定的机器中的任意一台花费给定的时间完成.
每个任务的全部前序任务完工后才能开工, 在不同机器间转移不消耗时间.
每台机器同一时间仅能执行一个任务且不可抢占, 即完成一个任务后才能开始下一个任务.
请确定每个任务何时在哪台机器上开工, 使得最后一个完工的任务最早完工.

柔性作业车间调度问题为上述任务调度问题的特例, 任务依赖拓扑为若干条单链.
即给定若干工件, 每个工件由一系列必须依次完成的工序组成.

- 参考文献.
  - [1] J. Ding, Z. Lü, C. M. Li, L. Shen, L. Xu, and F. Glover, “A two-individual based evolutionary algorithm for the flexible job shop scheduling problem,” in Proceedings of the AAAI Conference on Artificial Intelligence, Jul. 2019, vol. 33, pp. 2262–2271. doi: 10.1609/aaai.v33i01.33012262.
  - [2] C. Zhang, P. Li, Z. Guan, and Y. Rao, “A tabu search algorithm with a new neighborhood structure for the job shop scheduling problem,” Computers & Operations Research, vol. 34, no. 11, pp. 3229–3242, 2007, doi: 10.1016/j.cor.2005.12.002.
  - [3] M. A. González, C. R. Vela, and R. Varela, “Scatter search with path relinking for the flexible job shop scheduling problem,” European Journal of Operational Research, vol. 245, no. 1, Art. no. 1, Aug. 2015, doi: 10.1016/j.ejor.2015.02.052.


## 命令行参数

请大家编写程序时支持两个命令行参数, 依次为运行时间上限 (单位为秒) 和随机种子 (0-65535).
算例文件已重定向至标准输入 `stdin`/`cin`, 标准输出 `stdout`/`cout` 已重定向至解文件 (如需打印调试信息, 请使用标准错误输出 `stderr`/`cerr`).
例如, 在控制台运行以下命令表示调用可执行文件 `fjsp.exe` 在限时 300 秒, 随机种子为 12345 的情况下求解路径为 `../data/jsp.FT06.m6j6c1.txt` 的算例, 解文件输出至 `sln.jsp.FT06.m6j6c1.txt`:
```
fjsp.exe 300 123456 <../data/jsp.FT06.m6j6c1.txt >sln.jsp.FT06.m6j6c1.txt
```

- 运行时间上限.
  - 超出运行时间上限后测试程序会强行终止算法, 请确保在此之前已输出解 (最好还能自行正常退出).
- 随机种子设置.
  - 使用 C 语言随机数生成器请用 `srand`.
  - 使用 C++ 随机数生成器 (如 `mt19937`) 请在构造时传参或调用 `seed()` 方法设置.


## 输入的算例文件格式

所有算例的任务和机器分别从 0 开始连续编号.

第一行给出 3 个由空白字符分隔的整数, 分别表示任务数 N, 机器数 M, 以及所有任务所有工序的最大的候选机器数 C (若 C = 1 说明该算例为 JSP 算例).
接下来连续 N 行, 第 i 行表示第 i 个任务的信息.
每行第一个整数表示第 i 个任务的工序数 K, 随后连续 K 组由空白字符分隔的数据, 第 j 组数据表示该任务的第 j 道工序的信息.
每组数据第一个整数表示可执行该工序的候选机器数 S, 随后连续 S 个由空白字符分隔的二元组, 二元组 (G, D) 表示该工序可由机器 G 执行且加工时长为 D.

例如, 以下算例文件表示有 2 个任务和 4 台机器, 每个任务至多 2 台候选机器; 其中,  
任务 0 由 4 道工序构成,  
  其工序 0 可由机器 1 花费时长 654 完成,  
  其工序 1 可由机器 2 花费时长 147 完成,  
  其工序 2 可由机器 3 花费时长 345 完成,  
  其工序 3 可由机器 0 花费时长 447 完成;  
任务 1 由 3 道工序构成,  
  其工序 0 可由机器 1 花费时长 321 完成或由机器 0 花费时长 321 完成,  
  其工序 1 可由机器 2 花费时长 520 完成,  
  其工序 2 可由机器 3 花费时长 789 完成:
```
2 4 2
4    1  1 654    1  2 147   1  3 345    1  0 447
3    2  1 321  0 321    1  2 520    1  3 789
```

上述算例内数据的分组情况可以更加直观地表示成下面的情况:
```
2 4 2
4 [1 (1 654)] [1 (2 147)] [1 (3 345)] [1 (0 447)]
3 [2 (1 321) (0 321)] [1 (2 520)] [1 (3 789)]
```

为了使算例的数据分组更加直观, 给出的算例可能会按照如下约定: 使用 4 个空格分隔每行的 K 组数据, 使用 2 个空格分隔 S 个二元组.


## 输出的解文件格式

输出 M 行整数表示 M 台机器的任务分配与排序情况, 第 i 行表示第 i 台机器上执行的工序的有序列表.
每一行第一个整数表示第 i 台机器加工的工序数 E, 随后连续 E 个由空白字符分隔的二元组, 二元组 (J, O) 表示加工了任务 J 的工序 O, 二元组的出现顺序表示第 i 台机器的加工顺序.

例如, 以下解文件表示 2 台机器上的任务分配与排序情况; 其中,  
机器 0 执行了 3 道工序, 依次为任务 1 的工序 1, 任务 0 的工序 0, 任务 2 的工序 0;  
机器 1 加工了 2 道工序, 依次为任务 0 的工序 1, 任务 1 的工序 0:
```
3    1 1  0 0  2 0
2    0 1  1 0

```

注意上述调度方案存在死锁, 不是可行的调度方案.


## 提交要求

- 发送至邮箱 [szx@duhe.tech](mailto:szx@duhe.tech).
- 邮件标题格式为 "**Challenge2020FJSP-姓名-学校-专业**".
- 邮件附件为单个压缩包, 文件名为 "**姓名-学校-专业**", 其内包含下列文件.
  - **必要** 算法的可执行文件 (Windows 平台).
    - 建议基于官方 SDK 开发 ([https://gitee.com/suzhouxing/npbenchmark/tree/main/SDK.FJSP](https://gitee.com/suzhouxing/npbenchmark/tree/main/SDK.FJSP)).
    - 用 g++ 的同学编译时请静态链接, 即添加 `-static-libgcc -static-libstdc++` 编译选项.
  - **必要** 算法源码.
    - 可重入 (可在同一线程内反复调用而不会出现数据初始化错误或内存泄漏).
    - 可并发 (可在同一进程内的多个线程同时运行多个算法求解实例而互不干扰, 满足此要求一般不能有全局的非只读变量).
    - 可伸缩 (数据结构可以根据算例规模动态申请内存, 而非根据预先指定的编译期常量进行内存分配).
  - **可选** 算法在各算例上的运行情况概要, 至少包括以下几项信息 (可选, 仅在无法成功调用算法输出可通过检查程序的解时作为参考).
    - 算例名.
    - 所有任务的完工时间.
    - 计算耗时.
  - **可选** 算法在各算例上求得的完工时间最短的解文件 (可选, 仅在无法成功调用算法输出可通过检查程序的解时作为参考).
- 若成功提交, 在收到邮件时以及测试完成后系统均会自动发送邮件反馈提交情况.
  - 若测试结果较优, 可在排行榜页面看到自己的运行情况 ([https://gitee.com/suzhouxing/npbenchmark.data](https://gitee.com/suzhouxing/npbenchmark.data)).

例如:
```
苏宙行-华科-计科.zip
|   fjsp.exe
|   results.csv
|
+---src
|       main.cpp
|       algorithm.cpp
|       algorithm.h
|
+---results
        fjsp.barnes.mt10c1.m11j10c2.txt
        fjsp.barnes.mt10cc.m12j10c2.txt
        ...
```


## 算例清单

下载地址: [https://gitee.com/suzhouxing/npbenchmark.data/FJSP/Instance](https://gitee.com/suzhouxing/npbenchmark.data/FJSP/Instance)

fjsp.barnes.mt10c1.m11j10c2  
fjsp.barnes.mt10cc.m12j10c2  
fjsp.barnes.mt10x.m11j10c2  
fjsp.barnes.mt10xx.m12j10c3  
fjsp.barnes.mt10xxx.m13j10c4  
fjsp.barnes.mt10xy.m12j10c2  
fjsp.barnes.mt10xyz.m13j10c2  
fjsp.barnes.setb4c9.m11j15c2  
fjsp.barnes.setb4cc.m12j15c2  
fjsp.barnes.setb4x.m11j15c2  
fjsp.barnes.setb4xx.m12j15c3  
fjsp.barnes.setb4xxx.m13j15c4  
fjsp.barnes.setb4xy.m12j15c2  
fjsp.barnes.setb4xyz.m13j15c2  
fjsp.barnes.seti5c12.m16j15c2  
fjsp.barnes.seti5cc.m17j15c2  
fjsp.barnes.seti5x.m16j15c2  
fjsp.barnes.seti5xx.m17j15c3  
fjsp.barnes.seti5xxx.m18j15c4  
fjsp.barnes.seti5xy.m17j15c2  
fjsp.barnes.seti5xyz.m18j15c2  
fjsp.brandimarte.Mk01.m6j10c3  
fjsp.brandimarte.Mk02.m6j10c6  
fjsp.brandimarte.Mk03.m8j15c5  
fjsp.brandimarte.Mk04.m8j15c3  
fjsp.brandimarte.Mk05.m4j15c2  
fjsp.brandimarte.Mk06.m15j10c5  
fjsp.brandimarte.Mk07.m5j20c5  
fjsp.brandimarte.Mk08.m10j20c2  
fjsp.brandimarte.Mk09.m10j20c5  
fjsp.brandimarte.Mk10.m15j20c5  
fjsp.dauzere.01a.m5j10c3  
fjsp.dauzere.02a.m5j10c4  
fjsp.dauzere.03a.m5j10c5  
fjsp.dauzere.04a.m5j10c3  
fjsp.dauzere.05a.m5j10c4  
fjsp.dauzere.06a.m5j10c5  
fjsp.dauzere.07a.m8j15c4  
fjsp.dauzere.08a.m8j15c6  
fjsp.dauzere.09a.m8j15c8  
fjsp.dauzere.10a.m8j15c4  
fjsp.dauzere.11a.m8j15c6  
fjsp.dauzere.12a.m8j15c8  
fjsp.dauzere.13a.m10j20c4  
fjsp.dauzere.14a.m10j20c7  
fjsp.dauzere.15a.m10j20c10  
fjsp.dauzere.16a.m10j20c4  
fjsp.dauzere.17a.m10j20c7  
fjsp.dauzere.18a.m10j20c10  
fjsp.hurink.edata-abz5.m10j10c2  
fjsp.hurink.edata-abz6.m10j10c2  
fjsp.hurink.edata-abz7.m15j20c3  
fjsp.hurink.edata-abz8.m15j20c3  
fjsp.hurink.edata-abz9.m15j20c3  
fjsp.hurink.edata-car1.m5j11c2  
fjsp.hurink.edata-car2.m4j13c2  
fjsp.hurink.edata-car3.m5j12c2  
fjsp.hurink.edata-car4.m4j14c2  
fjsp.hurink.edata-car5.m6j10c2  
fjsp.hurink.edata-car6.m9j8c2  
fjsp.hurink.edata-car7.m7j7c2  
fjsp.hurink.edata-car8.m8j8c2  
fjsp.hurink.edata-la01.m5j10c2  
fjsp.hurink.edata-la02.m5j10c2  
fjsp.hurink.edata-la03.m5j10c2  
fjsp.hurink.edata-la04.m5j10c2  
fjsp.hurink.edata-la05.m5j10c2  
fjsp.hurink.edata-la06.m5j15c2  
fjsp.hurink.edata-la07.m5j15c2  
fjsp.hurink.edata-la08.m5j15c2  
fjsp.hurink.edata-la09.m5j15c2  
fjsp.hurink.edata-la10.m5j15c2  
fjsp.hurink.edata-la11.m5j20c2  
fjsp.hurink.edata-la12.m5j20c2  
fjsp.hurink.edata-la13.m5j20c2  
fjsp.hurink.edata-la14.m5j20c2  
fjsp.hurink.edata-la15.m5j20c2  
fjsp.hurink.edata-la16.m10j10c2  
fjsp.hurink.edata-la17.m10j10c2  
fjsp.hurink.edata-la18.m10j10c2  
fjsp.hurink.edata-la19.m10j10c2  
fjsp.hurink.edata-la20.m10j10c2  
fjsp.hurink.edata-la21.m10j15c3  
fjsp.hurink.edata-la22.m10j15c3  
fjsp.hurink.edata-la23.m10j15c2  
fjsp.hurink.edata-la24.m10j15c3  
fjsp.hurink.edata-la25.m10j15c3  
fjsp.hurink.edata-la26.m10j20c3  
fjsp.hurink.edata-la27.m10j20c3  
fjsp.hurink.edata-la28.m10j20c3  
fjsp.hurink.edata-la29.m10j20c3  
fjsp.hurink.edata-la30.m10j20c3  
fjsp.hurink.edata-la31.m10j30c3  
fjsp.hurink.edata-la32.m10j30c3  
fjsp.hurink.edata-la33.m10j30c2  
fjsp.hurink.edata-la34.m10j30c3  
fjsp.hurink.edata-la35.m10j30c2  
fjsp.hurink.edata-la36.m15j15c3  
fjsp.hurink.edata-la37.m15j15c3  
fjsp.hurink.edata-la38.m15j15c3  
fjsp.hurink.edata-la39.m15j15c3  
fjsp.hurink.edata-la40.m15j15c3  
fjsp.hurink.edata-mt06.m6j6c2  
fjsp.hurink.edata-mt10.m10j10c2  
fjsp.hurink.edata-mt20.m5j20c2  
fjsp.hurink.edata-orb01.m10j10c2  
fjsp.hurink.edata-orb02.m10j10c2  
fjsp.hurink.edata-orb03.m10j10c2  
fjsp.hurink.edata-orb04.m10j10c2  
fjsp.hurink.edata-orb05.m10j10c2  
fjsp.hurink.edata-orb06.m10j10c2  
fjsp.hurink.edata-orb07.m10j10c2  
fjsp.hurink.edata-orb08.m10j10c2  
fjsp.hurink.edata-orb09.m10j10c2  
fjsp.hurink.edata-orb10.m10j10c2  
fjsp.hurink.rdata-abz5.m10j10c3  
fjsp.hurink.rdata-abz6.m10j10c3  
fjsp.hurink.rdata-abz7.m15j20c3  
fjsp.hurink.rdata-abz8.m15j20c3  
fjsp.hurink.rdata-abz9.m15j20c3  
fjsp.hurink.rdata-car1.m5j11c3  
fjsp.hurink.rdata-car2.m4j13c3  
fjsp.hurink.rdata-car3.m5j12c3  
fjsp.hurink.rdata-car4.m4j14c3  
fjsp.hurink.rdata-car5.m6j10c3  
fjsp.hurink.rdata-car6.m9j8c3  
fjsp.hurink.rdata-car7.m7j7c3  
fjsp.hurink.rdata-car8.m8j8c3  
fjsp.hurink.rdata-la01.m5j10c3  
fjsp.hurink.rdata-la02.m5j10c3  
fjsp.hurink.rdata-la03.m5j10c3  
fjsp.hurink.rdata-la04.m5j10c3  
fjsp.hurink.rdata-la05.m5j10c3  
fjsp.hurink.rdata-la06.m5j15c3  
fjsp.hurink.rdata-la07.m5j15c3  
fjsp.hurink.rdata-la08.m5j15c3  
fjsp.hurink.rdata-la09.m5j15c3  
fjsp.hurink.rdata-la10.m5j15c3  
fjsp.hurink.rdata-la11.m5j20c3  
fjsp.hurink.rdata-la12.m5j20c3  
fjsp.hurink.rdata-la13.m5j20c3  
fjsp.hurink.rdata-la14.m5j20c3  
fjsp.hurink.rdata-la15.m5j20c3  
fjsp.hurink.rdata-la16.m10j10c3  
fjsp.hurink.rdata-la17.m10j10c3  
fjsp.hurink.rdata-la18.m10j10c3  
fjsp.hurink.rdata-la19.m10j10c3  
fjsp.hurink.rdata-la20.m10j10c3  
fjsp.hurink.rdata-la21.m10j15c3  
fjsp.hurink.rdata-la22.m10j15c3  
fjsp.hurink.rdata-la23.m10j15c3  
fjsp.hurink.rdata-la24.m10j15c3  
fjsp.hurink.rdata-la25.m10j15c3  
fjsp.hurink.rdata-la26.m10j20c3  
fjsp.hurink.rdata-la27.m10j20c3  
fjsp.hurink.rdata-la28.m10j20c3  
fjsp.hurink.rdata-la29.m10j20c3  
fjsp.hurink.rdata-la30.m10j20c3  
fjsp.hurink.rdata-la31.m10j30c3  
fjsp.hurink.rdata-la32.m10j30c3  
fjsp.hurink.rdata-la33.m10j30c3  
fjsp.hurink.rdata-la34.m10j30c3  
fjsp.hurink.rdata-la35.m10j30c3  
fjsp.hurink.rdata-la36.m15j15c3  
fjsp.hurink.rdata-la37.m15j15c3  
fjsp.hurink.rdata-la38.m15j15c3  
fjsp.hurink.rdata-la39.m15j15c3  
fjsp.hurink.rdata-la40.m15j15c3  
fjsp.hurink.rdata-mt06.m6j6c3  
fjsp.hurink.rdata-mt10.m10j10c3  
fjsp.hurink.rdata-mt20.m5j20c3  
fjsp.hurink.rdata-orb01.m10j10c3  
fjsp.hurink.rdata-orb02.m10j10c3  
fjsp.hurink.rdata-orb03.m10j10c3  
fjsp.hurink.rdata-orb04.m10j10c3  
fjsp.hurink.rdata-orb05.m10j10c3  
fjsp.hurink.rdata-orb06.m10j10c3  
fjsp.hurink.rdata-orb07.m10j10c3  
fjsp.hurink.rdata-orb08.m10j10c3  
fjsp.hurink.rdata-orb09.m10j10c3  
fjsp.hurink.rdata-orb10.m10j10c3  
##fjsp.hurink.sdata-abz5.m10j10c1 <=> jsp.ABZ05.m10j10c1  
##fjsp.hurink.sdata-abz6.m10j10c1 <=> jsp.ABZ06.m10j10c1  
##fjsp.hurink.sdata-abz7.m15j20c1 <=> jsp.ABZ07.m15j20c1  
##fjsp.hurink.sdata-abz8.m15j20c1 <=> jsp.ABZ08.m15j20c1  
##fjsp.hurink.sdata-abz9.m15j20c1 <=> jsp.ABZ09.m15j20c1  
fjsp.hurink.sdata-car1.m5j11c1  
fjsp.hurink.sdata-car2.m4j13c1  
fjsp.hurink.sdata-car3.m5j12c1  
fjsp.hurink.sdata-car4.m4j14c1  
fjsp.hurink.sdata-car5.m6j10c1  
fjsp.hurink.sdata-car6.m9j8c1  
fjsp.hurink.sdata-car7.m7j7c1  
fjsp.hurink.sdata-car8.m8j8c1  
##fjsp.hurink.sdata-la01.m5j10c1  <=> jsp.LA01.m5j10c1  
##fjsp.hurink.sdata-la02.m5j10c1  <=> jsp.LA02.m5j10c1  
##fjsp.hurink.sdata-la03.m5j10c1  <=> jsp.LA03.m5j10c1  
##fjsp.hurink.sdata-la04.m5j10c1  <=> jsp.LA04.m5j10c1  
##fjsp.hurink.sdata-la05.m5j10c1  <=> jsp.LA05.m5j10c1  
##fjsp.hurink.sdata-la06.m5j15c1  <=> jsp.LA06.m5j15c1  
##fjsp.hurink.sdata-la07.m5j15c1  <=> jsp.LA07.m5j15c1  
##fjsp.hurink.sdata-la08.m5j15c1  <=> jsp.LA08.m5j15c1  
##fjsp.hurink.sdata-la09.m5j15c1  <=> jsp.LA09.m5j15c1  
##fjsp.hurink.sdata-la10.m5j15c1  <=> jsp.LA10.m5j15c1  
##fjsp.hurink.sdata-la11.m5j20c1  <=> jsp.LA11.m5j20c1  
##fjsp.hurink.sdata-la12.m5j20c1  <=> jsp.LA12.m5j20c1  
##fjsp.hurink.sdata-la13.m5j20c1  <=> jsp.LA13.m5j20c1  
##fjsp.hurink.sdata-la14.m5j20c1  <=> jsp.LA14.m5j20c1  
##fjsp.hurink.sdata-la15.m5j20c1  <=> jsp.LA15.m5j20c1  
##fjsp.hurink.sdata-la16.m10j10c1 <=> jsp.LA16.m10j10c1  
##fjsp.hurink.sdata-la17.m10j10c1 <=> jsp.LA17.m10j10c1  
##fjsp.hurink.sdata-la18.m10j10c1 <=> jsp.LA18.m10j10c1  
##fjsp.hurink.sdata-la19.m10j10c1 <=> jsp.LA19.m10j10c1  
##fjsp.hurink.sdata-la20.m10j10c1 <=> jsp.LA20.m10j10c1  
##fjsp.hurink.sdata-la21.m10j15c1 <=> jsp.LA21.m10j15c1  
##fjsp.hurink.sdata-la22.m10j15c1 <=> jsp.LA22.m10j15c1  
##fjsp.hurink.sdata-la23.m10j15c1 <=> jsp.LA23.m10j15c1  
##fjsp.hurink.sdata-la24.m10j15c1 <=> jsp.LA24.m10j15c1  
##fjsp.hurink.sdata-la25.m10j15c1 <=> jsp.LA25.m10j15c1  
##fjsp.hurink.sdata-la26.m10j20c1 <=> jsp.LA26.m10j20c1  
##fjsp.hurink.sdata-la27.m10j20c1 <=> jsp.LA27.m10j20c1  
##fjsp.hurink.sdata-la28.m10j20c1 <=> jsp.LA28.m10j20c1  
##fjsp.hurink.sdata-la29.m10j20c1 <=> jsp.LA29.m10j20c1  
##fjsp.hurink.sdata-la30.m10j20c1 <=> jsp.LA30.m10j20c1  
##fjsp.hurink.sdata-la31.m10j30c1 <=> jsp.LA31.m10j30c1  
##fjsp.hurink.sdata-la32.m10j30c1 <=> jsp.LA32.m10j30c1  
##fjsp.hurink.sdata-la33.m10j30c1 <=> jsp.LA33.m10j30c1  
##fjsp.hurink.sdata-la34.m10j30c1 <=> jsp.LA34.m10j30c1  
##fjsp.hurink.sdata-la35.m10j30c1 <=> jsp.LA35.m10j30c1  
##fjsp.hurink.sdata-la36.m15j15c1 <=> jsp.LA36.m15j15c1  
##fjsp.hurink.sdata-la37.m15j15c1 <=> jsp.LA37.m15j15c1  
##fjsp.hurink.sdata-la38.m15j15c1 <=> jsp.LA38.m15j15c1  
##fjsp.hurink.sdata-la39.m15j15c1 <=> jsp.LA39.m15j15c1  
##fjsp.hurink.sdata-la40.m15j15c1 <=> jsp.LA40.m15j15c1  
##fjsp.hurink.sdata-mt06.m6j6c1   <=> jsp.FT06.m6j6c1  
##fjsp.hurink.sdata-mt10.m10j10c1 <=> jsp.FT10.m10j10c1  
##fjsp.hurink.sdata-mt20.m5j20c1  <=> jsp.FT20.m5j20c1  
##fjsp.hurink.sdata-orb01.m10j10c1  <=> jsp.ORB01.m10j10c1  
##fjsp.hurink.sdata-orb02.m10j10c1  <=> jsp.ORB02.m10j10c1  
##fjsp.hurink.sdata-orb03.m10j10c1  <=> jsp.ORB03.m10j10c1  
##fjsp.hurink.sdata-orb04.m10j10c1  <=> jsp.ORB04.m10j10c1  
##fjsp.hurink.sdata-orb05.m10j10c1  <=> jsp.ORB05.m10j10c1  
##fjsp.hurink.sdata-orb06.m10j10c1  <=> jsp.ORB06.m10j10c1  
##fjsp.hurink.sdata-orb07.m10j10c1  <=> jsp.ORB07.m10j10c1  
##fjsp.hurink.sdata-orb08.m10j10c1  <=> jsp.ORB08.m10j10c1  
##fjsp.hurink.sdata-orb09.m10j10c1  <=> jsp.ORB09.m10j10c1  
##fjsp.hurink.sdata-orb10.m10j10c1 <=> jsp.ORB10.m10j10c1  
fjsp.hurink.vdata-abz5.m10j10c9  
fjsp.hurink.vdata-abz6.m10j10c8  
fjsp.hurink.vdata-abz7.m15j20c11  
fjsp.hurink.vdata-abz8.m15j20c13  
fjsp.hurink.vdata-abz9.m15j20c12  
fjsp.hurink.vdata-car1.m5j11c5  
fjsp.hurink.vdata-car2.m4j13c4  
fjsp.hurink.vdata-car3.m5j12c4  
fjsp.hurink.vdata-car4.m4j14c3  
fjsp.hurink.vdata-car5.m6j10c5  
fjsp.hurink.vdata-car6.m9j8c7  
fjsp.hurink.vdata-car7.m7j7c6  
fjsp.hurink.vdata-car8.m8j8c7  
fjsp.hurink.vdata-la01.m5j10c5  
fjsp.hurink.vdata-la02.m5j10c5  
fjsp.hurink.vdata-la03.m5j10c5  
fjsp.hurink.vdata-la04.m5j10c4  
fjsp.hurink.vdata-la05.m5j10c5  
fjsp.hurink.vdata-la06.m5j15c5  
fjsp.hurink.vdata-la07.m5j15c5  
fjsp.hurink.vdata-la08.m5j15c5  
fjsp.hurink.vdata-la09.m5j15c5  
fjsp.hurink.vdata-la10.m5j15c5  
fjsp.hurink.vdata-la11.m5j20c5  
fjsp.hurink.vdata-la12.m5j20c5  
fjsp.hurink.vdata-la13.m5j20c4  
fjsp.hurink.vdata-la14.m5j20c5  
fjsp.hurink.vdata-la15.m5j20c5  
fjsp.hurink.vdata-la16.m10j10c8  
fjsp.hurink.vdata-la17.m10j10c9  
fjsp.hurink.vdata-la18.m10j10c9  
fjsp.hurink.vdata-la19.m10j10c8  
fjsp.hurink.vdata-la20.m10j10c8  
fjsp.hurink.vdata-la21.m10j15c8  
fjsp.hurink.vdata-la22.m10j15c8  
fjsp.hurink.vdata-la23.m10j15c8  
fjsp.hurink.vdata-la24.m10j15c8  
fjsp.hurink.vdata-la25.m10j15c9  
fjsp.hurink.vdata-la26.m10j20c9  
fjsp.hurink.vdata-la27.m10j20c9  
fjsp.hurink.vdata-la28.m10j20c9  
fjsp.hurink.vdata-la29.m10j20c9  
fjsp.hurink.vdata-la30.m10j20c9  
fjsp.hurink.vdata-la31.m10j30c9  
fjsp.hurink.vdata-la32.m10j30c9  
fjsp.hurink.vdata-la33.m10j30c8  
fjsp.hurink.vdata-la34.m10j30c9  
fjsp.hurink.vdata-la35.m10j30c9  
fjsp.hurink.vdata-la36.m15j15c12  
fjsp.hurink.vdata-la37.m15j15c12  
fjsp.hurink.vdata-la38.m15j15c12  
fjsp.hurink.vdata-la39.m15j15c11  
fjsp.hurink.vdata-la40.m15j15c11  
fjsp.hurink.vdata-mt06.m6j6c5  
fjsp.hurink.vdata-mt10.m10j10c8  
fjsp.hurink.vdata-mt20.m5j20c5  
fjsp.hurink.vdata-orb01.m10j10c8  
fjsp.hurink.vdata-orb02.m10j10c8  
fjsp.hurink.vdata-orb03.m10j10c8  
fjsp.hurink.vdata-orb04.m10j10c9  
fjsp.hurink.vdata-orb05.m10j10c8  
fjsp.hurink.vdata-orb06.m10j10c8  
fjsp.hurink.vdata-orb07.m10j10c8  
fjsp.hurink.vdata-orb08.m10j10c8  
fjsp.hurink.vdata-orb09.m10j10c8  
fjsp.hurink.vdata-orb10.m10j10c8  
jsp.ABZ05.m10j10c1  
jsp.ABZ06.m10j10c1  
jsp.ABZ07.m15j20c1  
jsp.ABZ08.m15j20c1  
jsp.ABZ09.m15j20c1  
jsp.DMU01.m15j20c1  
jsp.DMU02.m15j20c1  
jsp.DMU03.m15j20c1  
jsp.DMU04.m15j20c1  
jsp.DMU05.m15j20c1  
jsp.DMU06.m20j20c1  
jsp.DMU07.m20j20c1  
jsp.DMU08.m20j20c1  
jsp.DMU09.m20j20c1  
jsp.DMU10.m20j20c1  
jsp.DMU11.m15j30c1  
jsp.DMU12.m15j30c1  
jsp.DMU13.m15j30c1  
jsp.DMU14.m15j30c1  
jsp.DMU15.m15j30c1  
jsp.DMU16.m20j30c1  
jsp.DMU17.m20j30c1  
jsp.DMU18.m20j30c1  
jsp.DMU19.m20j30c1  
jsp.DMU20.m20j30c1  
jsp.DMU21.m15j40c1  
jsp.DMU22.m15j40c1  
jsp.DMU23.m15j40c1  
jsp.DMU24.m15j40c1  
jsp.DMU25.m15j40c1  
jsp.DMU26.m20j40c1  
jsp.DMU27.m20j40c1  
jsp.DMU28.m20j40c1  
jsp.DMU29.m20j40c1  
jsp.DMU30.m20j40c1  
jsp.DMU31.m15j50c1  
jsp.DMU32.m15j50c1  
jsp.DMU33.m15j50c1  
jsp.DMU34.m15j50c1  
jsp.DMU35.m15j50c1  
jsp.DMU36.m20j50c1  
jsp.DMU37.m20j50c1  
jsp.DMU38.m20j50c1  
jsp.DMU39.m20j50c1  
jsp.DMU40.m20j50c1  
jsp.DMU41.m15j20c1  
jsp.DMU42.m15j20c1  
jsp.DMU43.m15j20c1  
jsp.DMU44.m15j20c1  
jsp.DMU45.m15j20c1  
jsp.DMU46.m20j20c1  
jsp.DMU47.m20j20c1  
jsp.DMU48.m20j20c1  
jsp.DMU49.m20j20c1  
jsp.DMU50.m20j20c1  
jsp.DMU51.m15j30c1  
jsp.DMU52.m15j30c1  
jsp.DMU53.m15j30c1  
jsp.DMU54.m15j30c1  
jsp.DMU55.m15j30c1  
jsp.DMU56.m20j30c1  
jsp.DMU57.m20j30c1  
jsp.DMU58.m20j30c1  
jsp.DMU59.m20j30c1  
jsp.DMU60.m20j30c1  
jsp.DMU61.m15j40c1  
jsp.DMU62.m15j40c1  
jsp.DMU63.m15j40c1  
jsp.DMU64.m15j40c1  
jsp.DMU65.m15j40c1  
jsp.DMU66.m20j40c1  
jsp.DMU67.m20j40c1  
jsp.DMU68.m20j40c1  
jsp.DMU69.m20j40c1  
jsp.DMU70.m20j40c1  
jsp.DMU71.m15j50c1  
jsp.DMU72.m15j50c1  
jsp.DMU73.m15j50c1  
jsp.DMU74.m15j50c1  
jsp.DMU75.m15j50c1  
jsp.DMU76.m20j50c1  
jsp.DMU77.m20j50c1  
jsp.DMU78.m20j50c1  
jsp.DMU79.m20j50c1  
jsp.DMU80.m20j50c1  
jsp.FT06.m6j6c1  
jsp.FT10.m10j10c1  
jsp.FT20.m5j20c1  
jsp.LA01.m5j10c1  
jsp.LA02.m5j10c1  
jsp.LA03.m5j10c1  
jsp.LA04.m5j10c1  
jsp.LA05.m5j10c1  
jsp.LA06.m5j15c1  
jsp.LA07.m5j15c1  
jsp.LA08.m5j15c1  
jsp.LA09.m5j15c1  
jsp.LA10.m5j15c1  
jsp.LA11.m5j20c1  
jsp.LA12.m5j20c1  
jsp.LA13.m5j20c1  
jsp.LA14.m5j20c1  
jsp.LA15.m5j20c1  
jsp.LA16.m10j10c1  
jsp.LA17.m10j10c1  
jsp.LA18.m10j10c1  
jsp.LA19.m10j10c1  
jsp.LA20.m10j10c1  
jsp.LA21.m10j15c1  
jsp.LA22.m10j15c1  
jsp.LA23.m10j15c1  
jsp.LA24.m10j15c1  
jsp.LA25.m10j15c1  
jsp.LA26.m10j20c1  
jsp.LA27.m10j20c1  
jsp.LA28.m10j20c1  
jsp.LA29.m10j20c1  
jsp.LA30.m10j20c1  
jsp.LA31.m10j30c1  
jsp.LA32.m10j30c1  
jsp.LA33.m10j30c1  
jsp.LA34.m10j30c1  
jsp.LA35.m10j30c1  
jsp.LA36.m15j15c1  
jsp.LA37.m15j15c1  
jsp.LA38.m15j15c1  
jsp.LA39.m15j15c1  
jsp.LA40.m15j15c1  
jsp.ORB01.m10j10c1  
jsp.ORB02.m10j10c1  
jsp.ORB03.m10j10c1  
jsp.ORB04.m10j10c1  
jsp.ORB05.m10j10c1  
jsp.ORB06.m10j10c1  
jsp.ORB07.m10j10c1  
jsp.ORB08.m10j10c1  
jsp.ORB09.m10j10c1  
jsp.ORB10.m10j10c1  
jsp.SWV01.m10j20c1  
jsp.SWV02.m10j20c1  
jsp.SWV03.m10j20c1  
jsp.SWV04.m10j20c1  
jsp.SWV05.m10j20c1  
jsp.SWV06.m15j20c1  
jsp.SWV07.m15j20c1  
jsp.SWV08.m15j20c1  
jsp.SWV09.m15j20c1  
jsp.SWV10.m15j20c1  
jsp.SWV11.m10j50c1  
jsp.SWV12.m10j50c1  
jsp.SWV13.m10j50c1  
jsp.SWV14.m10j50c1  
jsp.SWV15.m10j50c1  
jsp.SWV16.m10j50c1  
jsp.SWV17.m10j50c1  
jsp.SWV18.m10j50c1  
jsp.SWV19.m10j50c1  
jsp.SWV20.m10j50c1  
jsp.TA01.m15j15c1  
jsp.TA02.m15j15c1  
jsp.TA03.m15j15c1  
jsp.TA04.m15j15c1  
jsp.TA05.m15j15c1  
jsp.TA06.m15j15c1  
jsp.TA07.m15j15c1  
jsp.TA08.m15j15c1  
jsp.TA09.m15j15c1  
jsp.TA10.m15j15c1  
jsp.TA11.m15j20c1  
jsp.TA12.m15j20c1  
jsp.TA13.m15j20c1  
jsp.TA14.m15j20c1  
jsp.TA15.m15j20c1  
jsp.TA16.m15j20c1  
jsp.TA17.m15j20c1  
jsp.TA18.m15j20c1  
jsp.TA19.m15j20c1  
jsp.TA20.m15j20c1  
jsp.TA21.m20j20c1  
jsp.TA22.m20j20c1  
jsp.TA23.m20j20c1  
jsp.TA24.m20j20c1  
jsp.TA25.m20j20c1  
jsp.TA26.m20j20c1  
jsp.TA27.m20j20c1  
jsp.TA28.m20j20c1  
jsp.TA29.m20j20c1  
jsp.TA30.m20j20c1  
jsp.TA31.m15j30c1  
jsp.TA32.m15j30c1  
jsp.TA33.m15j30c1  
jsp.TA34.m15j30c1  
jsp.TA35.m15j30c1  
jsp.TA36.m15j30c1  
jsp.TA37.m15j30c1  
jsp.TA38.m15j30c1  
jsp.TA39.m15j30c1  
jsp.TA40.m15j30c1  
jsp.TA41.m20j30c1  
jsp.TA42.m20j30c1  
jsp.TA43.m20j30c1  
jsp.TA44.m20j30c1  
jsp.TA45.m20j30c1  
jsp.TA46.m20j30c1  
jsp.TA47.m20j30c1  
jsp.TA48.m20j30c1  
jsp.TA49.m20j30c1  
jsp.TA50.m20j30c1  
jsp.TA51.m15j50c1  
jsp.TA52.m15j50c1  
jsp.TA53.m15j50c1  
jsp.TA54.m15j50c1  
jsp.TA55.m15j50c1  
jsp.TA56.m15j50c1  
jsp.TA57.m15j50c1  
jsp.TA58.m15j50c1  
jsp.TA59.m15j50c1  
jsp.TA60.m15j50c1  
jsp.TA61.m20j50c1  
jsp.TA62.m20j50c1  
jsp.TA63.m20j50c1  
jsp.TA64.m20j50c1  
jsp.TA65.m20j50c1  
jsp.TA66.m20j50c1  
jsp.TA67.m20j50c1  
jsp.TA68.m20j50c1  
jsp.TA69.m20j50c1  
jsp.TA70.m20j50c1  
jsp.TA71.m20j100c1  
jsp.TA72.m20j100c1  
jsp.TA73.m20j100c1  
jsp.TA74.m20j100c1  
jsp.TA75.m20j100c1  
jsp.TA76.m20j100c1  
jsp.TA77.m20j100c1  
jsp.TA78.m20j100c1  
jsp.TA79.m20j100c1  
jsp.TA80.m20j100c1  
jsp.YN01.m20j20c1  
jsp.YN02.m20j20c1  
jsp.YN03.m20j20c1  
jsp.YN04.m20j20c1  
