# Graph

## Definition

\(G(V,E)\) is a graph, where \(V\) is the set of vertices and \(E\) is the set of edges.

- **Undirected gragh**: \((v_i, v_j)=(v_j, v_i)\)
- **Directed graph**: \(\langle v_i, v_j \rangle \neq \langle v_j, v_i \rangle\)
- **Restrictions**:

    - No self loop
    - Multigraph is not considered

- **Complete graph**: with maximum number of edges
- **Adjacent**:

    - \(v_i\) and \(v_j\) are adjacent if \((v_i, v_j) \in E\)
    - \(v_i\) is adjacent to \(v_j\) if \(\langle v_i, v_j \rangle \in E\)

- **Length of a path**: number of edges in the path
- **Simple path**: \(v_0, v_1, \cdots, v_k\) are distinct
- **Cycle**: simple path with \(v_0 = v_k\)
- **Strongly connected directed graph**: for every pair of vertices \(v_i\) and \(v_j\), there is a path from \(v_i\) to \(v_j\) and a path from \(v_j\) to \(v_i\)
- **Strongly connected component**: a maximal strongly connected subgraph

## Topological Sort

### Definition

- **AOV Network**: 有向图，有向边表示关系。
- **Partial order**：transitive and irreflexive

### Algorithm

```c
void Topo(Graph G) {
    Queue Q;
    for (each vertex v) {
        if (indegree[v] == 0) {
            Enqueue(Q, v);
        }
    }
    while (!IsEmpty(Q)) {
        v = Dequeue(Q);
        print v;
        for (each w adjacent to v) {
            if (--indegree[w] == 0) {
                Enqueue(Q, w);
            }
        }
    }
}
```

## Shortest Path

### Dijkstra's Algorithm

```c
void Dijkstra(Graph G, Vertex s) {
    MinHeap H;
    T[s].dist = 0;
    Insert(H, s);
    while (!IsEmpty(H)) {
        Vertex v = DeleteMin(H);
        T[v].known = true;
        for (each w adjacent to v) {
            if (!T[w].known) {
                if (T[v].dist + weight < T[w].dist) {
                    T[w].dist = T[v].dist + weight;
                    T[w].path = v;
                    Insert(H, w);
                }
            }
        }
    }
}
```

- Time complexity: \(O((|V| + |E|) \log |V|)\)
- Space complexity: \(O(|V|)\)

### Bellman-Ford Algorithm

with negative weight

```c
void SPFA(Graph G, Vertex s) {
    Queue Q;
    T[s].dist = 0;
    Enqueue(Q, s);
    while (!IsEmpty(Q)) {
        Vertex v = Dequeue(Q);
        T[v].inqueue = false;
        for (each w adjacent to v) {
            if (T[v].dist + weight < T[w].dist) {
                T[w].dist = T[v].dist + weight;
                T[w].path = v;
                if (!T[w].inqueue) {
                    Enqueue(Q, w);
                    T[w].inqueue = true;
                }
            }
        }
    }
}
```

### Acyclic Graph

Use topological sort (BFS)

## Network Flow

- 残差图：设 \(f\) 为流量，则残差为：

    \[
    c_f(u, v) =
    \begin{cases}
    c(u, v) - f(u, v) & \text{if } (u, v) \in E \\
    f(v, u) & \text{if } (v, u) \in E \\
    0 & \text{otherwise}
    \end{cases}
    \]

- 找残差图中从源点到汇点的一条简单路径，称为增广路（augmenting path）。
- 增广路每次能推送的流量为路径上最小的残差。
- 更新残差图，直到找不到增广路为止。

## Minimum Spanning Tree

### Prim's Algorithm

- Start from set with one vertex
- Find the minimum edge connecting the set and the rest vertices.
- Add the adjacent vertex to the set.
- Repeat until all vertices are in the set.

### Krukal's Algorithm

Sort all edges by weight. Then add the edge to the tree if it doesn't form a cycle. (use union-find)

## DFS

### Biconnectivity

- **Biconnectivity** 双连通：任意两点之间至少存在两条不相交的路径。
- **Articulation point** 割点：删除该点后，图不再连通分量数量增加了。
- **Bi-connected component** 双连通分量：最大的双连通子图。

#### Tarjan's Algorithm

- `dfn[u]`：节点 `u` 的 dfs 序。
- `low[u]`：节点 `u` 或其子树能够追溯到的最早的祖先节点。

```c
void Tarjan(Graph G, Vertex u) {
    dfn[u] = low[u] = ++cnt;
    stack.push(u);
    instack[u] = true;
    for (each v adjacent to u) {
        if (!dfn[v]) {
            Tarjan(G, v);
            low[u] = min(low[u], low[v]);
        } else if (instack[v]) {
            low[u] = min(low[u], dfn[v]);
        }
    }
    if (dfn[u] == low[u]) {
        ++bcc_cnt;
        do {
            v = stack.pop();
            bcc[v] = bcc_cnt;
            instack[v] = false;
        } while (u != v);
    }
}
void FindBCC(Graph G) {
    for (each v) {
        if (!dfn[v]) {
            Tarjan(G, v);
        }
    }
}
```

### Euler Circuits and Paths

- **Euler Path**：经过所有边一次的通路。

    条件：有且仅有两个节点度数为奇数，其余节点度数为偶数。

    !!! info "Algorithm"

        从度数为奇数的节点开始 dfs，遍历完所有边后，欧拉路为 dfs 的后序遍历。

- **Euler Circuit**：经过所有边一次的回路。

    条件：所有节点度数为偶数。

    !!! info "Algorithm"

        Hierholzer's Algorithm

        从一条回路开始，每次将回路上任意节点替换为子回路，直到所有边都被访问。

- **Hamiltonian Path**：经过所有节点一次的通路。