# Union and Find

## The Disjoint Set ADT

Relation \(\sim\) is an equivalence relation iff its **symmetric**(对称), **reflexive**(自反) and **transitive**(传递).

Two members \(x, y\) are said to be in the same equivalence class iff \(x \sim y\).

### Operations

- **Union**: combine two equivalence classes into one
- **Find**: determine which equivalence class a given element belongs to

## Implementation

`S[u]` 代表节点 `u` 的父节点。

### Implementation 1

每个集合的根节点与一个集合的编号建立双向链表。

### Implementation 2

`S[root] = 0`，集合编号为根节点编号。

## Smart Union Algorithms

### Union by Size

Let `S[root] = -size`，`size` 为集合大小。

每次 Union 时，将小集合的根节点指向大集合的根节点。

时间复杂度：\(n\) 次 Union 操作，\(m\) 次 Find 操作，复杂度 \(O(n + m \log n)\)。

### Union by Height

每次 `Union` 时，将高度小的根节点指向高度大的根节点。

## Path Compression

路径压缩。

```c
int find(int x) {
    if (isroot(x)) return x;
    else return S[x] = find(S[x]);
}
```

## Time Complexity

\(O(\alpha(n))\)，其中 \(\alpha(n)\) 为 Ackermann 函数的反函数。