---
title: SmartLab Challenge 2022 - Minimum Connected Dominating Sets
date: 2022-04-23 15:41:35
categories:
- 算法挑战
tags:
- Challenge
- 算法挑战
- 组合优化
---
最小连通支配集问题在众多领域中有广泛的应用, 可以用来对多种组合优化问题进行建模.
最小连通支配集问题主要研究如何挑选一系列紧密相连的节点为其他节点提供服务的问题.
例如, 通信网络, 能源传输, 物流调拨等众多行业中的选址都可以用最小连通支配集问题进行建模.
高效的最小连通支配集问题求解算法具有极其重要的理论与应用价值.



# 最小连通支配集算法训练

## 问题概述

给定一个无向图, 请挑选一个节点数最少的连通分量, 使所有节点均与该连通分量中的至少一个节点相邻.

- 参考文献.
  - [1] X. Wu, Z. Lü, and F. Glover, “A Fast Vertex Weighting-Based Local Search for Finding Minimum Connected Dominating Sets,” INFORMS Journal on Computing, vol. 34, no. 2, pp. 817–833, Mar. 2022, doi: 10.1287/ijoc.2021.1106.
  - [2] X. Wu, Z. Lü, and P. Galinier, “Restricted swap-based neighborhood search for the minimum connected dominating set problem,” Networks, vol. 69, no. 2, pp. 222–236, Mar. 2017, doi: 10.1002/net.21728.


## 命令行参数

请大家编写程序时支持两个命令行参数, 依次为运行时间上限 (单位为秒) 和随机种子 (0-65535).
算例文件已重定向至标准输入 `stdin`/`cin`, 标准输出 `stdout`/`cout` 已重定向至解文件 (如需打印调试信息, 请使用标准错误输出 `stderr`/`cerr`).
例如, 在控制台运行以下命令表示调用可执行文件 `mcdsp.exe` 在限时 600 秒, 随机种子为 12345 的情况下求解路径为 `../data/LMS.n30e44.txt` 的算例, 解文件输出至 `sln.LMS.n30e44.txt`:
```
mcdsp.exe 600 123456 <../data/LMS.n30e44.txt >sln.LMS.n30e44.txt
```

- 运行时间上限.
  - 超出运行时间上限后测试程序会强行终止算法, 请确保在此之前已输出解 (最好还能自行正常退出).
- 随机种子设置.
  - 使用 C 语言随机数生成器请用 `srand`.
  - 使用 C++ 随机数生成器 (如 `mt19937`) 请在构造时传参或调用 `seed()` 方法设置.


## 输入的算例文件格式

所有算例的节点从 0 开始连续编号.

第一行给出 2 个由空白字符分隔的整数, 分别表示节点数 N 与无向边数 E (有向边数为 2E).
接下来连续 E 行, 每行包含 2 个由空白字符分隔的整数, 表示一条无向边的两个端点.

例如, 以下算例文件表示节点数为 4, 无向边数为 3; 其中:  
节点 0 分别与 1, 2, 3 相邻.
```
4 3 2
0 1
0 2
3 0
```


## 输出的解文件格式

输出 P 个用空白字符 (建议使用换行符) 分隔的整数, 分别表示挑选出的 P 个节点.

例如, 以下解文件表示选择节点 0 和 2 作为连通支配集:
```
0
2

```


## 提交要求

- 发送至邮箱 [szx@duhe.tech](mailto:szx@duhe.tech).
- 邮件标题格式为 "**Challenge2022MCDSP-姓名-学校-专业**".
- 邮件附件为单个压缩包 (文件大小 2M 以内), 文件名为 "**姓名-学校-专业**", 其内包含下列文件.
  - **必要** 算法的可执行文件 (Windows 平台).
    - 建议基于官方 SDK 开发 ([https://gitee.com/suzhouxing/npbenchmark/tree/main/SDK.MCDSP](https://gitee.com/suzhouxing/npbenchmark/tree/main/SDK.MCDSP)).
    - 用 g++ 的同学编译时建议静态链接, 即添加 `-static-libgcc -static-libstdc++` 编译选项.
  - **必要** 算法源码.
    - 可重入 (可在同一线程内反复调用而不会出现数据初始化错误或内存泄漏).
    - 可并发 (可在同一进程内的多个线程同时运行多个算法求解实例而互不干扰, 满足此要求一般不能有全局的非只读变量).
    - 可伸缩 (数据结构可以根据算例规模动态申请内存, 而非根据预先指定的编译期常量进行内存分配).
  - **可选** 算法在各算例上的运行情况概要, 至少包括以下几项信息 (仅在无法成功调用算法输出可通过检查程序的解时作为参考).
    - 算例名.
    - 连通支配集节点数.
    - 计算耗时.
  - **可选** 算法在各算例上求得的连通支配集节点数最少的解文件 (仅在无法成功调用算法输出可通过检查程序的解时作为参考).
- 其他所有问题通用的要求见 {% post_link Contest-ReadMe 'SmartLab Challenge - ReadMe' %}.

例如:
```
苏宙行-华科-计科.zip
|   mcdsp.exe
|   results.csv
|
+---src
|       main.cpp
|       algorithm.cpp
|       algorithm.h
|
+---results
        LPNMR.n40e200.txt
        LPNMR.n45e250.txt
        ...
```


## 算例清单

下载地址: [https://gitee.com/suzhouxing/npbenchmark.data/tree/data/MCDSP/Instance](https://gitee.com/suzhouxing/npbenchmark.data/tree/data/MCDSP/Instance)

算例规模从小到大依次为 (求解难度不一定随规模增加):

LPNMR.n40e200  
LPNMR.n45e250  
LPNMR.n50e250  
LPNMR.n55e250  
LPNMR.n60e400  
LPNMR.n70e250  
LPNMR.n80e500  
LPNMR.n90e600  
LMS.n30e44  
LMS.n30e87  
LMS.n30e131  
LMS.n30e218  
LMS.n30e305  
LMS.n50e61  
LMS.n50e123  
LMS.n50e245  
LMS.n50e368  
LMS.n50e613  
LMS.n50e858  
LMS.n70e121  
LMS.n70e242  
LMS.n70e483  
LMS.n70e725  
LMS.n70e1208  
LMS.n70e1691  
LMS.n100e248  
LMS.n100e495  
LMS.n100e990  
LMS.n100e1485  
LMS.n100e2475  
LMS.n100e3465  
LMS.n120e357  
LMS.n120e714  
LMS.n120e1428  
LMS.n120e2142  
LMS.n120e3570  
LMS.n120e4998  
LMS.n150e559  
LMS.n150e1118  
LMS.n150e2235  
LMS.n150e3353  
LMS.n150e5588  
LMS.n150e7823  
LMS.n200e995  
LMS.n200e1990  
LMS.n200e3980  
LMS.n200e5970  
LMS.n200e9950  
LMS.n200e13930  
BPFTC.n14e20  
BPFTC.n30e41  
BPFTC.n57e78  
BPFTC.n73e108  
BPFTC.n118e179  
BPFTC.n300e409  
RGG.n80e262  
RGG.n80e329  
RGG.n80e400  
RGG.n80e474  
RGG.n80e563  
RGG.n80e647  
RGG.n80e735  
RGG.n100e335  
RGG.n100e368  
RGG.n100e435  
RGG.n100e500  
RGG.n100e575  
RGG.n200e756  
RGG.n200e871  
RGG.n200e921  
RGG.n200e997  
RGG.n200e1113  
RGG.n200e1127  
RGG.n200e1269  
RGG.n200e1305  
RGG.n200e1400  
RGG.n200e1501  
RGG.n200e1541  
RGG.n200e1730  
RGG.n250e903  
RGG.n250e1011  
RGG.n250e1119  
RGG.n250e1246  
RGG.n300e1577  
RGG.n300e1710  
RGG.n300e1849  
RGG.n300e1990  
RGG.n350e1461  
RGG.n350e1555  
RGG.n350e1668  
RGG.n350e1787  
RGG.n400e1522  
RGG.n400e1621  
RGG.n400e1750  
RGG.n400e1880  
Sparse.n1000e1298  
Sparse.n1000e1497  
Sparse.n1000e1694  
Sparse.n1500e1995  
Sparse.n1500e2198  
Sparse.n1500e2397  
Sparse.n2000e2598  
Sparse.n2000e2798  
Sparse.n2000e2996  
Sparse.n2500e3297  
Sparse.n2500e3496  
Sparse.n2500e3697  
Sparse.n3000e3998  
Sparse.n3000e4298  
Sparse.n3000e4597  
BOBL.n1000e3449  
BOBL.n1000e3481  
BOBL.n1000e7150  
BOBL.n1000e7164  
BOBL.n1000e13923  
BOBL.n1000e14380  
BOBL.n1000e27002  
BOBL.n1000e27966  
BOBL.n1000e55730  
BOBL.n1000e56085  
BOBL.n1000e111644  
BOBL.n1000e111893  
BOBL.n5000e86922  
BOBL.n5000e87265  
BOBL.n5000e174799  
BOBL.n5000e183025  
BOBL.n5000e349693  
BOBL.n5000e361947  
BOBL.n5000e675960  
BOBL.n5000e701099  
BOBL.n5000e1400601  
BOBL.n5000e1408976  
BOBL.n5000e2792708  
BOBL.n5000e2797377  
