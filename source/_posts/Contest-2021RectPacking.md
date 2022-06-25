---
title: SmartLab Challenge 2021 - Rectangle Packing
date: 2021-07-10 17:21:42
categories:
- 算法挑战
tags:
- Challenge
- 算法挑战
- 组合优化
---

矩形装箱问题是物流运输, 芯片制造与游戏开发等领域中的重要问题, 应用场景十分广泛.
矩形装箱问题主要研究如何将一系列矩形块不重叠地摆放至矩形容器中, 使得容器面积最小的问题.
比如玻璃制造过程中, 要在原料 (大矩形) 上进行切割得到一系列成品 (小矩形).
比如在集成电路的物理设计中, 需要用最小的面积排布大量宏单元和标准单元以节约成本.
比如在游戏贴图加载时, 需要将若干矩形图片拼合成大块的矩形图像以充分发挥显存性能.
因此, 高效的矩形装箱问题的求解算法具有极其重要的理论与应用价值.



# 矩形装箱算法训练

## 问题概述

给定一系列长宽固定的矩形块, 可以旋转 0 或 90 度, 请确定每个矩形块的左下角坐标, 使得在所有矩形块不重叠的情况下, 包络所有矩形块的矩形区域面积最小.

- 参考文献.
  - [1] L. Wei, W.-C. Oon, W. Zhu, and A. Lim, “A skyline heuristic for the 2D rectangular packing and strip packing problems,” European Journal of Operational Research, vol. 215, no. 2, pp. 337–346, 2011, doi: 10.1016/j.ejor.2011.06.022.
  - [2] L. Wei, T. Tian, W. Zhu, and A. Lim, “A block-based layer building approach for the 2D guillotine strip packing problem,” European Journal of Operational Research, vol. 239, no. 1, pp. 58–69, 2014, doi: 10.1016/j.ejor.2014.04.020.
  - [3] K. He, P. Ji, and C. Li, “Dynamic reduction heuristics for the rectangle packing area minimization problem,” European Journal of Operational Research, vol. 241, no. 3, pp. 674–685, 2015, doi: 10.1016/j.ejor.2014.09.042.
  - [4] L. Wei, Q. Hu, S. C. H. Leung, and N. Zhang, “An improved skyline based heuristic for the 2D strip packing problem and its efficient implementation,” Computers & Operations Research, vol. 80, pp. 113–127, 2017, doi: 10.1016/j.cor.2016.11.024.
  - [5] K. He, H. Yang, Y. Jin, Q. Hu, and P. Ji, “The Orthogonal Packing and Scheduling Problem: Model, Heuristic, and Benchmark,” IEEE Transactions on Systems, Man, and Cybernetics: Systems, vol. 50, no. 4, pp. 1372–1383, Apr. 2020, doi: 10.1109/TSMC.2017.2768072.
  - [6] P. Ji, K. He, Z. Wang, Y. Jin, and J. Wu, “A Quasi-Newton-based Floorplanner for fixed-outline floorplanning,” Computers & Operations Research, vol. 129, p. 105225, 2021, doi: 10.1016/j.cor.2021.105225.


## 命令行参数

请大家编写程序时支持两个命令行参数, 依次为运行时间上限 (单位为秒) 和随机种子 (0-65535).
算例文件已重定向至标准输入 `stdin`/`cin`, 标准输出 `stdout`/`cout` 已重定向至解文件 (如需打印调试信息, 请使用标准错误输出 `stderr`/`cerr`).
例如, 在控制台运行以下命令表示调用可执行文件 `packing.exe` 在限时 300 秒, 随机种子为 12345 的情况下求解路径为 `../data/gsrc.b10a221679.txt` 的算例, 解文件输出至 `sln.gsrc.b10a221679.txt`:
```
packing.exe 300 123456 <../data/gsrc.b10a221679.txt >sln.gsrc.b10a221679.txt
```

- 运行时间上限.
  - 超出运行时间上限后测试程序会强行终止算法, 请确保在此之前已输出解 (最好还能自行正常退出).
- 随机种子设置.
  - 使用 C 语言随机数生成器请用 `srand`.
  - 使用 C++ 随机数生成器 (如 `mt19937`) 请在构造时传参或调用 `seed()` 方法设置.


## 输入的算例文件格式

所有算例的矩形块均从 0 开始连续编号.

第一行给出一个整数 N, 表示矩形块的数量.

接下来连续 N 行, 每行包含 2 个由空白字符分隔的整数, 表示矩形块的长和宽 (不发生旋转的情况下，长度对应横坐标，宽度对应纵坐标).

例如, 以下算例文件表示需放置 4 个矩形块; 其中,  
矩形块 0 的长为 1 宽为 3;  
矩形块 1 的长为 3 宽为 1;  
矩形块 2 的长为 2 宽为 2;  
矩形块 3 的长为 2 宽为 2:
```
4
1 3
3 1
2 2
2 2
```


## 输出的解文件格式

输出 N 行整数表示 N 个矩形块的放置情况, 第 i 行表示第 i 个矩形块的左下角坐标和旋转情况.
每行 3 个由空白字符分隔的整数, 分别为第 i 个矩形块的最小横坐标, 最小纵坐标, 旋转度数 (必须为 0 或 90).

例如, 以下解文件表示 4 个矩形块的放置情况; 其中,  
矩形块 0 的左下角坐标为 (0, 0), 不旋转;  
矩形块 1 的左下角坐标为 (1, 0), 旋转 90 度;  
矩形块 2 的左下角坐标为 (2, 0), 不旋转;  
矩形块 3 的左下角坐标为 (2, 2), 不旋转:
```
0 0 0
1 0 90
2 0 0
2 2 0

```


## 提交要求

- 发送至邮箱 [szx@duhe.tech](mailto:szx@duhe.tech).
- 邮件标题格式为 "**Challenge2021RPP-姓名-学校-专业**".
- 邮件附件为单个压缩包 (文件大小 2M 以内), 文件名为 "**姓名-学校-专业**", 其内包含下列文件.
  - **必要** 算法的可执行文件 (Windows 平台).
    - 建议基于官方 SDK 开发 ([https://gitee.com/suzhouxing/npbenchmark/tree/main/SDK.RPP](https://gitee.com/suzhouxing/npbenchmark/tree/main/SDK.RPP)).
    - 用 g++ 的同学编译时请静态链接, 即添加 `-static-libgcc -static-libstdc++` 编译选项.
  - **必要** 算法源码.
    - 可重入 (可在同一线程内反复调用而不会出现数据初始化错误或内存泄漏).
    - 可并发 (可在同一进程内的多个线程同时运行多个算法求解实例而互不干扰, 满足此要求一般不能有全局的非只读变量).
    - 可伸缩 (数据结构可以根据算例规模动态申请内存, 而非根据预先指定的编译期常量进行内存分配).
  - **可选** 算法在各算例上的运行情况概要, 至少包括以下几项信息 (仅在无法成功调用算法输出可通过检查程序的解时作为参考).
    - 算例名.
    - 剩余未覆盖元素数.
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
        gsrc.b10a221679.txt
        gsrc.b30a208591.txt
        ...
```


## 算例清单

下载地址: [https://gitee.com/suzhouxing/npbenchmark.data/tree/data/RPP/Instance](https://gitee.com/suzhouxing/npbenchmark.data/tree/data/RPP/Instance)

gsrc.b10a221679  
gsrc.b30a208591  
gsrc.b50a198579  
gsrc.b100a179501  
gsrc.b200a175696  
gsrc.b300a273170  
mcnc.ami33.b33a1156449  
mcnc.ami49.b49a35445424  
mcnc.apte.b9a46561628  
mcnc.hp.b11a8830584  
mcnc.xerox.b10a19350296  
