# Leftist Heaps and Skew Heaps

## Leftist Heaps

!!! note "Null Path Length"
    
    节点的 null path length 是从该节点到最近的没有两个儿子的节点的最短路径的长度。有

    \[
        \mathrm{NPL}(x) = \min(\mathrm{NPL}(\mathrm{left}(x)), \mathrm{NPL}(\mathrm{right}(x))) + 1
    \]

    定义 \(\mathrm{NPL}(\text{null}) = 0\)。

左偏树（Leftist Heap）是一种二叉堆，它满足左偏性质：对于每个节点 \(x\)，其左子树的 npl 不小于右子树的 npl。

### Operations

#### Merge

选取两个左偏树的根节点中较小的一个作为新根节点，然后递归地将较大根节点的右子树与较小根节点合并。

合并后，如果左子树的 npl 小于右子树的 npl，则交换左右子树，然后更新 npl。

参考代码：

```c
Node *merge(Node *T1, Node *T2) {
    if (T1 == NULL || T2 == NULL) {
        return T1 == NULL ? T2 : T1;
    }
    if (T1->key > T2->key) {
        swap(T1, T2);
    }
    T1->right = merge(T1->right, T2);
    if (T1->left == NULL || T1->left->npl < T1->right->npl) {
        swap(T1->left, T1->right);
    }
    T1->npl = T1->right == NULL ? 0 : T1->right->npl + 1;
    return T1;
}
```

#### Insertion

将新节点作为一个左偏树，然后与原左偏树合并。

#### Deletion

将根节点删除，然后将左右子树合并。

### Complexity Analysis

左偏树的右路径长度不超过 \(\log n\)。

!!! note "证明"

    右路径长度为 \(k\) 的左偏树至少包含 \(2^k - 1\) 个节点（归纳证明）。因此，节点数为 \(n\) 的左偏树的右路径长度不超过 \(\log n\)。

合并操作为遍历右路径，时间复杂度为 \(O(\log n)\)。

## Skew Heaps

斜堆（Skew Heap）是一种二叉堆，它不满足左偏性质。

类似于左偏树，但斜堆每次合并都交换左右子树。

### Operations

#### Merge

合并之后，交换左右子树。

参考代码：

```c
Node *merge(Node *T1, Node *T2) {
    if (T1 == NULL || T2 == NULL) {
        return T1 == NULL ? T2 : T1;
    }
    if (T1->key > T2->key) {
        swap(T1, T2);
    }
    T1->right = merge(T1->right, T2);
    swap(T1->left, T1->right);
    return T1;
}
```

### Complexity Analysis

均摊分析

设 \(D_i\) 为第 \(i\) 次操作后的根，势能函数 \(\Phi(D_i) = \texttt{重节点个数}\)

!!! note "重节点"

    定义重节点为右子树大小大于等于左子树大小的节点。

定义 \(H_i = l_i + h_i\) 为右路径上轻节点个数和重节点个数之和，即右路径的长度。

将合并前的两棵树分别表示为 \(*_1\) 和 \(*_2\)。

合并前的势能为 \(\Phi(D_i) = h_1 + h_2 + h\)，其中 \(h\) 非右路径上的重节点个数。

合并后，由于交换左右子树，右路径上的重节点一定会变成轻节点，轻节点可能变成重节点，取上界则有

\[
    \Phi(D_{i+1}) \leq l_1 + l_2 + h
\]

单次合并的实际代价 \(c_i = l_1 + l_2 + h_1 + h_2\)，均摊代价为

\[
    c'_i = c_i + \Phi(D_{i+1}) - \Phi(D_i) \leq 2(l_1 + l_2)
\]

由于轻节点个数为 \(O(\log n)\)，因此均摊代价为 \(O(\log n)\)。