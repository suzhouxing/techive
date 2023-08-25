---
title: SmartLab Challenge 2024 - Latin Square Completion
date: 2023-08-23 14:46:50
categories:
- 算法挑战
tags:
- Challenge
- 算法挑战
- 组合优化
---
拉丁方补全问题是一个经典的益智游戏, 也是图着色问题的特例.
拉丁方问题要求使用整数填充一个方阵, 保证每行每列均不出现重复数字, 与八皇后问题和数独问题联系十分紧密.
显然, 拉丁方问题存在平凡解或者说通解, 即按照特定规则简单构造便可得到可行解.
而拉丁方补全问题则通过引入一系列固定数字的单元格, 使问题的难度急剧增加.
高效的拉丁方问题求解算法经过合适的改造, 对一般的图着色问题的求解有重要的指导意义.



# 拉丁方补全算法训练

## 问题概述

给定一个 N * N 方阵, 部分单元格已经填充了数字且不得修改, 请为剩余的每个单元格填充一个 0 到 N-1 的整数, 使任意一行和任意一列内的数字均不相同.
为提高不同算法效果的区分度, 测试平台将松弛同行同列数字不同的约束, 并将存在相同数字的行或列的数量作为优化目标.

- 参考文献.
  - [1] S. Pan, Y. Wang, M. Yin, “A Fast Local Search Algorithm for the Latin Square Completion Problem,” AAAI 2022, vol. 36, no. 9, pp. 10327-10335, doi: 10.1609/aaai.v36i9.21274.
  - [2] Y. Jin and J.-K. Hao, “Solving the Latin Square Completion Problem by Memetic Graph Coloring,” IEEE Transactions on Evolutionary Computation, vol. 23, no. 6, pp. 1015-1028,  2019, doi: 10.1109/TEVC.2019.2899053.
  - [3] Z. Lü and J.-K. Hao, “A memetic algorithm for graph coloring,” European Journal of Operational Research, vol. 203, no. 1, pp. 241–250, 2010, doi: 10.1016/j.ejor.2009.07.016.
  - [4] L. Moalic and A. Gondran, “Variations on memetic algorithms for graph coloring problems,” Journal of Heuristics, vol. 24, no. 1, pp. 1–24, 2018, doi: 10.1007/s10732-017-9354-9.


## 命令行参数

请大家编写程序时支持两个命令行参数, 依次为运行时间上限 (单位为秒) 和随机种子 (0-65535).
算例文件已重定向至标准输入 `stdin`/`cin`, 标准输出 `stdout`/`cout` 已重定向至解文件 (如需打印调试信息, 请使用标准错误输出 `stderr`/`cerr`).
例如, 在控制台运行以下命令表示调用可执行文件 `lsc.exe` 在限时 600 秒, 随机种子为 12345 的情况下求解路径为 `../data/LSC.n50f750.00.txt` 的算例, 解文件输出至 `sln.LSC.n50f750.00.txt`:
```
lsc.exe 600 123456 <../data/LSC.n50f750.00.txt >sln.LSC.n50f750.00.txt
```

- 运行时间上限.
  - 超出运行时间上限后测试程序会强行终止算法, 请确保在此之前已输出解 (最好还能自行正常退出).
- 随机种子设置.
  - 使用 C 语言随机数生成器请用 `srand`.
  - 使用 C++ 随机数生成器 (如 `mt19937`) 请在构造时传参或调用 `seed()` 方法设置.


## 输入的算例文件格式

所有算例的行列与可填充数字均从 0 开始连续编号.

第一行给出 1 个整数, 表示方阵维度 N, 也即可填充的数字范围.
接下来连续若干行, 给出已填充固定整数的单元格信息.
每行包含 3 个由空白字符分隔的整数, 分别表示单元格的所在行 R, 单元格的所在列 C, 单元格填充整数 I.

例如, 以下算例文件表示方阵维度为 4; 其中:  
第 0 行第 1 列固定填充整数 2;  
第 2 行第 0 列固定填充整数 3.
```
4
0 1 2
2 0 3
```


## 输出的解文件格式

输出 N * N 个用空白字符分隔整数表示方阵中 N 行 N 列单元格填充的整数 (包括固定单元格), 第 i * N + j 个整数表示第 i 行第 j 列单元格填充的整数.

例如, 以下解文件表示第 1 行填充整数 0, 1, 2, 3, 第 2 行填充整数 1, 2, 3, 0, ...:
```
0 1 2 3
1 2 3 0
2 3 0 1
3 0 1 2

```


## 提交要求

- 发送至邮箱 [szx@duhe.tech](mailto:szx@duhe.tech).
- 邮件标题格式为 "**Challenge2024LSC-姓名-学校-专业**".
- 邮件附件为单个压缩包 (文件大小 2M 以内), 文件名为 "**姓名-学校-专业**", 其内包含下列文件.
  - **必要** 算法的可执行文件 (Windows 平台).
    - 建议基于官方 SDK 开发 ([https://gitee.com/suzhouxing/npbenchmark/tree/main/SDK.LSC](https://gitee.com/suzhouxing/npbenchmark/tree/main/SDK.LSC)).
    - 用 g++ 的同学编译时建议静态链接, 即添加 `-static-libgcc -static-libstdc++` 编译选项.
  - **必要** 算法源码.
    - 可重入 (可在同一线程内反复调用而不会出现数据初始化错误或内存泄漏).
    - 可并发 (可在同一进程内的多个线程同时运行多个算法求解实例而互不干扰, 满足此要求一般不能有全局的非只读变量).
    - 可伸缩 (数据结构可以根据算例规模动态申请内存, 而非根据预先指定的编译期常量进行内存分配).
  - **可选** 算法在各算例上的运行情况概要, 至少包括以下几项信息 (仅在无法成功调用算法输出可通过检查程序的解时作为参考).
    - 算例名.
    - 计算耗时.
  - **可选** 算法在各算例上求得的无冲突的解文件 (仅在无法成功调用算法输出可通过检查程序的解时作为参考).
- 其他所有问题通用的要求见 {% post_link Contest-ReadMe 'SmartLab Challenge - ReadMe' %}.

例如:
```
苏宙行-华科-计科.zip
|   lsc.exe
|   results.csv
|
+---src
|       main.cpp
|       algorithm.cpp
|       algorithm.h
|
+---results
        LSC.n50f750.00.txt
        LSC.n50f750.01.txt
        ...
```


## 算例清单

下载地址: [https://gitee.com/suzhouxing/npbenchmark.data/tree/data/LSC/Instance](https://gitee.com/suzhouxing/npbenchmark.data/tree/data/LSC/Instance)

算例规模从小到大依次为:

LSC.n50f1750.00  
LSC.n50f1750.01  
LSC.n50f1750.02  
LSC.n50f1750.03  
LSC.n50f1750.04  
LSC.n50f1750.05  
LSC.n50f1750.06  
LSC.n50f1750.07  
LSC.n50f1750.08  
LSC.n50f1750.09  
LSC.n50f1750.10  
LSC.n50f1750.11  
LSC.n50f1750.12  
LSC.n50f1750.13  
LSC.n50f1750.14  
LSC.n50f1750.15  
LSC.n50f1750.16  
LSC.n50f1750.17  
LSC.n50f1750.18  
LSC.n50f1750.19  
LSC.n50f1750.20  
LSC.n50f1750.21  
LSC.n50f1750.22  
LSC.n50f1750.23  
LSC.n50f1750.24  
LSC.n50f1750.25  
LSC.n50f1750.26  
LSC.n50f1750.27  
LSC.n50f1750.28  
LSC.n50f1750.29  
LSC.n50f1750.30  
LSC.n50f1750.31  
LSC.n50f1750.32  
LSC.n50f1750.33  
LSC.n50f1750.34  
LSC.n50f1750.35  
LSC.n50f1750.36  
LSC.n50f1750.37  
LSC.n50f1750.38  
LSC.n50f1750.39  
LSC.n50f1750.40  
LSC.n50f1750.41  
LSC.n50f1750.42  
LSC.n50f1750.43  
LSC.n50f1750.44  
LSC.n50f1750.45  
LSC.n50f1750.46  
LSC.n50f1750.47  
LSC.n50f1750.48  
LSC.n50f1750.49  
LSC.n50f1750.50  
LSC.n50f1750.51  
LSC.n50f1750.52  
LSC.n50f1750.53  
LSC.n50f1750.54  
LSC.n50f1750.55  
LSC.n50f1750.56  
LSC.n50f1750.57  
LSC.n50f1750.58  
LSC.n50f1750.59  
LSC.n50f1750.60  
LSC.n50f1750.61  
LSC.n50f1750.62  
LSC.n50f1750.63  
LSC.n50f1750.64  
LSC.n50f1750.65  
LSC.n50f1750.66  
LSC.n50f1750.67  
LSC.n50f1750.68  
LSC.n50f1750.69  
LSC.n50f1750.70  
LSC.n50f1750.71  
LSC.n50f1750.72  
LSC.n50f1750.73  
LSC.n50f1750.74  
LSC.n50f1750.75  
LSC.n50f1750.76  
LSC.n50f1750.77  
LSC.n50f1750.78  
LSC.n50f1750.79  
LSC.n50f1750.80  
LSC.n50f1750.81  
LSC.n50f1750.82  
LSC.n50f1750.83  
LSC.n50f1750.84  
LSC.n50f1750.85  
LSC.n50f1750.86  
LSC.n50f1750.87  
LSC.n50f1750.88  
LSC.n50f1750.89  
LSC.n50f1750.90  
LSC.n50f1750.91  
LSC.n50f1750.92  
LSC.n50f1750.93  
LSC.n50f1750.94  
LSC.n50f1750.95  
LSC.n50f1750.96  
LSC.n50f1750.97  
LSC.n50f1750.98  
LSC.n50f1750.99  
LSC.n60f2520.00  
LSC.n60f2520.01  
LSC.n60f2520.02  
LSC.n60f2520.03  
LSC.n60f2520.04  
LSC.n60f2520.05  
LSC.n60f2520.06  
LSC.n60f2520.07  
LSC.n60f2520.08  
LSC.n60f2520.09  
LSC.n60f2520.10  
LSC.n60f2520.11  
LSC.n60f2520.12  
LSC.n60f2520.13  
LSC.n60f2520.14  
LSC.n60f2520.15  
LSC.n60f2520.16  
LSC.n60f2520.17  
LSC.n60f2520.18  
LSC.n60f2520.19  
LSC.n60f2520.20  
LSC.n60f2520.21  
LSC.n60f2520.22  
LSC.n60f2520.23  
LSC.n60f2520.24  
LSC.n60f2520.25  
LSC.n60f2520.26  
LSC.n60f2520.27  
LSC.n60f2520.28  
LSC.n60f2520.29  
LSC.n60f2520.30  
LSC.n60f2520.31  
LSC.n60f2520.32  
LSC.n60f2520.33  
LSC.n60f2520.34  
LSC.n60f2520.35  
LSC.n60f2520.36  
LSC.n60f2520.37  
LSC.n60f2520.38  
LSC.n60f2520.39  
LSC.n60f2520.40  
LSC.n60f2520.41  
LSC.n60f2520.42  
LSC.n60f2520.43  
LSC.n60f2520.44  
LSC.n60f2520.45  
LSC.n60f2520.46  
LSC.n60f2520.47  
LSC.n60f2520.48  
LSC.n60f2520.49  
LSC.n60f2520.50  
LSC.n60f2520.51  
LSC.n60f2520.52  
LSC.n60f2520.53  
LSC.n60f2520.54  
LSC.n60f2520.55  
LSC.n60f2520.56  
LSC.n60f2520.57  
LSC.n60f2520.58  
LSC.n60f2520.59  
LSC.n60f2520.60  
LSC.n60f2520.61  
LSC.n60f2520.62  
LSC.n60f2520.63  
LSC.n60f2520.64  
LSC.n60f2520.65  
LSC.n60f2520.66  
LSC.n60f2520.67  
LSC.n60f2520.68  
LSC.n60f2520.69  
LSC.n60f2520.70  
LSC.n60f2520.71  
LSC.n60f2520.72  
LSC.n60f2520.73  
LSC.n60f2520.74  
LSC.n60f2520.75  
LSC.n60f2520.76  
LSC.n60f2520.77  
LSC.n60f2520.78  
LSC.n60f2520.79  
LSC.n60f2520.80  
LSC.n60f2520.81  
LSC.n60f2520.82  
LSC.n60f2520.83  
LSC.n60f2520.84  
LSC.n60f2520.85  
LSC.n60f2520.86  
LSC.n60f2520.87  
LSC.n60f2520.88  
LSC.n60f2520.89  
LSC.n60f2520.90  
LSC.n60f2520.91  
LSC.n60f2520.92  
LSC.n60f2520.93  
LSC.n60f2520.94  
LSC.n60f2520.95  
LSC.n60f2520.96  
LSC.n60f2520.97  
LSC.n60f2520.98  
LSC.n60f2520.99  
LSC.n70f3430.00  
LSC.n70f3430.01  
LSC.n70f3430.02  
LSC.n70f3430.03  
LSC.n70f3430.04  
LSC.n70f3430.05  
LSC.n70f3430.06  
LSC.n70f3430.07  
LSC.n70f3430.08  
LSC.n70f3430.09  
LSC.n70f3430.10  
LSC.n70f3430.11  
LSC.n70f3430.12  
LSC.n70f3430.13  
LSC.n70f3430.14  
LSC.n70f3430.15  
LSC.n70f3430.16  
LSC.n70f3430.17  
LSC.n70f3430.18  
LSC.n70f3430.19  
LSC.n70f3430.20  
LSC.n70f3430.21  
LSC.n70f3430.22  
LSC.n70f3430.23  
LSC.n70f3430.24  
LSC.n70f3430.25  
LSC.n70f3430.26  
LSC.n70f3430.27  
LSC.n70f3430.28  
LSC.n70f3430.29  
LSC.n70f3430.30  
LSC.n70f3430.31  
LSC.n70f3430.32  
LSC.n70f3430.33  
LSC.n70f3430.34  
LSC.n70f3430.35  
LSC.n70f3430.36  
LSC.n70f3430.37  
LSC.n70f3430.38  
LSC.n70f3430.39  
LSC.n70f3430.40  
LSC.n70f3430.41  
LSC.n70f3430.42  
LSC.n70f3430.43  
LSC.n70f3430.44  
LSC.n70f3430.45  
LSC.n70f3430.46  
LSC.n70f3430.47  
LSC.n70f3430.48  
LSC.n70f3430.49  
LSC.n70f3430.50  
LSC.n70f3430.51  
LSC.n70f3430.52  
LSC.n70f3430.53  
LSC.n70f3430.54  
LSC.n70f3430.55  
LSC.n70f3430.56  
LSC.n70f3430.57  
LSC.n70f3430.58  
LSC.n70f3430.59  
LSC.n70f3430.60  
LSC.n70f3430.61  
LSC.n70f3430.62  
LSC.n70f3430.63  
LSC.n70f3430.64  
LSC.n70f3430.65  
LSC.n70f3430.66  
LSC.n70f3430.67  
LSC.n70f3430.68  
LSC.n70f3430.69  
LSC.n70f3430.70  
LSC.n70f3430.71  
LSC.n70f3430.72  
LSC.n70f3430.73  
LSC.n70f3430.74  
LSC.n70f3430.75  
LSC.n70f3430.76  
LSC.n70f3430.77  
LSC.n70f3430.78  
LSC.n70f3430.79  
LSC.n70f3430.80  
LSC.n70f3430.81  
LSC.n70f3430.82  
LSC.n70f3430.83  
LSC.n70f3430.84  
LSC.n70f3430.85  
LSC.n70f3430.86  
LSC.n70f3430.87  
LSC.n70f3430.88  
LSC.n70f3430.89  
LSC.n70f3430.90  
LSC.n70f3430.91  
LSC.n70f3430.92  
LSC.n70f3430.93  
LSC.n70f3430.94  
LSC.n70f3430.95  
LSC.n70f3430.96  
LSC.n70f3430.97  
LSC.n70f3430.98  
LSC.n70f3430.99  
LSC.n70f3920.00  
LSC.n70f3920.01  
LSC.n70f3920.02  
LSC.n70f3920.03  
LSC.n70f3920.04  
LSC.n70f3920.05  
LSC.n70f3920.06  
LSC.n70f3920.07  
LSC.n70f3920.08  
LSC.n70f3920.09  
LSC.n70f3920.10  
LSC.n70f3920.11  
LSC.n70f3920.12  
LSC.n70f3920.13  
LSC.n70f3920.14  
LSC.n70f3920.15  
LSC.n70f3920.16  
LSC.n70f3920.17  
LSC.n70f3920.18  
LSC.n70f3920.19  
LSC.n70f3920.20  
LSC.n70f3920.21  
LSC.n70f3920.22  
LSC.n70f3920.23  
LSC.n70f3920.24  
LSC.n70f3920.25  
LSC.n70f3920.26  
LSC.n70f3920.27  
LSC.n70f3920.28  
LSC.n70f3920.29  
LSC.n70f3920.30  
LSC.n70f3920.31  
LSC.n70f3920.32  
LSC.n70f3920.33  
LSC.n70f3920.34  
LSC.n70f3920.35  
LSC.n70f3920.36  
LSC.n70f3920.37  
LSC.n70f3920.38  
LSC.n70f3920.39  
LSC.n70f3920.40  
LSC.n70f3920.41  
LSC.n70f3920.42  
LSC.n70f3920.43  
LSC.n70f3920.44  
LSC.n70f3920.45  
LSC.n70f3920.46  
LSC.n70f3920.47  
LSC.n70f3920.48  
LSC.n70f3920.49  
LSC.n70f3920.50  
LSC.n70f3920.51  
LSC.n70f3920.52  
LSC.n70f3920.53  
LSC.n70f3920.54  
LSC.n70f3920.55  
LSC.n70f3920.56  
LSC.n70f3920.57  
LSC.n70f3920.58  
LSC.n70f3920.59  
LSC.n70f3920.60  
LSC.n70f3920.61  
LSC.n70f3920.62  
LSC.n70f3920.63  
LSC.n70f3920.64  
LSC.n70f3920.65  
LSC.n70f3920.66  
LSC.n70f3920.67  
LSC.n70f3920.68  
LSC.n70f3920.69  
LSC.n70f3920.70  
LSC.n70f3920.71  
LSC.n70f3920.72  
LSC.n70f3920.73  
LSC.n70f3920.74  
LSC.n70f3920.75  
LSC.n70f3920.76  
LSC.n70f3920.77  
LSC.n70f3920.78  
LSC.n70f3920.79  
LSC.n70f3920.80  
LSC.n70f3920.81  
LSC.n70f3920.82  
LSC.n70f3920.83  
LSC.n70f3920.84  
LSC.n70f3920.85  
LSC.n70f3920.86  
LSC.n70f3920.87  
LSC.n70f3920.88  
LSC.n70f3920.89  
LSC.n70f3920.90  
LSC.n70f3920.91  
LSC.n70f3920.92  
LSC.n70f3920.93  
LSC.n70f3920.94  
LSC.n70f3920.95  
LSC.n70f3920.96  
LSC.n70f3920.97  
LSC.n70f3920.98  
LSC.n70f3920.99  
