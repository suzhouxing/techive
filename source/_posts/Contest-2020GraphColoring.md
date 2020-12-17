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
- 邮件标题格式为 "Challenge2020GCP-姓名-班级".
- 邮件附件为单个压缩包, 文件名为 "姓名-班级", 其内包含下列文件.
  - 算法的可执行文件.
  - 算法源码.
  - 算法在各算例上的运行情况概要, 至少包括以下几项信息.
    - 算例名.
    - 颜色数.
    - 计算耗时.
  - 算法在各算例上求得的颜色数最少的解文件.


## 检查程序

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
            {
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
            }

            HashSet<string> colors = new HashSet<string>();
            Dictionary<string, string> nodeColors = new Dictionary<string, string>();
            {
                string[] lines = File.ReadAllLines(outputFilePath);

                foreach (string line in lines) {
                    string[] cells = line.Split(' ');
                    nodeColors[cells[0]] = cells[1];
                    colors.Add(cells[1]);
                }
            }

            int conflictNum = 0;
            foreach (Edge edge in edges) {
                if (nodeColors[edge.src] == nodeColors[edge.dst]) { ++conflictNum; }
            }

            Console.Write("instance=");
            Console.Write(Path.GetFileName(inputFilePath));

            Console.Write(" color=");
            Console.Write(colors.Count);

            Console.Write(" conflict=");
            Console.WriteLine(conflictNum);
        }

        static void benchmark(string inputFilePath, string outputFilePath, string exeFilePath, string colorNum) {
            const int Repeat = 10;
            const int MillisecondTimeLimit = 10 * 60 * 1000;
            IntPtr ByteMemoryLimit = new IntPtr(1024 * 1024 * 1024);

            for (int i = 0; i < Repeat; ++i) {
                int seed = genSeed();
                StringBuilder cmdArgs = new StringBuilder();
                cmdArgs.Append(inputFilePath).Append(" ").Append(outputFilePath).Append(" ")
                    .Append(colorNum).Append(" ").Append(seed);

                Stopwatch sw = new Stopwatch();
                sw.Start();
                ProcessStartInfo psi = new ProcessStartInfo();
                psi.FileName = exeFilePath;
                psi.WorkingDirectory = Environment.CurrentDirectory;
                psi.Arguments = cmdArgs.ToString();
                Process p = Process.Start(psi);
                p.MaxWorkingSet = ByteMemoryLimit;
                if (!p.WaitForExit(MillisecondTimeLimit)) {
                    try { p.Kill(); } catch (Exception) { }
                }
                sw.Stop();

                Console.Write("time=");
                Console.Write(sw.ElapsedMilliseconds / 1000.0);
                Console.Write("s seed=");
                Console.Write(seed);
                Console.Write(" ");

                check(inputFilePath, outputFilePath);
            }
        }

        static int genSeed() {
            return (int)(DateTime.Now.Ticks & 0xffff);
        }
    }
}
```
