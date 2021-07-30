---
title: SmartLab Challenge 2020 - p-Center and Unicost Set Covering
date: 2020-12-22 10:19:15
categories:
- 算法挑战
tags:
- 算法挑战
- 组合优化
---
中心选址问题是通信与物流等领域中的重要问题, 其背后的单一成本集合覆盖问题更是用途广泛.
中心选址问题和单一成本集合覆盖问题主要研究如何使用有限的资源提供尽可能高质量的服务的问题.
例如, 移动基站, 消防站, 物流仓库, 数据中心等众多设施的选址都可以建模为中心选址问题.
高效的中心选址或单一成本集合覆盖问题的求解算法具有极其重要的理论与应用价值.



# 中心选址与单一成本集合覆盖算法训练

## 问题概述

给定一系列元素与若干子集, 请选择给定数量的子集, 使其并集等于所有元素的全集.

- 参考文献.
  - [1] Z. Su, Q. Zhang, Z. Lü, C.-M. Li, W. Lin, and F. Ma, “Weighting-based Variable Neighborhood Search for Optimal Camera Placement,” Proceedings of the AAAI Conference on Artificial Intelligence, vol. 35, no. 14, pp. 12400–12408, 2021.
  - [2] Q. Zhang, Z. Lü, Z. Su, C. Li, Y. Fang, and F. Ma, “Vertex Weighting-Based Tabu Search for p-Center Problem,” in Proceedings of the Twenty-Ninth International Joint Conference on Artificial Intelligence, IJCAI-20, 2020, pp. 1481–1487. doi: 10.24963/ijcai.2020/206.


## 命令行参数

请大家编写程序时支持四个命令行参数, 依次为算例文件路径, 输出解文件路径, 运行时间上限 (单位为秒) 和随机种子 (0-65535).
例如, 在控制台运行以下命令表示调用可执行文件 `uscp.exe` 求解路径为 `../data/pmed1.n100p5.txt` 的算例, 解文件输出至 `sln.pmed1.n100p5.txt`, 限时 1000 秒, 随机种子为 12345:
```
uscp.exe ../data/pmed1.n100p5.txt sln.pmed1.n100p5.txt 1000 12345
```

- 运行时间上限.
  - 超出运行时间上限后测试程序会强行终止算法, 请确保在此之前已保存解文件 (最好还能自行正常退出).
- 随机种子设置.
  - 使用 C 语言随机数生成器请用 `srand`.
  - 使用 C++ 随机数生成器 (如 `mt19937`) 请在构造时传参或调用 `seed()` 方法设置.
- 测试时可能会修改算例文件名，请勿针对文件名做特殊处理.


## 输入的算例文件格式

所有算例均已根据给定覆盖半径转换为判定问题, 处理为一系列固定集合数的单一成本集合覆盖算例, 并附有逐步缩小半径时新增的无法覆盖的节点的信息.
转换后每个节点都可以覆盖若干节点, 同时也对称地被若干节点覆盖, 故转换后的单一成本集合覆盖算例中集合数等于元素数.

所有算例的元素和集合分别从 0 开始连续编号.

第一行给出两个由空白字符分隔的整数 N 和 P, 分别表示节点数和中心数 (从集合覆盖的角度来看, N 既是集合数又是元素数, P 为可挑选出的集合数).

接下来每两行一组, 连续 N 组给出每个集合的覆盖范围.
每组中第一行为该集合能覆盖的元素数量 C, 第二行为空白字符分隔的 C 个数字, 分别表示该集合能覆盖的元素的编号.

然后给出两个由空白字符整数 U 和 L, 分别表示覆盖半径边长序号的上界和下界 (其中上界即前面给出的判定问题对应的覆盖半径的边长序号, 下界为估计值).

接下来连续 U - L 行给出每缩小一次覆盖半径新增的无法覆盖的元素的信息 (若 U = L = 0, 说明前面给出的判定问题对应的覆盖半径已经为最优半径, 缩小半径后已不存在可行解).
每行第一个整数 K 表示本次缩小半径将导致 K 个元素不再被某个集合覆盖, 随后连续 K 个由空白字符分隔的整数 S 表示集合 S 将新增一个无法覆盖的元素 (同一个集合可能重复出现多次).
注意, 在前面给出的判定问题数据中, 每个集合可覆盖的元素已按从近到远的顺序排序, 即每次只用将集合 S 可覆盖元素列表末尾的一个元素删除即可.

例如, 以下算例文件表示集合和元素的数量均为 4, 要求挑选出 2 个集合覆盖所有元素; 其中,  
集合 0 可以覆盖 2 个元素, 分别为元素 0 和 3;  
集合 1 可以覆盖 2 个元素, 分别为元素 1 和 2;  
集合 2 可以覆盖 3 个元素, 分别为元素 1, 2 和 3;  
集合 3 可以覆盖 2 个元素, 分别为元素 0 和 2;  
覆盖半径边长序号的上界为 5 下界为 3; 其中,  
半径缩小为第 4 短的边时, 集合 2 无法再覆盖最远的元素 3, 集合 3 无法再覆盖最远的元素 2;  
半径缩小为第 3 短的边时, 集合 1 无法再覆盖最远的元素 2, 集合 2 无法再覆盖最远的元素 1:
```
4 2
2
0 3
2
1 2
3
1 2 3
2
0 2
5 3
2	2 3
2	1 2
```


## 输出的解文件格式

输出一行用空白字符分隔的 P 个整数, 分别表示挑选出的 P 个中心 (集合).

例如, 以下解文件表示选择节点 0 和 2 作为中心 (集合):
```
0 2
```


## 提交要求

- 发送至邮箱 [zhouxing.su@qq.com](mailto:zhouxing.su@qq.com).
- 邮件标题格式为 "**Challenge2020USCP-姓名-学校-专业**".
- 邮件附件为单个压缩包 (文件大小 2M 以内), 文件名为 "**姓名-学校-专业**", 其内包含下列文件.
  - 算法的可执行文件 (Windows 平台).
    - 用 g++ 的同学编译时请静态链接, 即添加 `-static-libgcc -static-libstdc++` 编译选项.
    - 勿读取键盘输入 (包括最后按任意键退出), 否则所有算例的运行时间全部自动记为运行时间上限.
  - 算法源码.
  - 算法在各算例上的运行情况概要, 至少包括以下几项信息.
    - 算例名.
    - 剩余未覆盖元素数.
    - 计算耗时.
  - 算法在各算例上求得的完全覆盖的解文件 (可选, 仅在自动测试程序无法成功调用算法输出可通过检查程序的解文件时作为参考).

例如:
```
苏宙行-华科大-计科.zip
|   pcenter.exe
|   results.csv
|
+---src
|       main.cpp
|       algorithm.cpp
|       algorithm.h
|
+---results
        pmed1.n100p5.txt
        pmed2.n100p10.txt
        ...
```


## 检查程序

我们可能会使用以下 c# 程序检查大家提交的算法和结果 (仅供参考).

```cs
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Text;
using System.Diagnostics;


namespace UscpBenchmark {
    class Program {
        static readonly char[] InlineDelimiters = new char[] { ' ', '\t' };
        static readonly char[] WhiteSpaceChars = new char[] { ' ', '\t', '\r', '\n' };

        static void Main(string[] args) {
            string inputFilePath = args[0]; // instance file.
            string outputFilePath = args[1]; // solution file.

            if (args.Length > 3) {
                string exeFilePath = args[2]; // algorithm executable file.
                string secTimeout = args[3]; // timeout in second.
                benchmark(inputFilePath, outputFilePath, exeFilePath, secTimeout);
            } else {
                check(inputFilePath, outputFilePath);
            }
        }

        static void check(string inputFilePath, string outputFilePath) {
            int nodeNum = 0;
            int centerNum = 0;
            int maxRank = 0;
            int minRank = 0;
            List<List<int>> sets = new List<List<int>>();
            List<List<int>> setsWithdrops = new List<List<int>>();
            try { // load instance.
                string[] lines = File.ReadAllLines(inputFilePath);

                string[] cells = lines[0].Split(InlineDelimiters, StringSplitOptions.RemoveEmptyEntries);
                nodeNum = int.Parse(cells[0]);
                centerNum = int.Parse(cells[1]);
                int l = 1;
                for (sets.Capacity = nodeNum; (l < lines.Length) && (sets.Count < nodeNum); ++l) {
                    int coveredItemNum = int.Parse(lines[l]);
                    cells = lines[++l].Split(InlineDelimiters, StringSplitOptions.RemoveEmptyEntries);
                    List<int> set = new List<int>(coveredItemNum);
                    foreach (var cell in cells) { set.Add(int.Parse(cell)); }
                    sets.Add(set);
                }

                cells = lines[l++].Split(InlineDelimiters, StringSplitOptions.RemoveEmptyEntries);
                maxRank = int.Parse(cells[0]);
                minRank = int.Parse(cells[1]);
                for (setsWithdrops.Capacity = maxRank - minRank; l < lines.Length; ++l) {
                    cells = lines[l].Split(InlineDelimiters, StringSplitOptions.RemoveEmptyEntries);
                    List<int> setsWithdrop = new List<int>(cells.Length - 1);
                    for (int c = 1; c < cells.Length; ++c) { setsWithdrop.Add(int.Parse(cells[c])); }
                    setsWithdrops.Add(setsWithdrop);
                } // TODO[szx][0]: handle empty lines.
            } catch (Exception) { }

            List<int> pickedSets = new List<int>(centerNum);
            List<bool> notPickedSet = new List<bool>(Enumerable.Repeat(true, nodeNum));
            try { // load solution.
                string[] cells = File.ReadAllText(outputFilePath).Split(WhiteSpaceChars, StringSplitOptions.RemoveEmptyEntries);
                foreach (string cell in cells) {
                    int s = int.Parse(cell);
                    pickedSets.Add(s);
                    notPickedSet[s] = false;
                }
            } catch (Exception) { }

            int uncoveredItemNum = nodeNum;
            int rank = maxRank + 1;
            try { // check.
                List<int> coveringSetNumOfItems = new List<int>(Enumerable.Repeat(0, nodeNum));
                foreach (var s in pickedSets) {
                    foreach (var item in sets[s]) {
                        if (coveringSetNumOfItems[item]++ < 1) { --uncoveredItemNum; }
                    }
                }

                if (uncoveredItemNum == 0) {
                    for (int r = 0; (r < setsWithdrops.Count) && (rank > maxRank); ++r) {
                        foreach (var s in setsWithdrops[r]) {
                            if (notPickedSet[s]) { continue; }
                            int dropItem = sets[s].Last();
                            if (--coveringSetNumOfItems[dropItem] < 1) { rank = maxRank - r; break; }
                            sets[s].RemoveAt(sets[s].Count - 1);
                        }
                    }
                }
            } catch (Exception) { }

            Console.Write("instance=");
            Console.Write(Path.GetFileName(inputFilePath));

            Console.Write(" center=");
            Console.Write(pickedSets.Count);

            Console.Write(" uncovered=");
            Console.Write(uncoveredItemNum);

            Console.Write(" rank=");
            Console.WriteLine(rank);
        }

        static void benchmark(string inputFilePath, string outputFilePath, string exeFilePath, string secTimeout) {
            const int Repeat = 10;
            const int millisecondCheckInterval = 1000;

            long millisecondTimeLimit = int.Parse(secTimeout) * 1000;
            long byteMemoryLimit = 1024 * 1024 * 1024;

            for (int i = 0; i < Repeat; ++i) {
                try { File.Delete(outputFilePath); } catch (Exception) { }
                try {
                    int seed = genSeed();
                    StringBuilder cmdArgs = new StringBuilder();
                    cmdArgs.Append(inputFilePath).Append(" ").Append(outputFilePath)
                        .Append(" ").Append(secTimeout).Append(" ").Append(seed);

                    Stopwatch sw = new Stopwatch();
                    sw.Start();
                    ProcessStartInfo psi = new ProcessStartInfo();
                    psi.FileName = exeFilePath;
                    psi.WorkingDirectory = Environment.CurrentDirectory;
                    psi.Arguments = cmdArgs.ToString();
                    Process p = Process.Start(psi);
                    while (!p.WaitForExit(millisecondCheckInterval)
                        && (sw.ElapsedMilliseconds < millisecondTimeLimit)
                        && (p.PrivateMemorySize64 < byteMemoryLimit)) { }
                    try { p.Kill(); } catch (Exception) { }
                    sw.Stop();

                    Console.Write("solver=");
                    Console.Write(Path.GetDirectoryName(exeFilePath));

                    Console.Write(" time=");
                    Console.Write(sw.ElapsedMilliseconds / 1000.0);

                    Console.Write("s seed=");
                    Console.Write(seed);
                    Console.Write(" ");

                    check(inputFilePath, outputFilePath);
                } catch (Exception e) {
                    Console.WriteLine();
                    //Console.WriteLine(e);
                }
            }
        }

        static int genSeed() {
            return (int)(DateTime.Now.Ticks & 0xffff);
        }
    }
}

```


## 算例清单

算例规模从小到大依次为 (求解难度不一定随规模增加, 但除 pcb3038* 以外的算例应该都很容易求解):

[下载全部](https://gitee.com/suzhouxing/techive/attach_files/787061/download/pcenter.7z)

pmed01.n100p5  
pmed02.n100p10  
pmed03.n100p10  
pmed04.n100p20  
pmed05.n100p33  
pmed06.n200p5  
pmed07.n200p10  
pmed08.n200p20  
pmed09.n200p40  
pmed10.n200p67  
pmed11.n300p5  
pmed12.n300p10  
pmed13.n300p30  
pmed14.n300p60  
pmed15.n300p100  
pmed16.n400p5  
pmed17.n400p10  
pmed18.n400p40  
pmed19.n400p80  
pmed20.n400p133  
pmed21.n500p5  
pmed22.n500p10  
pmed23.n500p50  
pmed24.n500p100  
pmed25.n500p167  
pmed26.n600p5  
pmed27.n600p10  
pmed28.n600p60  
pmed29.n600p120  
pmed30.n600p200  
pmed31.n700p5  
pmed32.n700p10  
pmed33.n700p70  
pmed34.n700p140  
pmed35.n800p5  
pmed36.n800p10  
pmed37.n800p80  
pmed38.n900p5  
pmed39.n900p10  
pmed40.n900p90  
u1060p010r2273.08  
u1060p020r1580.80  
u1060p030r1207.77  
u1060p040r1020.56  
u1060p050r904.92  
u1060p060r781.17  
u1060p070r710.75  
u1060p080r652.16  
u1060p090r607.87  
u1060p100r570.01  
u1060p110r538.84  
u1060p120r510.27  
u1060p130r499.65  
u1060p140r452.46  
u1060p150r447.01  
rl1323p010r3077.30  
rl1323p020r2016.40  
rl1323p030r1631.50  
rl1323p040r1352.36  
rl1323p050r1187.27  
rl1323p060r1063.01  
rl1323p070r971.93  
rl1323p080r895.06  
rl1323p090r832.00  
rl1323p100r789.70  
u1817p010r457.91  
u1817p020r309.01  
u1817p030r240.99  
u1817p040r209.45  
u1817p050r184.91  
u1817p060r162.64  
u1817p070r148.11  
u1817p080r136.77  
u1817p090r129.51  
u1817p100r126.99  
u1817p110r109.25  
u1817p120r107.76  
u1817p130r104.73  
u1817p140r101.60  
u1817p150r91.60  
pcb3038p010r729  
pcb3038p020r494  
pcb3038p030r394  
pcb3038p040r337  
pcb3038p050r299  
pcb3038p100r207  
pcb3038p150r165  
pcb3038p200r141  
pcb3038p250r123  
pcb3038p300r116  
pcb3038p350r105  
pcb3038p400r97  
pcb3038p450r89  
pcb3038p500r85  
