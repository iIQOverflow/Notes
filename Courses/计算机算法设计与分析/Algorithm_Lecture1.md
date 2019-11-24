# Lecture1

观察：
1. 问题的结构，Q1：最简单的case，Q2：相似的问题，注意也要**从最简单的case入手**，Q3：能不能分解成小问题（sub-problems）
2. 问题的解，Q4：form（解是什么样子，如何表示这个解）Q5：space size（有多少个解），Q6：解之间的关系（能不能稍微做一些扰动，变成别的解）

分而治之：将复杂的实例分解成简单实例，或者使用简单实例的解组合成复杂实例的解，比如旅行商问题（TSP），4个结点的解能不能帮助构造出5个结点的解。

math problem
1. positive 正面求解，S1：DECOMPOSE，能否分解成子问题，如果可以，（INDUCTION，D-C）使用归纳的方法。优化可分解（optimal， sub-structure），则DP（动态规划），Greedy。S2：IMPROVEMENT，如果不能分解，improvement，一直用完整的解来做，典型代表线性规划，QP/SDP/NF/LS/MC/SA，...
P5 solution form x=[x1,x2,...xn],xi=0/1，ENUMERATION（枚举）
难的问题，trade-off，不求最优解（optimal->approximation）不求确定性的解（Deterministic->randomization）（Worst-case->practical cases）
2. negative 不可求解

旅行商 环 子问题的解不能加工成原始问题的解。

求解前预先估计下界：
1. B&B
2. EM
3. LP对偶
4. 近似算法

b在c之前，b在c之后，只选一个。

## take home message
1. 做问题时，从最简单的case入手。
2. 基于对问题结构/解的观察设计。从复杂到简单，分解成SUB-PROBLEM。