# Red-Black Trees and B+ Trees

## Red-Black Trees

二叉搜索树，满足以下条件：

1. 每个节点是红色或黑色
2. 根节点是黑色
3. 每个叶子节点（NIL）是黑色
4. 如果一个节点是红色，那么它的两个子节点都是黑色
5. 对于每个节点，从该节点到其所有后代叶子节点的简单路径上，均包含相同数目的黑色节点

!!! note "Black Height"
    从一个节点到一个叶子节点的任何简单路径上的黑色节点数目称为该节点的黑高度（black-height），记作 \(\mathrm{bh}(x)\)。

    !!! info "引理"

        一棵有 \(n\) 个内部节点的红黑树的高度至多为\(2\log(n+1)\)。

### Operations

#### 树旋转（Tree Rotation）

和 AVL 树、Splay 树一样。

#### Insertion

找到插入位置，插入新节点，初始颜色为红色。

可能破坏的性质为 2 和 4。

对于性质 2，为插入第一个节点的情况，直接将根节点涂黑即可。

对于性质 4，分为以下情况：

1. 出现两个连续的红色节点，且上层的黑色节点的左右儿子都是红色：

    <div align="center"><img src="/assets/img/cs/ads/chapter2/rbtree-ins-case1.png" width="50%"></div>

    调整后向上递归检查。

2. 出现两个连续的红色节点，上层的黑色节点的另外一个儿子是黑色，且两个红色节点不在一条直线上：

    <div align="center"><img src="/assets/img/cs/ads/chapter2/rbtree-ins-case2.png" width="50%"></div>

    进行一次旋转，使得两个红色节点在一条直线上。

3. 出现两个连续的红色节点，上层的黑色节点的另外一个儿子是黑色，且两个红色节点在一条直线上：

    <div align="center"><img src="/assets/img/cs/ads/chapter2/rbtree-ins-case3.png" width="50%"></div>

    将上层红色节点染黑，上层黑色节点染红，此时子树左边的黑高度比右边多 1，进行一次旋转。

#### Deletion

:TODO:

## B+ Trees

度（degree）为 \(m\) 的 B+ 树满足以下条件：

1. 根节点要么为叶子节点，要么儿子数目在 2 和 \(m\) 之间
2. 其他非叶子节点的儿子数目在 \(\lceil m/2 \rceil\) 和 \(m\) 之间
3. 每个叶子节点具有相同的深度

度为 4 的 B+ 树（2-3-4 树）：

<div align="center"><img src="/assets/img/cs/ads/chapter2/bplustree-example.png" width="90%"></div>

### Operations

#### Split

当一个节点的儿子数目超过 \(m\) 时，将其分裂为两个节点接在父节点，中间的键上升到父节点，再递归检查父节点。

在根节点分裂时，新建一个根节点。

#### Merge

当一个节点的儿子数目小于 \(\lceil m/2 \rceil\) 时，分类讨论：

- 若其某个相邻节点的儿子数目大于 \(\lceil m/2 \rceil\)，则将其相邻节点的相邻儿子移动到该节点
- 若其相邻节点的儿子数目都等于 \(\lceil m/2 \rceil\)，则将其与相邻节点合并

合并操作完成后，更新父节点的信息。

在根节点的儿子数目减少到 1 时，将其删除。

#### Insertion

找到插入位置，插入新节点，回溯检查进行 Split 操作。

#### Deletion

找到删除位置，删除节点，回溯检查进行 Merge 操作。

### Complexity Analysis

度为 \(m\) 且有 \(n\) 个节点的 B+ 树的高度为 \(O(\log_{\lceil m/2 \rceil} n)\)（每个节点的儿子数目取 \(\lceil m/2 \rceil\) 时高度最大）。

插入和删除操作的时间复杂度为 \(O(\log_{\lceil m/2 \rceil} n)\)。