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

### 两个事件独立

假设 $(\Omega, \mathcal{A}, P)$ 是一个概率空间，$A, B$ 是两个事件。如果 $P(B)>0$，并且

$$
P(A|B)=P(A)
$$

则 $A$ 和 $B$ 独立。则由条件概率定义，上式可写成

$$
P(AB) = P(A)P(B)
$$

!!! note
    * $P(B)=0$ 时乘积公式仍有意义。
    * $A, B$ 关系对等。
    * 若 $A, B$ 独立，则 $A, \bar B$ 独立， $\bar A, B$ 独立， $\bar A, \bar B$ 独立
    * 与加法（并）的区别：

        $$
        \begin{align}
            P(A+B) &= P(A) + P(B) \qquad &&A \cap B = \emptyset \\
            P(AB) &= P(A)P(B) \qquad &&A, B \text{独立}
        \end{align}
        $$

### 三个事件独立

若 $A, B, C$ 是三个事件，若 **$A, B, C$ 两两相互独立** 且

$$
P(ABC) = P(A)P(B)P(C)
$$

则称 $A,B,C$ 相互独立。

!!! warning
    两两独立不一定相互独立，相互独立一定两两独立。

    !!! example
        一个正四面体的三面分别涂成红、黑、白三色，另一面涂上三种颜色。现随机一扔，记底面涂有红、黑、白分别为事件 $A,B,C$。

        可得

        $$
        P(A) = P(B) = P(C) = \frac{1}{2}
        $$

        且
        
        $$
        \begin{align}
        P(AB) = \frac{1}{4} = P(A)P(B) \\
        P(AC) = \frac{1}{4} = P(A)P(C) \\
        P(BC) = \frac{1}{4} = P(B)P(C)
        \end{align}
        $$

        则 $A,B,C$ 两两独立。但

        $$
        P(ABC) = \frac{1}{4} \not = P(A)P(B)P(C)
        $$

        故 $A,B,C$ 不相互独立。原因在于若 $AB$ 发生，则 $C$ 一定发生，失去了独立性。

!!! note
    若 $A,B,C$ 相互独立，则 $\bar A,B,C$ 相互独立，$A+B, C$ 相互独立，等等类似关系成立。

### m 个事件相互独立

假设 $A_k, 1\leq k\leq m$ 是 $m$ 个事件，若 $A_k$ 中任意 $r < m$ 个都相互独立，且

$$
P(\bigcap_{1\leq k\leq m}A_k) = \prod_{1\leq k\leq m}P(A_k)
$$

则 $A_k, 1\leq k\leq m$ 相互独立。

### 二项试验

又称 n-重 Bernonlli 试验。

* 试验 $E$ 包含若干个基本结果。
* 事件 $A$ 为具有某种属性的基本结果集合，发生的概率为 $P(A) = p_A$

独立重复进行 $n$ 次试验，并观察记录结果。判断 $A$ 发生与否，统计 $A$ 发生的次数 $n_A$.

!!! note "概率模型"
    每次试验 $A$ 发生记为 $1$，不发生记为 $0$。

    独立重复 $n$ 次试验，所得结果为

    $$
    \omega = (\omega _1, \dots, \omega _n) \qquad \omega_i =0, 1
    $$

    用 $\Omega_n$ 表示所有 $\omega$ 的全体

    $$
    \Omega_n = \{\omega = (\omega _1, \dots, \omega _n), \omega_i =0, 1\}
    $$

    其中每个 $\omega$ 出现的概率为

    $$
    P_n(\{\omega\}) = p_A^{\sum \omega_i}(1 - p_A)^{n - \sum \omega_i}
    $$

    由此得到概率空间 $(\Omega_n, P_n)$，即为 n-重 Bernonlli 试验的概率模型。

考虑事件 $B = \{\omega: n_A(\omega) = k\}$，则

$$
P_n(B) = {n \choose k}p_A^k(1 - p_A)^{n - k}
$$

### 乘积概率空间

考虑两个试验 $E_1, E_2$，其概率空间分别为 $(\Omega_1, P_1), (\Omega_2, P_2)$

现同时独立做两个试验，记录其基本结果 $\omega = (\omega_1, \omega_2)$

考虑 $E_1, E_2$ 所有基本结果的全体

$$
\Omega = \{\omega = (\omega_1, \omega_2), \quad \omega_1 \in \Omega_1, \omega_2 \in \Omega_2\}
$$

考虑事件

$$
A = A_1 \times A_2 = \{\omega = (\omega_1, \omega_2), \quad \omega_1 \in A_1, \omega_2 \in A_2\}
$$

定义其概率

$$
P(A) = P(A_1)P(A_2)
$$

得到新的概率空间：乘积概率空间 $(\Omega, P)$

## 补充说明

### 概率的可加性

如果 $A_n, n \geq 1$ 互不相交，则

$$
P(\sum_{n = 1}^\infty A_n) = \sum_{n = 1}^\infty P(A_n)
$$

### 概率的连续性

!!! note "事件的极限"
    * 假设 $A_n$ 是一列增加事件

        $$
        A_1 \subseteq \dots \subseteq A_n \subseteq \dots
        $$

        定义

        $$
        \lim_{n \to \infty} A_n = \bigcup_{n=1}^\infty A_n
        $$

    * 假设 $A_n$ 是一列递减事件

        $$
        A_1 \supseteq \dots \supseteq A_n \supseteq \dots
        $$

        定义

        $$
        \lim_{n \to \infty} A_n = \bigcap_{n=1}^\infty A_n
        $$

假设 $A_n$ 是一列增加事件，则

$$
P(\lim_{n\to \infty} A_n) = \lim_{n\to \infty}P(A_n)
$$

!!! proof
    记 $B_1 = A_1, B_k = A_k - A_{k - 1}$ ，则 $B_k$ 互不相交，则

    $$
    \begin{align}
    P(\lim_{n\to\infty}A_n) &= P(\sum_{n=1}^\infty B_n) \\
                            &= \sum_{n=1}^\infty P(B_n) \\
                            &= \sum_{n=1}^\infty P(A_n) - P(A_{n - 1}) \\
                            &= \lim_{n\to\infty} P(A_n)
    \end{align}
    $$

### 条件概率具有概率的运算性质

假设 $(\Omega, \mathcal{A}, P)$ 是概率空间， $B$ 是一个事件， $P(B)>0$

对于任意事件 $A \in \mathcal{A}$ ，条件概率 $P(A|B) = \frac{P(AB)}{P(B)}$

将

$$
P(\cdot | B) : \mathcal{A} \mapsto [0, 1]
$$

视作一个概率。

则有

$$
\begin{align}
P(A_1+A_2|B) = P(A_1|B) + P(A_2|B) \\
P(A_1-A_2|B) = P(A_1|B) - P(A_2|B)
\end{align}
$$

等等类似性质。