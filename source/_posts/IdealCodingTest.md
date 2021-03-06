---
title: 理想的编程测试
date: 2018-06-09 10:41:02
categories:
- 技术
tags:
- 技术
- 招聘
- 求职
- 编程
---
以下观点仅针对博士招聘或者高级职位的招聘, 如果只是想招个码农一丝不苟地完成编码任务, 可以忽略此文...

最近参加某公司的博士特招, 收到了的一封邮件说让我做个编程测试.
与普通校招不同, 这封邮件并没有让我去 OJ 平台做题, 而是直接在邮件正文中给了 3 道题让我几天内做完并回复邮件.
这种新颖的形式让我眼前一亮, 虽然最后招聘人员的回复让我发现我想多了, 但是还是想分享一下自己的思考.

这种编程测试有以下几个特点:

- 问题描述模糊
- 有常规求解方法 (有可能在 ACM 比赛中短时间内实现)
- 有高级求解方法
- 线下完成且时间宽裕

这种开放性的命题其实可以考察很多内容.

首先, 因为问题描述存在不严谨的地方, 沟通会非常重要, 换句话说, 这是一种需求分析的意识和能力.
其次, 因为没有限制输入输出数据格式, 需要自己定义接口, 自己确定代码框架.
写出来的代码要足够模块化以适应需求的变更, 包括问题定义的改变和性能要求的提升, 同时接口定义合理方便在其他地方复用.
然后, 没有给定输入输出数据格式肯定更不会给测试用例, 需要自己编写测试用例并且有测试相关的代码, 同时也是使用接口的示例代码.
最后, 编码风格也是可以需要注意的因素之一, 比如命名统一规范且足够自文档, 对功能复杂的模块有适当的注释等等.

举个例子, 这样一道题目 "对一个字符串进行词频统计, 按词频降序排序输出 (word, count)", 你在 OJ 上刷题时会怎么做? 在工作中遇到会怎么做?
这个问题其实给得非常模糊, 如果是我至少会想到以下几个问题:

- 字符串长度是否超过内存容量限制?
- 文件是否只包含 ASCII 码且按照拉丁语系的书写习惯组织词法语法?
- 文件是否包含空白字符以外的非字母字符 (如数字和标点)? 如果包含, 是否仍然严格以空白字符为分隔符分词?
- 单词是否区分大小写?
- 不同单词数量规模有多大? 是否可能超出 int 最大范围?
- 每个单词的出现频率是否超出 int 最大范围?
- 两个单词词频相同怎么办?

对于最复杂的情况, 比如需要做中文分词, 外部排序, 甚至分布式处理, 可能需要用到各种开源库, 代码量甚至可以达到成千上万行...

也许你会说, 理想很丰满现实很骨感.
上面这些好处大家都懂, 但是只要你是一种考核, 就会有针对性的应试技巧.
在线编程测试时间那么短, 可以在一定程度上杜绝抄袭, 但是弄成离线的测试放宽时间限制, 根本不理解算法的人也可以复制粘贴搞出一份代码提交.
首先能敲出代码的人不一定自己真的理解了算法, 甚至觉得自己理解了算法的人也不一定是真的理解了, 很多人对算法的理解并没有达到能证明其正确性和最优性的地步.
其次这种测试考察的是各方面的综合能力, 而不是某个具体问题的解法.
靠死记硬背做出一道题, 换个问题可能就不会做了 (当然, 我们必须承认反复训练可以提升设计算法的感觉).
但是遇到问题先仔细分析需求, 然后从可复用性和可扩展性考虑规划代码架构, 最后进行系统的测试的能力, 是可以在每个项目中发挥作用的.
至于同时参加编程测试的人互相抄袭的问题, 现在应该已经有自动代码查重的工具了. 而且题库丰富起来之后, 给可能相互认识的求职者 (比如同一个研究所) 分发不同的题目即可.

当然, 这种模式也有缺点, 就是在现有技术水平下只能人工判卷, 且评判结果主观性太强, 无法适应大规模的招聘.
