---
title: SmartLab Challenge 2022 - Directed Feedback Vertex Set Problem
date: 2022-04-15 21:49:35
categories:
- 算法挑战
tags:
- Challenge
- 算法挑战
- 组合优化
---
反馈点集问题在众多领域中有广泛的应用, 可以用来对多种组合优化问题进行建模.
有向反馈点集问题主要研究如何消除有向拓扑图中的环路问题.
比如在操作系统解决死锁时, 需要尽可能少地强行终止用户进程.
比如以层次图模式绘制思维导图或电路原理图时, 需要尽量避免出现逆向边.
高效的反馈点集问题求解算法具有极其重要的理论与应用价值.



# 有向反馈点集算法训练

## 问题概述

给定一个有向图, 请删除最少数量的节点 (所有与被删除节点直接相连的有向边均被一并删除), 使该有向图无环.

- 参考文献.
  - [1] P. Galinier, E. Lemamou, and M. W. Bouzidi, “Applying local search to the feedback vertex set problem,” Journal of Heuristics, vol. 19, no. 5, pp. 797–818, Oct. 2013, doi: 10.1007/s10732-013-9224-z.
  - [2] P. Festa, P. M. Pardalos, and M. G. C. Resende, “Feedback Set Problems,” in Handbook of Combinatorial Optimization: Supplement Volume A, D.-Z. Du and P. M. Pardalos, Eds. Boston, MA: Springer US, 1999, pp. 209–258. doi: 10.1007/978-1-4757-3023-4_4.
  - [3] D. Zhao, L. Xu, S. -M. Qin, G. Liu, and Z. Wang, “The Feedback Vertex Set Problem of Multiplex Networks,” IEEE Transactions on Circuits and Systems II: Express Briefs, vol. 67, no. 12, pp. 3492–3496, Dec. 2020, doi: 10.1109/TCSII.2020.2997974.


## 命令行参数

请大家编写程序时支持两个命令行参数, 依次为运行时间上限 (单位为秒) 和随机种子 (0-65535).
算例文件已重定向至标准输入 `stdin`/`cin`, 标准输出 `stdout`/`cout` 已重定向至解文件 (如需打印调试信息, 请使用标准错误输出 `stderr`/`cerr`).
例如, 在控制台运行以下命令表示调用可执行文件 `dfvsp.exe` 在限时 600 秒, 随机种子为 12345 的情况下求解路径为 `../data/pardalos.n50e100.txt` 的算例, 解文件输出至 `sln.pardalos.n50e100.txt`:
```
dfvsp.exe 600 123456 <../data/pardalos.n50e100.txt >sln.pardalos.n50e100.txt
```

- 运行时间上限.
  - 超出运行时间上限后测试程序会强行终止算法, 请确保在此之前已输出解 (最好还能自行正常退出).
- 随机种子设置.
  - 使用 C 语言随机数生成器请用 `srand`.
  - 使用 C++ 随机数生成器 (如 `mt19937`) 请在构造时传参或调用 `seed()` 方法设置.


## 输入的算例文件格式

所有算例的节点从 0 开始连续编号.

第一行给出 2 个由空白字符分隔的整数, 分别表示节点数 N, 有向边数 E.
接下来连续 N 行给出有向图的邻接表, 第 i 行表示第 i 个节点可以直接到达的节点集合, 节点集合由若干由空白字符分隔的整数表示.

例如, 以下算例文件表示节点数为 3, 有向边数为 4; 其中:  
从节点 0 出发可以到达节点 1 和 2;  
从节点 1 出发可以到达节点 0;  
从节点 2 出发可以到达节点 1.
```
3 4
1 2
0
1
```


## 输出的解文件格式

输出 K 个用空白字符 (建议使用换行符) 分隔的整数表示 K 个被删除的节点.

例如, 以下解文件表示删除节点 0 和 2:
```
0
2

```


## 提交要求

- 发送至邮箱 [szx@duhe.tech](mailto:szx@duhe.tech).
- 邮件标题格式为 "**Challenge2022DFVSP-姓名-学校-专业**".
- 邮件附件为单个压缩包 (文件大小 2M 以内), 文件名为 "**姓名-学校-专业**", 其内包含下列文件.
  - **必要** 算法的可执行文件 (Windows 平台).
    - 建议基于官方 SDK 开发 ([https://gitee.com/suzhouxing/npbenchmark/tree/main/SDK.DFVSP](https://gitee.com/suzhouxing/npbenchmark/tree/main/SDK.DFVSP)).
    - 用 g++ 的同学编译时建议静态链接, 即添加 `-static-libgcc -static-libstdc++` 编译选项.
  - **必要** 算法源码.
    - 可重入 (可在同一线程内反复调用而不会出现数据初始化错误或内存泄漏).
    - 可并发 (可在同一进程内的多个线程同时运行多个算法求解实例而互不干扰, 满足此要求一般不能有全局的非只读变量).
    - 可伸缩 (数据结构可以根据算例规模动态申请内存, 而非根据预先指定的编译期常量进行内存分配).
  - **可选** 算法在各算例上的运行情况概要, 至少包括以下几项信息 (可选, 仅在无法成功调用算法输出可通过检查程序的解时作为参考).
    - 算例名.
    - 删除节点数.
    - 计算耗时.
  - **可选** 算法在各算例上求得的颜色数最少的解文件 (可选, 仅在无法成功调用算法输出可通过检查程序的解时作为参考).
- 若成功提交, 在收到邮件时以及测试完成后系统均会自动发送邮件反馈提交情况.
  - 若测试结果较优, 可在排行榜页面看到自己的运行情况 ([https://gitee.com/suzhouxing/npbenchmark.data](https://gitee.com/suzhouxing/npbenchmark.data)).

例如:
```
苏宙行-华科-计科.zip
|   dfvsp.exe
|   results.csv
|
+---src
|       main.cpp
|       algorithm.cpp
|       algorithm.h
|
+---results
        pardalos.n50e100.txt
        pardalos.n50e150.txt
        ...
```


## 算例清单

下载地址: [https://gitee.com/suzhouxing/npbenchmark.data/DFVSP/Instance](https://gitee.com/suzhouxing/npbenchmark.data/DFVSP/Instance)

算例规模从小到大依次为 (求解难度不一定随规模增加):

pardalos.n50e100.txt  
pardalos.n50e150.txt  
pardalos.n50e200.txt  
pardalos.n50e250.txt  
pardalos.n50e300.txt  
pardalos.n50e500.txt  
pardalos.n50e600.txt  
pardalos.n50e700.txt  
pardalos.n50e800.txt  
pardalos.n50e900.txt  
pardalos.n100e200.txt  
pardalos.n100e300.txt  
pardalos.n100e400.txt  
pardalos.n100e500.txt  
pardalos.n100e600.txt  
pardalos.n100e1000.txt  
pardalos.n100e1100.txt  
pardalos.n100e1200.txt  
pardalos.n100e1300.txt  
pardalos.n100e1400.txt  
pardalos.n500e1000.txt  
pardalos.n500e1500.txt  
pardalos.n500e2000.txt  
pardalos.n500e2500.txt  
pardalos.n500e3000.txt  
pardalos.n500e5000.txt  
pardalos.n500e5500.txt  
pardalos.n500e6000.txt  
pardalos.n500e6500.txt  
pardalos.n500e7000.txt  
pardalos.n1000e3000.txt  
pardalos.n1000e3500.txt  
pardalos.n1000e4000.txt  
pardalos.n1000e4500.txt  
pardalos.n1000e5000.txt  
pardalos.n1000e10000.txt  
pardalos.n1000e15000.txt  
pardalos.n1000e20000.txt  
pardalos.n1000e25000.txt  
pardalos.n1000e30000.txt  
pace.h001.n1024e2103.txt  
pace.h003.n1024e3480.txt  
pace.h005.n843e3995.txt  
pace.h007.n2048e4096.txt  
pace.h009.n1024e5231.txt  
pace.h011.n1024e5480.txt  
pace.h013.n2048e5216.txt  
pace.h015.n2048e7005.txt  
pace.h017.n1024e9737.txt  
pace.h019.n1024e10284.txt  
pace.h021.n2048e10573.txt  
pace.h023.n2048e11098.txt  
pace.h025.n1024e15102.txt  
pace.h027.n2048e14738.txt  
pace.h029.n1024e19861.txt  
pace.h031.n2048e18876.txt  
pace.h033.n6301e20777.txt  
pace.h035.n8192e30498.txt  
pace.h037.n8192e30814.txt  
pace.h039.n9473e43691.txt  
pace.h041.n8192e45139.txt  
pace.h043.n8192e50901.txt  
pace.h045.n8192e54189.txt  
pace.h047.n8192e54807.txt  
pace.h049.n8192e57468.txt  
pace.h051.n8192e71995.txt  
pace.h053.n8192e73399.txt  
pace.h055.n15268e74339.txt  
pace.h057.n8192e83083.txt  
pace.h059.n7115e103689.txt  
pace.h061.n8192e112798.txt  
pace.h063.n36682e88328.txt  
pace.h065.n26475e106762.txt  
pace.h067.n8192e138733.txt  
pace.h069.n8192e156904.txt  
pace.h071.n8192e168714.txt  
pace.h073.n16384e166938.txt  
pace.h075.n34978e169522.txt  
pace.h077.n62586e147892.txt  
pace.h079.n16384e216334.txt  
pace.h081.n32768e224254.txt  
pace.h083.n32768e256256.txt  
pace.h085.n16384e283794.txt  
pace.h087.n58960e269439.txt  
pace.h089.n8192e358201.txt  
pace.h091.n16384e354031.txt  
pace.h093.n32768e363878.txt  
pace.h095.n32768e409363.txt  
pace.h097.n16384e458925.txt  
pace.h099.n97898e447979.txt  
pace.h101.n65536e618237.txt  
pace.h103.n159316e544621.txt  
pace.h105.n65536e652583.txt  
pace.h107.n32768e699385.txt  
pace.h109.n8192e839576.txt  
pace.h111.n77360e828161.txt  
pace.h113.n194085e854377.txt  
pace.h115.n262111e1234877.txt  
pace.h117.n325729e1469679.txt  
pace.h119.n342573e1619304.txt  
pace.h121.n16384e2076651.txt  
pace.h123.n4096e2097152.txt  
pace.h125.n16384e2097152.txt  
pace.h127.n32768e2128971.txt  
pace.h129.n32768e2148521.txt  
pace.h131.n16384e2194576.txt  
pace.h133.n131072e2130456.txt  
pace.h135.n32768e2235835.txt  
pace.h137.n32768e2241646.txt  
pace.h139.n16384e2293868.txt  
pace.h141.n65536e2321780.txt  
pace.h143.n32768e2412833.txt  
pace.h145.n32768e2416820.txt  
pace.h147.n32768e2422154.txt  
pace.h149.n131072e2340382.txt  
pace.h151.n65536e2457973.txt  
pace.h153.n32768e2554765.txt  
pace.h155.n131072e2459701.txt  
pace.h157.n281903e2312497.txt  
pace.h159.n32768e2621362.txt  
pace.h161.n32768e2684883.txt  
pace.h163.n32768e2696696.txt  
pace.h165.n131072e2622100.txt  
pace.h167.n65536e2731506.txt  
pace.h169.n32768e2808603.txt  
pace.h171.n131072e2741624.txt  
pace.h173.n32768e2843144.txt  
pace.h175.n131072e2757784.txt  
pace.h177.n131072e2784435.txt  
pace.h179.n131072e2921009.txt  
pace.h181.n131072e2950732.txt  
pace.h183.n131072e3115621.txt  
pace.h185.n32768e3222505.txt  
pace.h187.n131072e3225441.txt  
pace.h189.n262144e3100310.txt  
pace.h191.n131072e3275806.txt  
pace.h193.n32768e3734348.txt  
pace.h195.n4096e4194304.txt  
pace.h197.n8192e4194304.txt  
pace.h199.n875713e5105039.txt  

<details style="border: 1px solid #aaa; border-radius: 4px;"><summary>⯈ 隐藏算例</summary>
pace.e001.n512e651.txt  
pace.e003.n1024e6055.txt  
pace.e005.n1024e5736.txt  
pace.e007.n1024e4198.txt  
pace.e009.n1024e6804.txt  
pace.e011.n4096e15076.txt  
pace.e013.n2048e7038.txt  
pace.e015.n2048e9390.txt  
pace.e017.n1024e3800.txt  
pace.e019.n2048e11718.txt  
pace.e021.n2048e6147.txt  
pace.e023.n2048e5400.txt  
pace.e025.n1024e2802.txt  
pace.e027.n2048e9406.txt  
pace.e029.n2048e11302.txt  
pace.e031.n4096e26071.txt  
pace.e033.n1024e2010.txt  
pace.e035.n4096e17074.txt  
pace.e037.n4096e28675.txt  
pace.e039.n1024e2570.txt  
pace.e041.n4096e27082.txt  
pace.e043.n2048e5700.txt  
pace.e045.n4096e30202.txt  
pace.e047.n4096e28612.txt  
pace.e049.n4096e23857.txt  
pace.e051.n4096e12946.txt  
pace.e053.n4096e26872.txt  
pace.e055.n4096e12085.txt  
pace.e057.n4096e12298.txt  
pace.e059.n1024e1124.txt  
pace.e061.n2048e4320.txt  
pace.e063.n4096e23766.txt  
pace.e065.n8192e48759.txt  
pace.e067.n2048e4241.txt  
pace.e069.n4096e22368.txt  
pace.e071.n2048e4896.txt  
pace.e073.n8192e51796.txt  
pace.e075.n8192e31504.txt  
pace.e077.n8192e54744.txt  
pace.e079.n8192e52154.txt  
pace.e081.n8192e51030.txt  
pace.e083.n2048e3537.txt  
pace.e085.n8192e69446.txt  
pace.e087.n8192e69372.txt  
pace.e089.n8192e55556.txt  
pace.e091.n2048e3364.txt  
pace.e093.n16384e109487.txt  
pace.e095.n8192e59029.txt  
pace.e097.n16384e103725.txt  
pace.e099.n8192e38175.txt  
pace.e101.n32768e246723.txt  
pace.e103.n16384e77536.txt  
pace.e105.n2048e10584.txt  
pace.e107.n16384e104833.txt  
pace.e109.n16384e149298.txt  
pace.e111.n2048e31904.txt  
pace.e113.n1024e25750.txt  
pace.e115.n32768e304304.txt  
pace.e117.n32768e304790.txt  
pace.e119.n32768e272707.txt  
pace.e121.n1024e5480.txt  
pace.e123.n32768e256256.txt  
pace.e125.n32768e288748.txt  
pace.e127.n2048e25492.txt  
pace.e129.n32768e256665.txt  
pace.e131.n32768e240240.txt  
pace.e133.n2048e22332.txt  
pace.e135.n32768e240816.txt  
pace.e137.n2048e7003.txt  
pace.e139.n65536e288492.txt  
pace.e141.n65536e303686.txt  
pace.e143.n65536e303696.txt  
pace.e145.n2048e27086.txt  
pace.e147.n1024e25052.txt  
pace.e149.n1024e3624.txt  
pace.e151.n65536e653240.txt  
pace.e153.n2048e22306.txt  
pace.e155.n65536e619239.txt  
pace.e157.n65536e652583.txt  
pace.e159.n65536e550097.txt  
pace.e161.n2048e28713.txt  
pace.e163.n131072e574081.txt  
pace.e165.n131072e603932.txt  
pace.e167.n1024e5487.txt  
pace.e169.n131072e604300.txt  
pace.e171.n131072e604042.txt  
pace.e173.n1024e23175.txt  
pace.e175.n2048e22520.txt  
pace.e177.n32768e320320.txt  
pace.e179.n2048e27346.txt  
pace.e181.n32768e321088.txt  
pace.e183.n1024e5471.txt  
pace.e185.n4096e62879.txt  
pace.e187.n1024e3632.txt  
pace.e189.n1024e2048.txt  
pace.e191.n2048e4143.txt  
pace.e193.n1024e17144.txt  
pace.e195.n1024e32768.txt  
pace.e197.n4096e93346.txt  
pace.e199.n16384e559996.txt  
</details>
