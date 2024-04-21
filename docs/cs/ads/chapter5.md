# Binomial Queue

## Definition

二项队列由一组二项树组成。度为 \(k\) 的二项树满足：

- 满足堆序性质。
- 有 \(2^k\) 个节点。
- 根节点有 \(k\) 棵子树，分别是度为 \(0, 1, \ldots, k-1\) 的二项树。

<div align="center"> <img src="/assets/img/CS/ads/chapter5/binomial-queue.png" width="80%"/> </div>

## Operations

### Merge

#### Merge Trees

合并两棵二项树，要求度必须相等。

选择键值较小的根节点作为新根节点，然后将另一二项树作为新根节点的子树。

#### Merge Queues

!!! note "二项树的度和二项队列节点数的关系"

    假设 \(n\) 的二进制表示为 \(b_k b_{k-1} \ldots b_0\)，则节点数为 \(n\) 的二项队列，若 \(b_i = 1\)，则包含度为 \(i\) 的二项树。

类似二进制加法，同度的二项树合并，若产生进位，则合并到度更高的二项树。

### Insertion

将新节点作为度为 \(0\) 的二项树，然后与原二项队列合并。

### Deletion

找到根节点键值最小的二项树，删除根节点，然后将其子树作为一个新的二项队列，与原二项队列合并。

## Complexity Analysis

二项树合并的时间复杂度为 \(O(1)\)，单次二项队列合并的时间复杂度为 \(O(\log n)\)。

插入的均摊时间复杂度为 \(O(1)\)。