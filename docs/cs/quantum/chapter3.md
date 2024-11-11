# 量子算法

## 量子傅立叶变换

### 离散傅立叶变换

将离散信号从时域转换到频域。

离散傅立叶变换：

\[
    \begin{aligned}
        &\{x_j\} \xrightarrow{\mathrm{DFT}} \{y_k\} \\
        &y_k = \frac{1}{\sqrt{N}} \sum_{j=0}^{N-1} x_j e^{\frac{2\pi i}{N} jk}
    \end{aligned}
\]

逆离散傅立叶变换：

\[
    \begin{aligned}
        &\{y_k\} \xrightarrow{\mathrm{IDFT}} \{x_j\} \\
        &x_j = \frac{1}{\sqrt{N}} \sum_{k=0}^{N-1} y_k e^{-\frac{2\pi i}{N} jk}
    \end{aligned}
\]

### 量子傅立叶变换

量子傅里叶变换（QFT）的作用是将量子态从计算基 \(\{\left\lvert j \right\rangle\}\) 转换到频域基 \(\{\left\lvert k \right\rangle\}\)。振幅序列 \(\{x_j\}\) 被转换为 \(\{y_k\}\)

\[
    \begin{aligned}
        &\sum_{j=0}^{N-1} x_j \left\lvert j \right\rangle \xrightarrow{\mathrm{QFT}} \sum_{k=0}^{N-1} y_k \left\lvert k \right\rangle \\
        &y_k = \frac{1}{\sqrt{N}} \sum_{j=0}^{N-1} x_j e^{\frac{2\pi i}{N} jk}
    \end{aligned}
\]

作为量子算符为：

\[
    \mathrm{QFT}\left\lvert j \right\rangle = \frac{1}{\sqrt{N}} \sum_{k=0}^{N-1} e^{\frac{2\pi i}{N} jk} \left\lvert k \right\rangle
\]

### QFT 的张量基形式

规定记法：

- 基态 \(\left\lvert j \right\rangle\) 表示为 \(\left\lvert \overline{j_1 j_2 \cdots j_n} \right\rangle\)，其中 \(j = j_1 2^{n-1} + j_2 2^{n-2} + \cdots + j_n 2^0\)。
- \(\overline{0.j_1 j_2 \cdots j_n}\) 表示二进制小数 \(j_1 2^{-1} + j_2 2^{-2} + \cdots + j_n 2^{-n}\)。

使用 \(n\) 个量子比特进行 QFT，共有 \(N = 2^n\) 个基态。QFT 可以写成：

\[
    \begin{aligned}
        \mathrm{QFT}\left\lvert j \right\rangle &= \frac{1}{\sqrt{N}} \sum_{k=0}^{N-1} e^{\frac{2\pi i}{N} jk} \left\lvert k \right\rangle \\
        \mathrm{QFT}\left\lvert \overline{j_1 j_2 \cdots j_n} \right\rangle &= \frac{1}{\sqrt{2^n}} (\left\lvert 0 \right\rangle + e^{2\pi i \overline{0.j_n}} \left\lvert 1 \right\rangle) \otimes \cdots \otimes (\left\lvert 0 \right\rangle + e^{2\pi i \overline{0.j_1 j_2 \cdots j_n}} \left\lvert 1 \right\rangle)
    \end{aligned}
\]

!!! note "Proof"

    \[
        \begin{aligned}
            \mathrm{QFT}\left\lvert j \right\rangle &= \frac{1}{\sqrt{2^n}} \sum_{k=0}^{2^n-1} \exp(\frac{2\pi i}{2^n} jk) \left\lvert k \right\rangle \\
            &= \frac{1}{\sqrt{2^n}} \sum_{k_1=0}^{1} \cdots \sum_{k_n=0}^{1} \exp(\frac{2\pi i}{2^n} j \sum_{l=1}^{n} k_l 2^{n-l}) \left\lvert k_1 k_2 \cdots k_n \right\rangle \\
            &= \frac{1}{\sqrt{2^n}} \sum_{k_1=0}^{1} \cdots \sum_{k_n=0}^{1} \bigotimes_{l=1}^{n} \exp(\frac{2\pi i}{2^n} j k_l 2^{n-l}) \left\lvert k_l \right\rangle \\
            &= \frac{1}{\sqrt{2^n}} \bigotimes_{l=1}^{n} \sum_{k_l=0}^{1} \exp(\frac{2\pi i}{2^n} j k_l 2^{n-l}) \left\lvert k_l \right\rangle \\
            &= \frac{1}{\sqrt{2^n}} \bigotimes_{l=1}^{n} (\left\lvert 0 \right\rangle + \exp(2\pi i j 2^{-l}) \left\lvert 1 \right\rangle)
        \end{aligned}
    \]

    由于 \(e^{2\pi i} = 1\)，所以可以把 \(j 2^{-l}\) 的整数部分略去，得到：

    \[
        \begin{aligned}
            \mathrm{QFT}\left\lvert j \right\rangle &= \frac{1}{\sqrt{2^n}} (\left\lvert 0 \right\rangle + e^{2\pi i \overline{0.j_n}} \left\lvert 1 \right\rangle) \otimes \cdots \otimes (\left\lvert 0 \right\rangle + e^{2\pi i \overline{0.j_1 j_2 \cdots j_n}} \left\lvert 1 \right\rangle)
        \end{aligned}
    \]

!!! example "单量子比特 QFT"

    考虑单量子比特下的 QFT：

    \[
        \mathrm{QFT}_1 \left\lvert j \right\rangle = \frac{1}{\sqrt{2}} \sum_{k=0}^{1} e^{\frac{2 \pi i}{2} jk} \left\lvert k \right\rangle
    \]

    对于 \(\left\lvert 0 \right\rangle\) 和 \(\left\lvert 1 \right\rangle\)，有：

    \[
        \begin{aligned}
            \mathrm{QFT}_1 \left\lvert 0 \right\rangle &= \frac{1}{\sqrt{2}} (\left\lvert 0 \right\rangle + \left\lvert 1 \right\rangle) \\
            \mathrm{QFT}_1 \left\lvert 1 \right\rangle &= \frac{1}{\sqrt{2}} (\left\lvert 0 \right\rangle - \left\lvert 1 \right\rangle)
        \end{aligned}
    \]

    所以，单量子比特的 QFT 和 Hadamard 门等价。

!!! example "双量子比特 QFT"

    双量子比特下的 QFT：

    \[
        \mathrm{QFT}_2 \left\lvert j \right\rangle = \frac{1}{2} (\left\lvert 0 \right\rangle + e^{2\pi i \overline{0.j_2}} \left\lvert 1 \right\rangle) \otimes (\left\lvert 0 \right\rangle + e^{2\pi i \overline{0.j_1 j_2}} \left\lvert 1 \right\rangle)
    \]

    对于计算基 \(\left\lvert 10 \right\rangle\) 有：

    \[
        \begin{aligned}
            \mathrm{QFT}_2 \left\lvert 10 \right\rangle &= \frac{1}{2} (\left\lvert 0 \right\rangle + e^{2\pi i \overline{0.0}} \left\lvert 1 \right\rangle) \otimes (\left\lvert 0 \right\rangle + e^{2\pi i \overline{0.10}} \left\lvert 1 \right\rangle) \\
            &= \frac{1}{2} (\left\lvert 0 \right\rangle + \left\lvert 1 \right\rangle) \otimes (\left\lvert 0 \right\rangle - \left\lvert 1 \right\rangle) \\
            &= \frac{1}{2} (\left\lvert 00 \right\rangle - \left\lvert 01 \right\rangle + \left\lvert 10 \right\rangle - \left\lvert 11 \right\rangle)
        \end{aligned}
    \]

### QFT 的量子电路

定义单量子比特旋转门：

\[
    R_k =
    \begin{bmatrix}
        1 & 0 \\
        0 & e^{\frac{2\pi i}{2^k}}
    \end{bmatrix}
\]

构建如下量子电路：

<div align="center"><img src="/assets/img/CS/quantum/chapter3/qft_circuit.png" width="90%"></div>

执行过程：

1. 经过一个 \(H\) 门后，量子态变为 \(\frac{1}{\sqrt{2}} (\left\lvert 0 \right\rangle + e^{2\pi i \overline{0.j_1}} \left\lvert 1 \right\rangle)\left\lvert j_2 \cdots j_n \right\rangle\)。
2. 当 \(j_2 = \left\lvert 1 \right\rangle\) 时，执行受控旋转门 \(R_2\)，量子态变为 \(\frac{1}{\sqrt{2}} (\left\lvert 0 \right\rangle + e^{2\pi i \overline{0.j_1 j_2}} \left\lvert 1 \right\rangle)\left\lvert j_3 \cdots j_n \right\rangle\)。
3. 同理，执行所有作用于第一个量子比特的受控旋转门，得到 \(\frac{1}{\sqrt{2}} (\left\lvert 0 \right\rangle + e^{2\pi i \overline{0.j_1 j_2 \cdots j_n}} \left\lvert 1 \right\rangle)\)。
4. 同理，最后得到 \(\frac{1}{\sqrt{2^n}} (\left\lvert 0 \right\rangle + e^{2\pi i \overline{0.j_1 j_2 \cdots j_n}} \left\lvert 1 \right\rangle)(\left\lvert 0 \right\rangle + e^{2\pi i \overline{0.j_2 \cdots j_n}} \left\lvert 1 \right\rangle) \cdots (\left\lvert 0 \right\rangle + e^{2\pi i \overline{0.j_n}} \left\lvert 1 \right\rangle)\)。
5. 最后再加一系列 \(\mathrm{SWAP}\) 门，得到 QFT 的量子电路。

QFT 的量子门复杂度为 \(\mathcal{O}(n^2)\)。

!!! example "双量子比特 QFT 电路"

    <div align="center"><img src="/assets/img/CS/quantum/chapter3/2qubit_qft_circuit.png" width="30%"></div>

## 量子相位估计

量子相位估计（QPE）用于估计给定量子态的相位信息。

### 基本目标

已知酉矩阵 \(U\) 和其本征态 \(\left\lvert \mu \right\rangle\)，对应本征值为 \(e^{2\pi i \varphi}\)，估计其中相位的值 \(\varphi \in \left[0, 1\right)\)。

### QPE 量子电路

<div align="center"><img src="/assets/img/CS/quantum/chapter3/qpe_circuit.png" width="60%"></div>

#### 第一阶段

<div align="center"><img src="/assets/img/CS/quantum/chapter3/qpe_circuit1.png" width="60%"></div>

在 \(R_p\) 中，从下往上设量子态为 \(\left\lvert j_1 \right\rangle, \left\lvert j_2 \right\rangle, \cdots, \left\lvert j_n \right\rangle\)，则：

\[
    \left\lvert j_k \mu \right\rangle \xrightarrow{\mathrm{C-U}^{2^{k - 1}}} \left\lvert j_k \right\rangle U^{j_k 2^{k-1}} \left\lvert \mu \right\rangle
\]

因为 \(U\left\lvert \mu \right\rangle = e^{2\pi i \varphi} \left\lvert \mu \right\rangle\)，所以：

\[
    \left\lvert j_k \right\rangle U^{j_k 2^{k-1}} \left\lvert \mu \right\rangle = \frac{1}{\sqrt{2}} (\left\lvert 0 \right\rangle + e^{2\pi i 2^{k - 1} \varphi} \left\lvert 1 \right\rangle) \left\lvert \mu \right\rangle
\]

#### 第二阶段

由 \(\varphi = \overline{0.\varphi_1 \varphi_2 \cdots \varphi_n}\) 得第一阶段结束后 \(R_p\) 的量子态为：

\[
    \frac{1}{\sqrt{2^n}} (\left\lvert 0 \right\rangle + e^{2\pi i \overline{0.\varphi_n}} \left\lvert 1 \right\rangle) \otimes \cdots \otimes (\left\lvert 0 \right\rangle + e^{2\pi i \overline{0.\varphi_1 \varphi_2 \cdots \varphi_n}} \left\lvert 1 \right\rangle)
\]

最后，对 \(R_p\) 的量子态进行逆量子傅立叶变换（IQFT），得 \(\left\lvert \varphi_1 \varphi_2 \cdots \varphi_n \right\rangle\)，从而得到 \(\varphi\) 的估计值。