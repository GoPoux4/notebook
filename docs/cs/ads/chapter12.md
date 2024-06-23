# Local Search

- Local：定义邻居解
- Search：在邻居解中搜索最优解

## Neighbor Relation

- \(S \sim S'\)：\(S'\) 是 \(S\) 的相邻解，通过 \(S\) 转化得到
- \(N(S) = \{S' \mid S \sim S'\}\)：\(S\) 的邻居解集合

## Vertex Cover

给定图 \(G = (V, E)\)，找到最小的顶点集合 \(S \subseteq V\)，使得每条边至少有一个端点在 \(S\) 中。

初始从 \(S\) 开始，每次随机选择一个顶点 \(v \in S\)，将其从 \(S\) 中移除，直到 \(S\) 不再是顶点覆盖。

### The Metropolis Algorithm

每次若 \(S'\) 更优，则接受 \(S'\) 作为新的解，否则以一定概率 \(e^{-\Delta \text{cost} / (kT)}\) 接受 \(S'\)。

其中 \(\Delta \text{cost} = \left| \text{cost}(S) - \text{cost}(S') \right|\)，\(k, T\) 为超参数。

## Hopfield Neural Network

图 \(G = (V, E)\) 每条边有边权，给每个点设置状态 \(s_i \in \{-1, 1\}\)。

- 如果 \(w_i < 0\)，则希望两端点状态相同
- 如果 \(w_i > 0\)，则希望两端点状态不同
- \(|w_i|\) 越大，约强调这个约束

定义：

- 边 \(e = (u, v)\) 是 good 的，如果 \(w_e s_u s_v < 0\)，否则是 bad 的。（即满足约束时为 good）
- 点 \(v\) 被满足，如果：

    \[
        \sum_{v: e = (u, v) \in E} w_e s_u s_v \geq 0
    \]

- 如果所有点都被满足了，那么定义这个分配是 stable 的。

### State-flipping Algorithm

初始给每个点随机状态，如果当前分配不是 stable 的，则随机选择一个没有被满足的点，将其状态翻转。

!!! note "Proof"

    评估解 \(S\) 的好坏：\(\Phi(S) = \sum_{e \text{ is good}} |w_e|\)，\(\Phi(S)\) 越大越好。

    翻转了一个点 \(u\) 后，原本的好边变为坏边，坏边变为好边，所以：

    \[
        \Phi(S') = \Phi(S) - \sum_{e: u \in e, e \text{ is good in } S} |w_e| + \sum_{e: u \in e, e \text{ is bad in } S} |w_e|
    \]

    由于 \(u\) 是没有被满足的点，所以：

    \[
        \sum_{e: u \in e, e \text{ is bad}} |w_e| \geq \sum_{e: u \in e, e \text{ is good}} |w_e|
    \]

    所以 \(\Phi(S') \geq \Phi(S)\)，即每次翻转都能使得解更优。

近似比为 \(2\)，\(w(A, B) \geq \frac{1}{2} w(A^*, B^*)\)。