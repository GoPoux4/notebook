# Dynamic Programming

## Optimal Binary Search Tree

N 个词，按照字典序排列 \(w_1, w_2, \cdots, w_N\)，每个词 \(w_i\) 的被检索到的概率为 \(p_i\)，求一棵二叉搜索树，使得检索的期望代价最小。

即最小化 \(\sum_{i=1}^N p_i (d_i + 1)\)，其中 \(d_i\) 为 \(w_i\) 在树中的深度。

设

- \(T_{ij}\) 为 \(w_i, w_{i+1}, \cdots, w_j\) 的最优二叉搜索树
- \(c_{ij}\) 为 \(T_{ij}\) 的代价

则有

\[
c_{ij} = \min_{i \leq r \leq j} \left\{ c_{i,r-1} + c_{r+1,j} + \sum_{k=i}^j p_k \right\}
\]

且 \(c_{ii} = p_i\)。