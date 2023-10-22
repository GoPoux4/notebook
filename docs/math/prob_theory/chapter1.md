# 事件及其概率

## 随机事件和概率

### 随机现象

随机现象：确定性现象、随机现象（不确定性现象）。

随机现象基本属性：
- 可重复进行或重复观察（定性）
- 试验之前不知道会出现何种结果（定性）
- 所有可能的结果是已知的（定量）

### 样本空间

样本空间：随机现象所有可能的结果 $\Omega$.

样本点：每一个结果 $\omega \in \Omega$.

### 事件

事件：具有某种属性的基本结果构成事件，通常用 $A, B, C$ 表示.

事件的发生：某次试验的结果 $\omega \in A$ 则 $A$ 发生，否则不发生.

### 概率

概率：事件 $A$ 发生的概率 $P(A)$.

概率的计算：

- 物理方法
- 统计方法：频率估计概率.

概率的定义和量化：

- 主观概率：基于已有知识和信息的一种信仰或判断
- 经验概率（统计）：通过随机测试（例：抛掷硬币，频率推算概率）
- 公理化体系：严格逻辑推理

!!! note "关于统计方法"
    - 统计方法具体、可计算
    - 统计方法的基本出发点：频率极限存在且不依赖于具体的试验环境
    - 频率的极限是概率：Bernoulli 和 Borel 数学证明
  
概率论主要目的：计算随机事件的概率.

!!! note "事件的运算"
    和集合运算类似，注意术语使用。

    - $\emptyset$ ，不可能事件.
    - $\Omega$ ，必然事件.
    - $A \subseteq B$ ，$A$ 发生则 $B$ 一定发生.
    - $A \cap B \ \text{or} \ AB$ ，$A$ 和 $B$ 同时发生.
    - $A \cup B$ ，$A$ 或者 $B$ 发生.
    - $\bar A$ ，$A$ 的对立事件.
    - $A \setminus B$ ，$A$ 发生但 $B$ 不发生.
    - $A \cap B = \emptyset$ ，$A, B$ 互不相交，写作 $A \cup B = A + B$.
    - $\text{De Morgan}$ 对偶运算原理：$\overline{(\cap A_n)} = \cup \bar A_n, \overline{(\cup A_n)} = \cap\bar A_n$.

### 概率运算的基本性质

- $P(\emptyset) = 0, P(\Omega) = 1$
- $P(\bar A) = 1 - P(A)$
- $A \cap B = \emptyset$，则

$$
P(A + B) = P(A) + P(B)
$$

- 若 $A_1, \cdots, A_m$ 互不相交，则

$$
P(\sum_{k=1}^{m}A_k) = \sum_{k=1}^{m}P(A_k)
$$

## 基本概率模型

概率模型：随机现象的数学描述，包括样本空间、事件、每个事件的概率大小。

### 古典概率模型

模型特征：

- 有限个基本结果
- 每个结果等可能发生

数学描述：

$$
\begin{align}
    \Omega = \{\omega_1, \dots, \omega_N\}, \quad N < \infty \\
    P(\{\omega_i\}) = \frac{1}{N}, \quad i = 1, 2, \dots, N
\end{align}
$$

事件 A 的概率： $P(A) = \displaystyle\frac{|A|}{N}$ ，其中 $|A|$ 为事件包含基本结果数。

对于古典概率模型，关键在于计算 $N$ 和 $|A|$ 。

!!! note "计算技巧"
    - 乘法原理
    - 排列组合

    常用关系式：

    $$
    \begin{array}{ccc}
        \displaystyle P_{N}^{k} = N(N-1)\dots (N-k+1) = \frac{N!}{k!} \\
        \displaystyle {N \choose k} = \frac{P_N^k}{k!} \\
        \displaystyle {N \choose k} + {N \choose k-1} = {N+1 \choose k} \\
        \displaystyle N! \sim \sqrt{2\pi N} e^{-N} N^N, \quad N \rightarrow \infty
    \end{array}
    $$

### 几何概率模型

模型特征：样本空间是一个区域，所有基本结果等可能发生。

- 基本结果不可数，且 $\Omega$ 是 $\mathbb{R}, \mathbb{R}^2, \dots, \mathbb{R}^k$ 上的可测区域
- 事件 $A$ 是 $\Omega$ 的可测子集
- 事件 $A$ 的概率 $\displaystyle P(A) = \frac{|A|}{|\Omega|}$

关键在于计算 $|A|, |\Omega|$

???+ example "Buffon's Needle Problem"
    若一根长度为 $l$ 的短针，抛在横线间间距为 $d$ 的均匀横纹纸上，求针落在一个与某条横线相交的位置的概率。假设 $l \leq d$ 。

    记针的中心距离最近的平行线的距离为 $a < d/2$ ，针与平行线的夹角为 $\theta \leq \pi /2$ 。

    则样本空间 $\Omega = [0, d / 2] \times [0, \pi / 2]$

    记事件 $A$ 为针落在一个与某条横线相交的位置，则

    $$
    A \ \text{发生} \iff a \leq \frac{l}{2}\sin\theta
    $$

    $$
    \begin{align}
        P(A) &= \frac{|A|}{|\Omega|} \\
             &= \frac{\int_{0}^{\pi / 2}\frac{l}{2} \sin\theta \text{d}\theta}{\frac{d}{2}\frac{\pi}{2}} \\
             &= \frac{2l}{\pi d}
    \end{align}
    $$

### 其他概率模型

例：

- 抛掷不均匀硬币
- 彩票

### 概率空间公理化体系

描述概率空间的三要素：

- 样本空间 $\Omega$
- 事件类 $\sigma$ -域 $\mathcal{A}$
- 概率 $P$

$(\Omega, \mathcal{A}, P)$ 构成概率空间，是随机现象的数学描述（概率模型），其中：

- $\mathcal{A}$ 满足
    - $\emptyset, \Omega \in \mathcal{A}$
    - $A \in \mathcal{A} \rightarrow \bar A \in \mathcal{A}$
    - $A_n \in \mathcal{A}, n \geq 1 \rightarrow \bigcup_{n=1}^{\infty}A_n \in \mathcal{A}$
- $P$ 满足
    - $P(\emptyset) = 0, P(\Omega) = 1$
    - $\forall A \in \mathcal{A}, P(A) \geq 0$
    - $A_n \in \mathcal{A}$ 互不相交，则

        $$
        P(\sum_{n=1}^{\infty}A_n) = \sum_{n=1}^{\infty}P(A_n)
        $$

: 任何满足上述性质的 $P$ 都称为空间 $(\Omega, \mathcal{A})$ 上的概率。

!!! note "概率 $P$ 的运算性质"
    - 单调性： $A \subseteq B$ ，则 $P(A) \leq P(B)$
    - $P(\bar A) = 1 - P(A)$
    - 对任意 $A_N \in \mathcal{A}, n \geq 1$,

    $$
    P(\bigcup_{n=1}^{\infty}A_n) \leq \sum_{n=1}^{\infty}P(A_n)
    $$

    - $A, B \in \mathcal{A}$ ，则（容斥原理）

    $$
    P(A\cup B) = P(A) + P(B) - P(AB)
    $$

    : 一般地，对于任意 $A_k \in \mathcal{A}, 1 \leq k \leq m$,

    $$
    \begin{align}
    \end{align}
    $$

## 条件概率

假设 $(\Omega, \mathcal{A}, P)$ 是一个概率空间， $A, B$ 是两个事件， $P(B)>0$ 。令

$$
P(A|B) = \frac{P(AB)}{P(B)}
$$

为在 $B$ 发生的条件下， $A$ 发生的条件概率。

!!! note "$P(B)>0$"
    - 分母不能为 $0$
    - 零概率事件无法观察到

原公式可以改写成乘法公式

$$
P(AB) = P(A|B)P(B)
$$

推广到多个事件：链式法则

$$
\begin{align}
    P(ABC) &= P(A|BC)P(BC) \\
           &= P(A|BC)P(B|C)P(C)
\end{align}
$$

???+ example
    $n$ 张彩票有一张中奖彩票，求第 $k$ 个人中奖的概率。

    令 $A_i$ 表示第 $i$ 个人中奖

    $$
    \begin{align}
    P(A_k \bar A_1 \dots \bar A_{k-1})
    &= P(\bar A_1)P(\bar A_2|\bar A_1)\dots P(A_k|\bar A_1 \dots \bar A_{k-1}) \\
    &= \frac{n-1}{n} \cdot \frac{n-2}{n-1} \cdot \dots \cdot \frac{1}{n - k + 1} \\
    &= \frac{1}{n}
    \end{align}
    $$

### 全概率公式

假设 $(\Omega, \mathcal{A}, P)$ 是一个概率空间， $B_k, 1\leq k \leq N$ 是 $N$ 个互不相交事件，且 $\Omega = \sum_{k=1}^NB_k$ 则

$$
P(A) = \sum_{k=1}^N P(A|B_k)P(B_k), \quad N \leq \infty
$$

### 贝叶斯公式

假设 $(\Omega, \mathcal{A}, P)$ 是一个概率空间， $B_k, 1\leq k \leq N$ 是 $N$ 个互不相交事件，且 $\Omega = \sum_{k=1}^NB_k$ 则

$$
P(B_k|A) = \frac{P(A|B_k)P(B_k)}{\sum_{i=1}^{N}P(A|B_i)P(B_i)} = \frac{P(A|B_k)P(B_k)}{P(A)}
$$

$P(B_k)$ 为先验概率， $P(B_k|A)$ 为后验概率

## 独立性

假设 $(\Omega, \mathcal{A}, P)$ 是一个概率空间，$A, B$ 是两个事件。如果 $P(B)>0$，并且

$$
P(A|B)=P(A)
$$

则 $A$ 和 $B$ 独立。则由条件概率定义，上式可写成

$$
P(AB) = P(A)P(B)
$$