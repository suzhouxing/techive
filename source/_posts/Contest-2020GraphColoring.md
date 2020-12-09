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
  - 算法在各算例上的运行情况, 至少包括以下几项信息.
    - 算例名.
    - 颜色数.
    - 计算耗时.


## 检查程序

我们可能会使用以下程序检查大家提交的算法和结果 (仅供参考).

```cpp
#include <iostream>
#include <fstream>
#include <chrono>
#include <string>
#include <sstream>
#include <vector>
#include <set>
#include <map>
#include <random>
#include <cstdlib>


using namespace std;


static int genSeed() { return static_cast<int>(random_device()() & 0xffff); }

static void check(string inputFilePath, string outputFilePath) {
    enum { MaxLineLen = 1024 };

    struct Edge {
        int src;
        int dst;
    };

    int nodeNum, edgeNum;
    vector<Edge> edges;
    {
        char c;
        string s;
        ifstream inputFile(inputFilePath);

        while (inputFile >> c) {
            if (c != 'c') { break; }
            inputFile.ignore(MaxLineLen, '\n');
        }
        inputFile >> c >> s >> nodeNum >> edgeNum;

        edges.resize(edgeNum);
        for (int e = 0; e < edgeNum; ++e) {
            inputFile >> c >> edges[e].src >> edges[e].dst;
        }
    }

    int conflictNum = 0;
    set<int> colors;
    {
        map<int, int> nodeColors;
        ifstream outputFile(outputFilePath);

        for (int n = 0; n < nodeNum; ++n) {
            int node, color;
            outputFile >> node >> color;
            nodeColors[node] = color;
            colors.insert(color);
        }

        for (auto e = edges.begin(); e != edges.end(); ++e) {
            if (nodeColors[e->src] == nodeColors[e->dst]) { ++conflictNum; }
        }
    }

    cout << "instance=" << inputFilePath << "color=" << colors.size()
        << " conflict=" << conflictNum << endl;
}

int main(int argc, char *argv[]) {
    enum { Repeat = 10 };

    string inputFilePath(argv[1]); // instance file.
    string outputFilePath(argv[2]); // solution file.

    if (argc > 3) {
        string exeFilePath(argv[3]); // algorithm executable file.
        string colorNum(argv[4]);

        ostringstream cmd; // https://github.com/lowleveldesign/process-governor
        cmd << "bin/procgov.exe --maxmem=1G --cpu=1 --timeout=10m -r \""
            << exeFilePath << "\" \"" << inputFilePath << "\" \""
            << outputFilePath << "\" " << colorNum << " " << genSeed();

        for (int i = 0; i < Repeat; ++i) {
            chrono::steady_clock::time_point begin = chrono::steady_clock::now();
            system(cmd.str().c_str());
            chrono::steady_clock::time_point end = chrono::steady_clock::now();

            auto duration = chrono::duration_cast<chrono::milliseconds>(end - begin);
            cout << "time=" << (duration.count() / 1000.0) << "s ";

            check(inputFilePath, outputFilePath);
        }
    } else {
        check(inputFilePath, outputFilePath);
    }
}
```
