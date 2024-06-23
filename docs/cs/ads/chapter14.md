# Parallel Algorithms

描述并行算法：

- PRAM 模型：Parallel Random Access Machine，共享内存模型
- WD：Work Depth

## PRAM 模型

PRAM 模型是一种共享内存模型，有三种类型：

- EREW：Exclusive Read Exclusive Write 排他读排他写
- CREW：Concurrent Read Exclusive Write 并发读排他写
- CRCW：Concurrent Read Concurrent Write 并发读并发写

    如果发生写冲突，有两种策略：

    - Arbitrary rule：任意规则，随机选择一个写入
    - Priority rule：优先规则，设定优先级，优先级高的写入
    - Common rule：公共规则，所有写入的值必须相同

`pardo` 表示循环并行。

## WD 模型

闲置处理器不用设置为 idle，而是可以让其执行其他任务。

## Measuring the Performance

- Work load：操作的总数 \(W(N)\)
- Worst case running time：最坏情况运行时间 \(T(N)\)

渐进等价：

- \(W(N)\) 次操作，运行时间为 \(T(N)\)
- 需要的处理器数 \(P(N) = W(N) / T(N)\)，只能用于 PRAM 模型
- 设处理器数为 \(p \leq P(N)\)，则运行时间为 \(W(N) / p\)
- 如果处理器数 \(p > P(N)\)，则运行时间为 \(W(N) / p + T(N)\)

## Prefix Sum

前缀和问题：给定一个数组，计算前缀和。

## Merging

merge 两个有序数组。

\(\mathrm{RANK}(j, A)\) 代表数组 \(A\) 中小于等于 \(B_j\) 的元素个数，即为 \(B_j\) 应该放在数组 \(A\) 中的位置。

## Doubly-Logarithmic Paradigm

