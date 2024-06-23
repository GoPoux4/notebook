# Randomized Algorithm

## 概率论

- \(\mathrm{Pr}(A)\) 表示事件 \(A\) 发生的概率
- \(\bar{A}\) 表示事件 \(A\) 不发生
- \(\mathrm{E}(X)\) 表示随机变量 \(X\) 的期望

## The Hiring Problem

面试一个人后立即决定是否录用。

面试费用 \(C_i\)，雇佣费用 \(C_h\)。\(N\) 个人，雇佣 \(M\) 个人。

### Naive Algorithm

每次如果当前面试的人比之前所有的人都好，则雇佣。

设 \(X\) 为雇佣的人数，\(X_i\) 为第 \(i\) 个人是否被雇佣。

\[
X_i = \begin{cases}
1, & \text{candidate is hired} \\
0, & \text{candidate is not hired}
\end{cases}
\]

每个人被雇佣独立，所以

\[
\mathrm{E}(X) = \mathrm{E}(\sum_{i=1}^N X_i) = \sum_{i=1}^N \mathrm{E}(X_i) = \sum_{i=1}^N \mathrm{Pr}(X_i = 1) = \sum_{i=1}^N \frac{1}{i} = \ln N + O(1)
\]

### Online Version

面试的时候需要立即决定是否雇佣，且只能雇佣一次。

先面试 \(K\) 个人，找到最好的，但是不进行雇佣。然后面试后面的人，如果比之前的人好，则雇佣，结束面试。

!!! note "Decide \(K\)"
    \(S_i\) 表示第 \(i\) 个人是最好的。\(S_i\) 为真，如果事件 \(A\)（第 \(i\) 个人是最好的）发生，且事件 \(B\)（第 \(K+1\) 到第 \(i-1\) 个人都不被雇佣）发生。

    \[
    \mathrm{Pr}(S_i) = \mathrm{Pr}(A \cap B) = \mathrm{Pr}(A) \cdot \mathrm{Pr}(B) = \frac{1}{N} \cdot \frac{K}{i-1} = \frac{K}{N(i-1)}
    \]

    得到雇佣到最好的人的概率为

    \[
    \mathrm{Pr}(S) = \sum_{i=K+1}^N \mathrm{Pr}(S_i) = \sum_{i=K+1}^N \frac{K}{N(i-1)} = \frac{K}{N} \sum_{i=K}^{N-1} \frac{1}{i}
    \]

    令 \(K = \lceil N/e \rceil\)，\(\mathrm{Pr}(S) = 1/e\)。

## QuickSort

如果随机选取的 pivot 导致一侧元素个数小于 \(n/4\) ，那么重新选取 pivot。选出合适 pivot 的期望次数是两次。

将子问题分为几类：

Type \(j\)：子问题 \(S\) 满足 \(N\left(\frac{3}{4}\right)^{j+1} \leq |S| < N\left(\frac{3}{4}\right)^{j}\)

最多有 \(\left(\frac{4}{3}\right)^{j+1}\) 个 Type \(j\) 的子问题。

时间开销 \(\mathrm{E}(T_{\text{Type }j}) = \mathrm{O}(N \cdot \left(\frac{4}{3}\right)^{j+1}) \times \left(\frac{3}{4}\right)^{j+1} = \mathrm{O}(N)\)。

不同 type 个数 \(\log_{4/3} N = \mathrm{O}(\log N)\)。

所以期望时间复杂度为 \(\mathrm{O}(N \log N)\)。