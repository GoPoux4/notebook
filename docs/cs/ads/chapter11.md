# Approximation

## Approximation Ratio

假设近似算法的解为 \(C\)，最优解为 \(C^*\)。那么近似算法的近似比例为：

\[
\max\left(\frac{C}{C^*}, \frac{C^*}{C}\right) \leq \rho(n)
\]

- PTAS: 关于 \(n\) 都是多项式时间的近似算法。
- FPTAS: 关于 \(1/\epsilon\) 和 \(n\) 是多项式时间的近似算法。

## Approximate Bin Packing

NP-hard 问题

假设箱子大小为 1。

### Next Fit

每次尝试放入最后一个箱子，如果放不下就新开一个箱子。

假设使用了 \(2M\) 个箱子，设 \(S_i\) 为第 \(i\) 个箱子装入的物品的大小之和。那么：

\[
\begin{aligned}
S_1 + S_2 > 1 \\
S_2 + S_3 > 1 \\
\cdots \\
S_{2M-1} + S_{2M} > 1
\end{aligned}
\]

将所有不等式相加，得到：

\[
2(S_1 + S_2 + \cdots + S_{2M}) > 2M \Rightarrow \sum_{i=1}^{2M} S_i > M
\]

则最优解至少需要

\[
\lceil \frac{\sum_{i=1}^n s_i}{\text{size of bin}} \rceil = \lceil \sum_{i=1}^n s_i \rceil \geq M + 1
\]

所以 Next Fit 的近似比例为 \(2\)。

### First Fit

每次找到第一个能放下的箱子放入，如果没有就新开一个箱子。

### Best Fit

每次找到剩余空间最小的箱子放入，如果没有就新开一个箱子。

### Worst Fit

每次找到剩余空间最大的箱子放入，如果没有就新开一个箱子。

## Knapsack Problem 

0-1 背包问题。近似比为 \(2\)。

## K-Center Problem

### Greedy Algorithm

平面上 \(n\) 个点，找到 \(k\) 个圆，使得每个点到 \(k\) 个点中最近的圆心的距离的最大值最小。

若圆半径为 \(r\)，那么若将圆心放在点上，则需要 \(2r\) 的半径才能覆盖原本圆的范围。

二分答案，验证：

1. 随机找一个点作为圆心，排除所有距离小于 \(2r\) 的点。
2. 重复 1 直到所有点都被覆盖。
3. 如果圆心数量大于 \(k\)，说明最优解大于 \(r\)，否则小于等于 \(r\)。

### Approximation Algorithm 

1. 任意找一个点作为圆心。
2. 找到离这个点最远的点，作为第二个圆心。
3. 重复 2 直到找到 \(k\) 个圆心。
4. 找到能覆盖所有点的最小半径。

近似比为 \(2\)。

除非 P=NP，否则不存在多项式时间的近似比例小于 \(2\) 的算法。