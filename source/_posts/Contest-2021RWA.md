---
title: SmartLab Challenge 2021 - Routing and Wavelength Assignment
date: 2021-07-10 17:20:58
categories:
- 算法挑战
tags:
- 算法挑战
- 组合优化
---
路由与波长分配问题是通信领域中的重要问题, 具有较高的实际应用价值.
路由与波长分配问题主要研究如何为通信业务规划传输路径并分配波长, 使得所有业务可以在互不干扰的情况下传输.
高效的路由与波长分配问题的求解算法对提高网络的传输效率具有意义重大.
同时, 路由与波长分配问题还与多智能体路径规划以及芯片布线十分相似, 具有重要的理论意义.



# 路由与波长分配算法训练

## 问题概述

给定一个有向图, 以及一系列通信业务, 每个通信业务有固定的起点和终点.
请为每个通信业务规划一条传输路径, 并为其分配一个波长, 在确保每条有向边上经过的所有业务波长互不相同的前提下, 最小化使用的波长数.

- 参考文献.
  - [1] Y. Fang, Z. Lü, Z. Su, Y. Wang, T. Zhang, and Q. Zhang, “Local Search based on a New Neighborhood for Routing and Wavelength Assignment,” in 2020 IEEE International Conference on Systems, Man, and Cybernetics (SMC), 2020, pp. 1123–1128. doi: 10.1109/SMC42975.2020.9283031.


## 命令行参数

请大家编写程序时支持四个命令行参数, 依次为算例文件路径, 输出解文件路径, 运行时间上限 (单位为秒) 和随机种子 (0-65535).
例如, 在控制台运行以下命令表示调用可执行文件 `rwa.exe` 求解路径为 `../data/ATT.n90e274t359.txt` 的算例, 解文件输出至 `ATT.n90e274t359.txt`, 限时 300 秒, 随机种子为 12345:
```
rwa.exe ../data/ATT.n90e274t359.txt ATT.n90e274t359.txt 300 12345
```

- 运行时间上限.
  - 超出运行时间上限后测试程序会强行终止算法, 请确保在此之前已保存解文件 (最好还能自行正常退出).
    - 可以每次迭代时检查是否超时, 也可以每次更新最优解后保存一次.
- 随机种子设置.
  - 使用 C 语言随机数生成器请用 `srand`.
  - 使用 C++ 随机数生成器 (如 `mt19937`) 请在构造时传参或调用 `seed()` 方法设置.
- 测试时可能会修改算例文件名，请勿针对文件名做特殊处理.


## 输入的算例文件格式

所有算例的节点从 0 开始连续编号.

第一行给出三个由空白字符分隔的整数, 分别表示节点数 N, 有向边数 E, 以及通信业务数 T.
接下来连续 E 行, 每行包含两个由空白字符分隔的整数, 第 i 行表示第 i 条有向边的源点和宿点.
接下来连续 T 行, 每行包含两个由空白字符分隔的整数, 第 i 行表示第 i 个通信业务的起点和终点.

例如, 以下算例文件表示在 4 个节点和 5 条有向边的有向图上传输 3 个通信业务; 其中,  
有向图包含 0, 1, 2, 3 共 4 个节点以及 0-1, 1-2, 2-3, 3-0, 1-0 共 4 条有向边,  
第一个通信业务的起点为 0 终点为 1,  
第二个通信业务的起点为 1 终点为 0,  
第二个通信业务的起点为 1 终点为 3:
```
4 5 3
0 1
1 2
2 3
3 0
1 0
0 1
1 0
1 3
```


## 输出的解文件格式

输出 T 行整数表示 T 个通信业务的传输方案, 第 i 行表示第 i 个通信业务的波长分配和传输路径.
每一行第一个整数表示第 i 个通信业务使用的波长, 第二个整数表示传输路径上的节点数, 随后连续 P 个由空白字符分隔的整数表示传输路径上依次经过的节点.

波长可以取 `int` 范围内任意整数, 检查程序自动统计不同的整数的数量.
传输路径上的节点若不包含通信业务的起点和终点, 检查程序将自动将其添加至路径中.

例如, 以下解文件表示 3 个通信业务的波长分配和传输路径; 其中,  
通信业务 0 使用的波长为 1, 依次经过节点 0 和 1;  
通信业务 1 使用的波长为 1, 直接从起点 1 到达终点 0;  
通信业务 2 使用的波长为 0, 依次经过节点 1 和 2, 最后到达终点 3:
```
1 2 0 1
1 0
0 2 1 2
```


## 提交要求

- 发送至邮箱 [zhouxing.su@qq.com](mailto:zhouxing.su@qq.com).
- 邮件标题格式为 "**Challenge2021RWA-姓名-学校-专业**".
- 邮件附件为单个压缩包 (文件大小 2M 以内), 文件名为 "**姓名-学校-专业**", 其内包含下列文件.
  - 算法的可执行文件 (Windows 平台).
    - 用 g++ 的同学编译时请静态链接, 即添加 `-static-libgcc -static-libstdc++` 编译选项.
    - 勿读取键盘输入 (包括最后按任意键退出), 否则所有算例的运行时间全部自动记为运行时间上限.
  - 算法源码.
  - 算法在各算例上的运行情况概要, 至少包括以下几项信息.
    - 算例名.
    - 使用的波长数.
    - 计算耗时.
  - 算法在各算例上求得的使用波长数最少的解文件 (可选, 仅在自动测试程序无法成功调用算法输出可通过检查程序的解文件时作为参考).

例如:
```
苏宙行-华科大-计科.zip
|   rwa.exe
|   results.csv
|
+---src
|       main.cpp
|       algorithm.cpp
|       algorithm.h
|
+---results
        ATT.n90e274t359.txt
        ATT2.n71e350t2918.txt
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


namespace RwaBenchmark {
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
            Dictionary<string, Dictionary<string, HashSet<string>>> edges = new Dictionary<string, Dictionary<string, HashSet<string>>>();
            List<string[]> traffics = new List<string[]>();
            try { // load instance.
                string[] lines = File.ReadAllLines(inputFilePath);
                string[] nums = lines[0].Split(InlineDelimiters, StringSplitOptions.RemoveEmptyEntries);
                int nodeNum = int.Parse(nums[0]);
                int edgeNum = int.Parse(nums[1]);
                int trafficNum = int.Parse(nums[2]);
                int l = 1;
                for (int n = 0; (n < edgeNum) && (l < lines.Length); ++l, ++n) {
                    string[] nodes = lines[l].Split(InlineDelimiters, StringSplitOptions.RemoveEmptyEntries);
                    edges.TryAdd(nodes[0], new Dictionary<string, HashSet<string>>());
                    edges[nodes[0]].Add(nodes[1], new HashSet<string>());
                }

                traffics.Capacity = trafficNum;
                for (int t = 0; (t < trafficNum) && (l < lines.Length); ++l, ++t) {
                    traffics.Add(lines[l].Split(InlineDelimiters, StringSplitOptions.RemoveEmptyEntries));
                }
            } catch (Exception) { }

            int brokenPathNum = 0;
            int conflictNum = 0;
            HashSet<string> colors = new HashSet<string>();
            try { // load solution and check.
                string[] lines = File.ReadAllLines(outputFilePath);
                for (int l = 0; l < lines.Length; ++l) {
                    List<string> nums = lines[l].Split(InlineDelimiters, StringSplitOptions.RemoveEmptyEntries).ToList();
                    nums.Add(traffics[l][1]);
                    string color = nums[0];
                    colors.Add(color);
                    string src = traffics[l][0];
                    for (int i = 2; i < nums.Count; ++i) {
                        string dst = nums[i];
                        if (dst == src) { continue; }
                        if (!edges.ContainsKey(src) || !edges[src].ContainsKey(dst)) { ++brokenPathNum; break; }
                        if (edges[src][dst].Contains(color)) { ++conflictNum; }
                        edges[src][dst].Add(color);
                        src = nums[i];
                    }
                }
            } catch (Exception) { }

            Console.Write("instance=");
            Console.Write(Path.GetFileName(inputFilePath));

            Console.Write(" waveLenNum=");
            Console.Write(colors.Count);

            Console.Write(" brokenPathNum=");
            Console.Write(brokenPathNum);

            Console.Write(" conflictNum=");
            Console.Write(conflictNum);
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

[下载全部](https://gitee.com/suzhouxing/techive/attach_files/767944/download/rwa.7z)

ATT.n90e274t359  
ATT2.n71e350t2918  
brasil.n27e140t1370  
EON.n20e78t373  
Finland.n31e102t930  
NSF-01.n14e42t284  
NSF-03.n14e42t285  
NSF-12.n14e42t551  
NSF-48.n14e42t547  
NSF2-01.n14e44t284  
NSF2-03.n14e44t285  
NSF2-12.n14e44t551  
NSF2-48.n14e44t547  
Y3-020-1.n100e344t1975  
Y3-020-2.n100e368t1961  
Y3-020-3.n100e356t2055  
Y3-020-4.n100e354t1989  
Y3-020-5.n100e350t1975  
Y3-040-1.n100e344t3894  
Y3-040-2.n100e368t3956  
Y3-040-3.n100e356t4012  
Y3-040-4.n100e354t3959  
Y3-040-5.n100e350t3996  
Y3-060-1.n100e344t5967  
Y3-060-2.n100e368t5907  
Y3-060-3.n100e356t5989  
Y3-060-4.n100e354t5932  
Y3-060-5.n100e350t5990  
Y3-080-1.n100e344t7959  
Y3-080-2.n100e368t7911  
Y3-080-3.n100e356t7987  
Y3-080-4.n100e354t7908  
Y3-080-5.n100e350t7924  
Y3-100-1.n100e344t9900  
Y3-100-2.n100e368t9900  
Y3-100-3.n100e356t9900  
Y3-100-4.n100e354t9900  
Y3-100-5.n100e350t9900  
Y4-020-1.n100e440t1975  
Y4-020-2.n100e460t1961  
Y4-020-3.n100e436t2055  
Y4-020-4.n100e442t1989  
Y4-020-5.n100e430t1975  
Y4-040-1.n100e440t3894  
Y4-040-2.n100e460t3956  
Y4-040-3.n100e436t4012  
Y4-040-4.n100e442t3959  
Y4-040-5.n100e430t3996  
Y4-060-1.n100e440t5967  
Y4-060-2.n100e460t5907  
Y4-060-3.n100e436t5989  
Y4-060-4.n100e442t5932  
Y4-060-5.n100e430t5990  
Y4-080-1.n100e440t7959  
Y4-080-2.n100e460t7911  
Y4-080-3.n100e436t7987  
Y4-080-4.n100e442t7908  
Y4-080-5.n100e430t7924  
Y4-100-1.n100e440t9900  
Y4-100-2.n100e460t9900  
Y4-100-3.n100e436t9900  
Y4-100-4.n100e442t9900  
Y4-100-5.n100e430t9900  
Y5-020-1.n100e570t1975  
Y5-020-2.n100e504t1961  
Y5-020-3.n100e582t2055  
Y5-020-4.n100e548t1989  
Y5-020-5.n100e568t1975  
Y5-040-1.n100e570t3894  
Y5-040-2.n100e504t3956  
Y5-040-3.n100e582t4012  
Y5-040-4.n100e548t3959  
Y5-040-5.n100e568t3996  
Y5-060-1.n100e570t5967  
Y5-060-2.n100e504t5907  
Y5-060-3.n100e582t5989  
Y5-060-4.n100e548t5932  
Y5-060-5.n100e568t5990  
Y5-080-1.n100e570t7959  
Y5-080-2.n100e504t7911  
Y5-080-3.n100e582t7987  
Y5-080-4.n100e548t7908  
Y5-080-5.n100e568t7924  
Y5-100-1.n100e570t9900  
Y5-100-2.n100e504t9900  
Y5-100-3.n100e582t9900  
Y5-100-4.n100e548t9900  
Y5-100-5.n100e568t9900  
Z04x25-020.n100e400t1975  
Z04x25-040.n100e400t3894  
Z04x25-060.n100e400t5967  
Z04x25-080.n100e400t7959  
Z04x25-100.n100e400t9900  
Z05x20-020.n100e400t1975  
Z05x20-040.n100e400t3894  
Z05x20-060.n100e400t5967  
Z05x20-080.n100e400t7959  
Z05x20-100.n100e400t9900  
Z06x17-020.n102e408t1975  
Z06x17-040.n102e408t3894  
Z06x17-060.n102e408t5967  
Z06x17-080.n102e408t7959  
Z06x17-100.n102e408t10302  
Z08x13-020.n104e416t1975  
Z08x13-040.n104e416t3894  
Z08x13-060.n104e416t5967  
Z08x13-080.n104e416t7959  
Z08x13-100.n104e416t10712  
Z10x10-020.n100e400t1975  
Z10x10-040.n100e400t3894  
Z10x10-060.n100e400t5967  
Z10x10-080.n100e400t7959  
Z10x10-100.n100e400t9900  
