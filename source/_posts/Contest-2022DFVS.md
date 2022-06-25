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
  - [2] Hen-Ming Lin and Jing-Yang Jou, “Computing minimum feedback vertex sets by contraction operations and its applications on CAD,” in Proceedings 1999 IEEE International Conference on Computer Design: VLSI in Computers and Processors (Cat. No.99CB37040), Oct. 1999, pp. 364–369. doi: 10.1109/ICCD.1999.808567.
  - [3] P. Festa, P. M. Pardalos, and M. G. C. Resende, “Feedback Set Problems,” in Handbook of Combinatorial Optimization: Supplement Volume A, D.-Z. Du and P. M. Pardalos, Eds. Boston, MA: Springer US, 1999, pp. 209–258. doi: 10.1007/978-1-4757-3023-4_4.
  - [4] D. Zhao, L. Xu, S. -M. Qin, G. Liu, and Z. Wang, “The Feedback Vertex Set Problem of Multiplex Networks,” IEEE Transactions on Circuits and Systems II: Express Briefs, vol. 67, no. 12, pp. 3492–3496, Dec. 2020, doi: 10.1109/TCSII.2020.2997974.


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
  - **可选** 算法在各算例上的运行情况概要, 至少包括以下几项信息 (仅在无法成功调用算法输出可通过检查程序的解时作为参考).
    - 算例名.
    - 删除节点数.
    - 计算耗时.
  - **可选** 算法在各算例上求得的颜色数最少的解文件 (仅在无法成功调用算法输出可通过检查程序的解时作为参考).
- 其他所有问题通用的要求见 {% post_link Contest-ReadMe 'SmartLab Challenge - ReadMe' %}.

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

下载地址: [https://gitee.com/suzhouxing/npbenchmark.data/tree/data/DFVSP/Instance](https://gitee.com/suzhouxing/npbenchmark.data/tree/data/DFVSP/Instance)

算例规模从小到大依次为 (求解难度不一定随规模增加):

pardalos.n50e100  
pardalos.n50e150  
pardalos.n50e200  
pardalos.n50e250  
pardalos.n50e300  
pardalos.n50e500  
pardalos.n50e600  
pardalos.n50e700  
pardalos.n50e800  
pardalos.n50e900  
pardalos.n100e200  
pardalos.n100e300  
pardalos.n100e400  
pardalos.n100e500  
pardalos.n100e600  
pardalos.n100e1000  
pardalos.n100e1100  
pardalos.n100e1200  
pardalos.n100e1300  
pardalos.n100e1400  
pardalos.n500e1000  
pardalos.n500e1500  
pardalos.n500e2000  
pardalos.n500e2500  
pardalos.n500e3000  
pardalos.n500e5000  
pardalos.n500e5500  
pardalos.n500e6000  
pardalos.n500e6500  
pardalos.n500e7000  
pardalos.n1000e3000  
pardalos.n1000e3500  
pardalos.n1000e4000  
pardalos.n1000e4500  
pardalos.n1000e5000  
pardalos.n1000e10000  
pardalos.n1000e15000  
pardalos.n1000e20000  
pardalos.n1000e25000  
pardalos.n1000e30000  
pace.h001.n1024e2103  
pace.h003.n1024e3480  
pace.h005.n843e3995  
pace.h007.n2048e4096  
pace.h009.n1024e5231  
pace.h011.n1024e5480  
pace.h013.n2048e5216  
pace.h015.n2048e7005  
pace.h017.n1024e9737  
pace.h019.n1024e10284  
pace.h021.n2048e10573  
pace.h023.n2048e11098  
pace.h025.n1024e15102  
pace.h027.n2048e14738  
pace.h029.n1024e19861  
pace.h031.n2048e18876  
pace.h033.n6301e20777  
pace.h035.n8192e30498  
pace.h037.n8192e30814  
pace.h039.n9473e43691  
pace.h041.n8192e45139  
pace.h043.n8192e50901  
pace.h045.n8192e54189  
pace.h047.n8192e54807  
pace.h049.n8192e57468  
pace.h051.n8192e71995  
pace.h053.n8192e73399  
pace.h055.n15268e74339  
pace.h057.n8192e83083  
pace.h059.n7115e103689  
pace.h061.n8192e112798  
pace.h063.n36682e88328  
pace.h065.n26475e106762  
pace.h067.n8192e138733  
pace.h069.n8192e156904  
pace.h071.n8192e168714  
pace.h073.n16384e166938  
pace.h075.n34978e169522  
pace.h077.n62586e147892  
pace.h079.n16384e216334  
pace.h081.n32768e224254  
pace.h083.n32768e256256  
pace.h085.n16384e283794  
pace.h087.n58960e269439  
pace.h089.n8192e358201  
pace.h091.n16384e354031  
pace.h093.n32768e363878  
pace.h095.n32768e409363  
pace.h097.n16384e458925  
pace.h099.n97898e447979  
pace.h101.n65536e618237  
pace.h103.n159316e544621  
pace.h105.n65536e652583  
pace.h107.n32768e699385  
pace.h109.n8192e839576  
pace.h111.n77360e828161  
pace.h113.n194085e854377  
pace.h115.n262111e1234877  
pace.h117.n325729e1469679  
pace.h119.n342573e1619304  
pace.h121.n16384e2076651  
pace.h123.n4096e2097152  
pace.h125.n16384e2097152  
pace.h127.n32768e2128971  
pace.h129.n32768e2148521  
pace.h131.n16384e2194576  
pace.h133.n131072e2130456  
pace.h135.n32768e2235835  
pace.h137.n32768e2241646  
pace.h139.n16384e2293868  
pace.h141.n65536e2321780  
pace.h143.n32768e2412833  
pace.h145.n32768e2416820  
pace.h147.n32768e2422154  
pace.h149.n131072e2340382  
pace.h151.n65536e2457973  
pace.h153.n32768e2554765  
pace.h155.n131072e2459701  
pace.h157.n281903e2312497  
pace.h159.n32768e2621362  
pace.h161.n32768e2684883  
pace.h163.n32768e2696696  
pace.h165.n131072e2622100  
pace.h167.n65536e2731506  
pace.h169.n32768e2808603  
pace.h171.n131072e2741624  
pace.h173.n32768e2843144  
pace.h175.n131072e2757784  
pace.h177.n131072e2784435  
pace.h179.n131072e2921009  
pace.h181.n131072e2950732  
pace.h183.n131072e3115621  
pace.h185.n32768e3222505  
pace.h187.n131072e3225441  
pace.h189.n262144e3100310  
pace.h191.n131072e3275806  
pace.h193.n32768e3734348  
pace.h195.n4096e4194304  
pace.h197.n8192e4194304  
pace.h199.n875713e5105039  

<details style="border: 1px solid #aaa; border-radius: 4px;"><summary>⯈ 隐藏算例</summary>
pace.e001.n512e651  
pace.e003.n1024e6055  
pace.e005.n1024e5736  
pace.e007.n1024e4198  
pace.e009.n1024e6804  
pace.e011.n4096e15076  
pace.e013.n2048e7038  
pace.e015.n2048e9390  
pace.e017.n1024e3800  
pace.e019.n2048e11718  
pace.e021.n2048e6147  
pace.e023.n2048e5400  
pace.e025.n1024e2802  
pace.e027.n2048e9406  
pace.e029.n2048e11302  
pace.e031.n4096e26071  
pace.e033.n1024e2010  
pace.e035.n4096e17074  
pace.e037.n4096e28675  
pace.e039.n1024e2570  
pace.e041.n4096e27082  
pace.e043.n2048e5700  
pace.e045.n4096e30202  
pace.e047.n4096e28612  
pace.e049.n4096e23857  
pace.e051.n4096e12946  
pace.e053.n4096e26872  
pace.e055.n4096e12085  
pace.e057.n4096e12298  
pace.e059.n1024e1124  
pace.e061.n2048e4320  
pace.e063.n4096e23766  
pace.e065.n8192e48759  
pace.e067.n2048e4241  
pace.e069.n4096e22368  
pace.e071.n2048e4896  
pace.e073.n8192e51796  
pace.e075.n8192e31504  
pace.e077.n8192e54744  
pace.e079.n8192e52154  
pace.e081.n8192e51030  
pace.e083.n2048e3537  
pace.e085.n8192e69446  
pace.e087.n8192e69372  
pace.e089.n8192e55556  
pace.e091.n2048e3364  
pace.e093.n16384e109487  
pace.e095.n8192e59029  
pace.e097.n16384e103725  
pace.e099.n8192e38175  
pace.e101.n32768e246723  
pace.e103.n16384e77536  
pace.e105.n2048e10584  
pace.e107.n16384e104833  
pace.e109.n16384e149298  
pace.e111.n2048e31904  
pace.e113.n1024e25750  
pace.e115.n32768e304304  
pace.e117.n32768e304790  
pace.e119.n32768e272707  
pace.e121.n1024e5480  
pace.e123.n32768e256256  
pace.e125.n32768e288748  
pace.e127.n2048e25492  
pace.e129.n32768e256665  
pace.e131.n32768e240240  
pace.e133.n2048e22332  
pace.e135.n32768e240816  
pace.e137.n2048e7003  
pace.e139.n65536e288492  
pace.e141.n65536e303686  
pace.e143.n65536e303696  
pace.e145.n2048e27086  
pace.e147.n1024e25052  
pace.e149.n1024e3624  
pace.e151.n65536e653240  
pace.e153.n2048e22306  
pace.e155.n65536e619239  
pace.e157.n65536e652583  
pace.e159.n65536e550097  
pace.e161.n2048e28713  
pace.e163.n131072e574081  
pace.e165.n131072e603932  
pace.e167.n1024e5487  
pace.e169.n131072e604300  
pace.e171.n131072e604042  
pace.e173.n1024e23175  
pace.e175.n2048e22520  
pace.e177.n32768e320320  
pace.e179.n2048e27346  
pace.e181.n32768e321088  
pace.e183.n1024e5471  
pace.e185.n4096e62879  
pace.e187.n1024e3632  
pace.e189.n1024e2048  
pace.e191.n2048e4143  
pace.e193.n1024e17144  
pace.e195.n1024e32768  
pace.e197.n4096e93346  
pace.e199.n16384e559996  
</details>
