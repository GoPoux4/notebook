# NP Completeness

## Halting Problem

停机问题，程序判断另一个程序是否会停机。

## The Class NP

!!! note "Turing Machine"

    假设有无限的 memory，和一个可以左右移动的 scanner，可以读写 memory。

    - Deterministic Turing Machine 确定性图灵机：每次只有一个状态转移。
    - Non-deterministic Turing Machine 非确定性图灵机：每次有多个可能的状态转移，每次都能选择一个正确的转移。

- NP: Non-deterministic Polynomial-time
    一个问题是 NP 的，当且仅当可以在多项式时间内验证一个解是否正确。可以由一个非确定性图灵机在多项式时间内解决。
- P: Polynomial-time
    一个问题是 P 的，当且仅当可以由一个确定性图灵机在多项式时间内解决。

P 是 NP 的子集。

### NP-Completeness(NPC) Problem

任何 NP 问题都可以在多项式时间内约化为 NPC 问题。

## A Formal-language Framework

Abstract problem \(Q\) 包含多个 instance \(I\)，每个 instance 对应一个 solution \(S\)

!!! example "Shortest Path Problem"
    - \(I = (G, s, t), G = (V, E), s, t \in V\)
    - \(S = \langle s, v_1, v_2, \cdots, t \rangle, \langle v_i, v_{i+1} \rangle \in E\)

    转换为 decision problem：

    - \(I = (G, s, t, k)\) 是否存在一条长度小于 \(k\) 的路径。 
    - \(S = \{\text{yes}, \text{no}\}\)

### Formal Language Theory