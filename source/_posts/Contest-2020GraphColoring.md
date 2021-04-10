---
title: SmartLab Challenge 2020 - Graph Coloring
date: 2020-12-09 10:28:35
categories:
- 算法挑战
tags:
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
高效的图着色问题求解算法具有及其重要的理论与应用价值.



# 图着色算法训练

## 命令行参数

请大家编写程序时支持四个命令行参数, 分别为算例文件路径, 输出解文件路径, 颜色数和随机种子.
后续我们可能会通过在控制台运行 `你的算法.exe 输入算例路径 输出解文件路径 颜色数 随机种子` 测试大家提交的算法, 例如:
```
gcp.exe ../DSJC500.5.col dsjc500.5.txt 48 123456
```

- 随机种子设置.
  - 使用 C 语言随机数生成器请用 `srand`.
  - 使用 C++ 随机数生成器 (如 `mt19937`) 请在构造时传参或调用 `seed()` 方法设置.
- 测试时可能会修改算例文件名，请勿针对文件名做特殊处理.


## 输入的算例文件格式

DIMACS 图着色算例格式.
建议读取算例时不要根据开头给出的边数进行相关数据的初始化, 而是根据是否已经读到文件末尾自动判断.


## 输出的解文件格式

对于 N 个节点的算例, 输出 N 行.
每一行输出用空格分隔的两个数字, 分别表示算例中的节点编号以及为该节点分配的颜色.

颜色可以取 `int` 范围内任意整数, 检查程序自动统计不同的整数的数量.

例如, 以下解文件表示 1 号节点染颜色 0, 2 号节点染颜色 2, 3 号节点染颜色 1:
```
1 0 
2 2 
3 1
```


## 提交要求

- 发送至邮箱 [su.zhouxing@qq.com](mailto:su.zhouxing@qq.com).
- 邮件标题格式为 "**Challenge2020GCP-姓名-班级**".
- 邮件附件为单个压缩包, 文件名为 "**姓名-班级**", 其内包含下列文件.
  - 算法的可执行文件 (Windows 平台).
    - 用 g++ 的同学编译时请静态链接, 即添加 `-static-libgcc -static-libstdc++` 编译选项.
    - 用 visual studio 2017 以上版本的同学编译时请静态链接, 即添加 `\MT` 编译选项.
    - 勿读取键盘输入 (包括最后按任意键退出), 否则所有算例的运行时间全部自动记为运行时间上限.
  - 算法源码.
  - 算法在各算例上的运行情况概要, 至少包括以下几项信息.
    - 算例名.
    - 颜色数.
    - 计算耗时.
  - 算法在各算例上求得的颜色数最少的解文件 (可选, 仅在自动测试程序无法成功调用算法输出可通过检查程序的解文件时作为参考).


## 测试与检查程序

我们可能会使用以下 c# 程序检查大家提交的算法和结果 (仅供参考).

```cs
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Text;
using System.Diagnostics;


namespace GcpBenchmark {
    class Program {
        static void Main(string[] args) {
            string inputFilePath = args[0]; // instance file.
            string outputFilePath = args[1]; // solution file.

            if (args.Length > 3) {
                string exeFilePath = args[2]; // algorithm executable file.
                string colorNum = args[3];
                benchmark(inputFilePath, outputFilePath, exeFilePath, colorNum);
            } else {
                check(inputFilePath, outputFilePath);
            }
        }

        struct Edge {
            public string src;
            public string dst;
        };

        static void check(string inputFilePath, string outputFilePath) {
            int nodeNum = 0;
            int edgeNum = 0;
            List<Edge> edges = new List<Edge>();
            try {
                string[] lines = File.ReadAllLines(inputFilePath);

                foreach (string line in lines) {
                    if (line.Length <= 0) { continue; }
                    if (line[0] == 'c') { continue; }

                    string[] cells = line.Split(' ');
                    if (line[0] == 'p') {
                        nodeNum = int.Parse(cells[2]);
                        edgeNum = int.Parse(cells[3]);
                        edges.Capacity = edgeNum;
                    } else if (line[0] == 'e') {
                        edges.Add(new Edge { src = cells[1], dst = cells[2] });
                    }
                }
            } catch (Exception) { }

            HashSet<string> colors = new HashSet<string>();
            Dictionary<string, string> nodeColors = new Dictionary<string, string>();
            try {
                string[] lines = File.ReadAllLines(outputFilePath);

                foreach (string line in lines) {
                    string[] cells = line.Split(' ');
                    nodeColors[cells[0]] = cells[1];
                    colors.Add(cells[1]);
                }
            } catch (Exception) { }

            int conflictNum = 0;
            try {
                foreach (Edge edge in edges) {
                    if (nodeColors[edge.src] == nodeColors[edge.dst]) { ++conflictNum; }
                }
            } catch (Exception) { }

            Console.Write("instance=");
            Console.Write(Path.GetFileName(inputFilePath));

            Console.Write(" color=");
            Console.Write(colors.Count);

            Console.Write(" conflict=");
            Console.WriteLine(conflictNum);
        }

        static void benchmark(string inputFilePath, string outputFilePath, string exeFilePath, string colorNum) {
            const int Repeat = 10;
            const int millisecondCheckInterval = 1000;

            int millisecondTimeLimit = 20 * 60 * 1000;
            long byteMemoryLimit = 1024 * 1024 * 1024;

            for (int i = 0; i < Repeat; ++i) {
                try { File.Delete(outputFilePath); } catch (Exception) { }
                try {
                    int seed = genSeed();
                    StringBuilder cmdArgs = new StringBuilder();
                    cmdArgs.Append(inputFilePath).Append(" ").Append(outputFilePath)
                        .Append(" ").Append(colorNum).Append(" ").Append(seed);

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

算例规模从小到大依次为 (求解难度不一定随规模增加, 但 DSJC500.5 以前的算例应该都很容易求解):

[下载全部](coloring.data.7z)

DSJC125.1
DSJC125.5
DSJC125.9
DSJC250.1
DSJC250.5
DSJC250.9
DSJC500.1
DSJC500.5
DSJC500.9
DSJC1000.1
DSJC1000.5
DSJC1000.9
