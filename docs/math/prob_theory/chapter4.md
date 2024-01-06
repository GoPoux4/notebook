# 概率极限定理

## 经典概率极限定理

下面三个定理只涉及 Bernoulli 试验，$X \sim B(n, p)$。

\[
\begin{aligned}
P(X = k) &= \binom{n}{k} p^k (1 - p)^{n - k} \\
E(X) &= np \\
Var(X) &= np(1 - p)
\end{aligned}
\]

### Bernoulli 大数定律

!!! info "依概率收敛"

    设 $X_n, n \geq 1$ 是一列随机变量，$X$ 是另一个随机变量，如果对于任意的 $\varepsilon > 0$，有

    \[
    P(\omega: \left| X_n(\omega) - X(\omega) \right| > \varepsilon) \rightarrow 0, \quad n \rightarrow \infty
    \]

    则称 $X_n$ 依概率收敛到 $X$，记作 $X_n \xrightarrow{P} X$。

设 $0 < p < 1$，$S_n \sim B(n, p)$，则

\[
P(\omega: \left| \frac{S_n(\omega)}{n} - p \right| > \varepsilon) \rightarrow 0, \quad n \rightarrow \infty
\]

即 **频率接近概率真值** 的数学解释。即 $\dfrac{S_n}{n}$ 依概率收敛到 $p$。

\[
\frac{S_n}{n} \xrightarrow{P} p, \quad n \rightarrow \infty
\]

!!! info

    不能使用 $\varepsilon - N$ 语言来描述，因为随机变量取值范围和 $n$ 无关。

    \[
    P(\omega: \left| \frac{S_n(\omega)}{n} - p \right| > \varepsilon) = \sum_{k: \left| \frac{k}{n} - p \right| > \varepsilon} \binom{n}{k} p^k (1 - p)^{n - k} > 0
    \]

    即总会发生 $\left| \frac{S_n}{n} - p \right| > \varepsilon$。

### de Moivre-Laplace 中心极限定理

!!! info "依分布收敛"

    设 $X_n, n \geq 1$ 是一列随机变量，相应分布函数为 $F_n(x)$，$X$ 是另一个随机变量，分布函数为 $F(x)$，如果对于任意的 $F(x)$ 的连续点 $x$，有

    \[
    F_n(x) \rightarrow F(x), \quad n \rightarrow \infty
    \]

    则称 $X_n$ 依分布收敛到 $X$，记作 $X_n \xrightarrow{D} X$ 或 $F_n \xrightarrow{D} F$。

假设 $S_n \sim B(n, p)$，则

\[
P(\frac{S_n - np}{\sqrt{np(1 - p)}} \leq x) \asymp \int^x_{-\infty} \frac{1}{\sqrt{2\pi}} e^{-\frac{t^2}{2}} \ \mathrm{d} t, \quad n \rightarrow \infty
\]

??? note "$\asymp$ 意为渐进相等"

    即 $f \asymp g \Leftrightarrow \exists C, D > 0$，使得 $C|g| \leq |f| \leq D|g|$。

其中 $\dfrac{S_n - np}{\sqrt{np(1 - p)}}$ 称为 **规范化随机变量**，记作 $Z_n$，满足 $EZ_n = 0, VarZ_n = 1$；等式右侧为 **标准正态分布** 的分布函数 $\Phi(x)$。即

\[
F_{Z_n}(x) \xrightarrow{D} \Phi(x), \quad n \rightarrow \infty
\]

即满足 Bernoulli 二项分布的规范化随机变量分布渐进于标准正态分布。

!!! example "应用"

    \[
    \begin{aligned}
    P(a \leq S_n \leq b) &= P(\frac{a - np}{\sqrt{np(1 - p)}} \leq \frac{S_n - np}{\sqrt{np(1 - p)}} \leq \frac{b - np}{\sqrt{np(1 - p)}}) \\
    &\asymp \Phi(\frac{b - np}{\sqrt{np(1 - p)}}) - \Phi(\frac{a - np}{\sqrt{np(1 - p)}})
    \end{aligned}
    \]

??? note "证明（$p=1/2$ 的情况）"

    出发点

    \[
    P(a \leq \frac{S_n - np}{\sqrt{np(1 - p)}} \leq b) = \sum_{k: a \leq \frac{k - np}{\sqrt{np(1 - p)}} \leq b} \binom{n}{k} p^k (1 - p)^{n - k}
    \]

    :TODO:

    （szg：感兴趣的了解）

### Poisson 极限定理

设 $S_n \sim B(n, p_n)$，若 $np_n \rightarrow \lambda$，则

\[
P(S_n = k) \rightarrow \frac{\lambda^k}{k!} e^{-\lambda}, \quad n \rightarrow \infty
\]

!!! note "证明"

    由 $np_n \rightarrow \lambda$，可知 $p_n = \frac{\lambda}{n} + o(\frac{1}{n})$。

    \[
    \begin{align}
    P(S_n = k)
    =& {n \choose k}p_n^k(1-p_n)^{n-k} \\
    =& \frac{1}{k!} \cdot n(n-1)\cdots(n-k+1)\cdot \frac{1}{n^k}\cdot (np_k)^k \cdot (1 - \frac{\lambda}{n} + o(\frac{1}{n}))^{n-k} \\
    =& \left[(1-\frac{1}{n})(1-\frac{2}{n})\cdots(1-\frac{k-1}{n})\right] \left[\frac{\lambda^k}{k!}\right] \left[(1-\frac{\lambda}{n})^{n-k}\right] \\
    \to & \frac{\lambda^k}{k!} e^{-\lambda}, \quad n\to \infty
    \end{align}
    \]

## 经典极限定理的推广

### Bernoulli 大数定理的推广

#### Chebyshev 大数定律

!!! info "Chebyshev 不等式"

    设 $X$ 是一个随机变量，则对于任意的 $\varepsilon > 0$，有

    \[
    P(\left|X - EX \right| > \epsilon) \leq \frac{VarX}{\varepsilon^2}
    \]

    ??? note "证明 Bernoulli 大数定理"

        \[
        \begin{aligned}
        P(\left| \frac{S_n}{n} - p \right| > \varepsilon) &= P(\left| S_n - np \right| > n\varepsilon) \\
        &\leq \frac{np(1 - p)}{n^2\varepsilon^2} \\
        &= \frac{p(1 - p)}{n\varepsilon^2} \\
        &\rightarrow 0, \quad n \rightarrow \infty
        \end{aligned}
        \]

假设 $X_k, k \geq 1$ 是一列随机变量，且 $EX_k = \mu_k$，设 $S_n = \sum_{k=1}^nX_k$，若

\[
\frac{VarS_n}{n^2} \rightarrow 0, \quad n \rightarrow \infty 
\]

则

\[
\frac{S_n}{n} - \frac{\sum_{k=1}^n\mu_k}{n} \xrightarrow{P} 0, \quad n \rightarrow \infty
\]

特别的，若 $EX_k = \mu$，则

\[
\frac{S_n}{n} \xrightarrow{P} \mu, \quad n \rightarrow \infty
\]

!!! note "证明"

    首先有

    \[
    ES_n = E\sum_{k=1}^nX_k = \sum_{k=1}^nEX_k = \sum_{k=1}^n\mu_k
    \]

    对于 $\forall \varepsilon > 0$，有

    \[
    \begin{aligned}
    P(\left| \frac{S_n}{n} - \frac{\sum_{k=1}^n\mu_k}{n} \right| > \varepsilon)
    &= P(\left| S_n - ES_n \right| > n\varepsilon) \\
    &\leq \frac{VarS_n}{n^2\varepsilon^2}, \quad \text{Chebyshev 不等式} \\
    &\rightarrow 0, \quad n \rightarrow \infty, \quad \text{前提条件}
    \end{aligned}
    \]

揭示了 **样本的均值渐近于总体的均值**，且没有独立性要求。

缺点：要求方差存在。

!!! example "应用"

    设 $\xi_k, k \geq 1$ 是一列独立的随机变量，有 $\xi_1 \equiv 0$，且当 $k \geq 2$ 有

    \[
    \begin{aligned}
    P(\xi_k = k) = P(\xi_k = -k) &= \frac{1}{2k \log k} \\
    P(\xi_k = 0) &= 1 - \frac{1}{k \log k}
    \end{aligned}
    \]

    记 $S_n = \sum_{k=1}^n\xi_k$，证明

    \[
    \frac{S_n}{n} \xrightarrow{P} 0, \quad n \rightarrow \infty
    \]

    ??? note "证明"

        由于 $\xi_k$ 不是同分布，故使用 Chebyshev 大数定律。

        有 $E\xi_k = 0$，$Var\xi_k = \frac{k}{\log k}$，故

        \[
        \frac{VarS_n}{n^2} = \frac{1}{n^2} \sum_{k=1}^n\frac{k}{\log k} \sim \frac{1}{n^2} \frac{n^2}{\log n} \rightarrow 0, \quad n \rightarrow \infty
        \]

        故

        \[
        \frac{S_n}{n} \xrightarrow{P} 0, \quad n \rightarrow \infty
        \]

#### Khintchine 大数定律

假设 $X_k, k \geq 1$ 是一列独立同分布的随机变量，$EX_k = \mu$，设 $S_n = \sum_{k=1}^nX_k$，则

\[
\frac{S_n}{n} \xrightarrow{P} \mu, \quad n \rightarrow \infty
\]

### de Moivre-Laplace 中心极限定理的推广

#### Levy-Feller 中心极限定理

设 $X_k, k \geq 1$ 是一列独立同分布的随机变量，$EX_k = \mu, VarX_k = \sigma^2$，设 $S_n = \sum_{k=1}^nX_k$，则

\[
P(\frac{S_n - n\mu}{\sigma\sqrt{n}} \leq x) \rightarrow \Phi(x), \quad \forall x, n \rightarrow \infty
\]

即

\[
\frac{S_n - n\mu}{\sigma\sqrt{n}} \xrightarrow{D} N(0, 1), \quad n \rightarrow \infty
\]

说明测量误差可以用正态分布描述。

#### Lyapunov 中心极限定理

设 $X_k, k \geq 1$ 是一列独立随机变量，$EX_k = \mu_k, VarX_k = \sigma_k^2$，设

\[
S_n = \sum_{k=1}^nX_k, B_n = \sum_{k=1}^n\sigma_k^2
\]

若

\[
\begin{aligned}
B_n &\rightarrow 0, \\
E|X_k|^3 &< \infty, \quad \forall k \\
\frac{\sum_{k=1}^nE|X_k|^3}{B_n^{3/2}} &\rightarrow 0, \quad n \rightarrow \infty
\end{aligned}
\]

则

\[
P(\frac{\sum_{k=1}^n(\xi_k - \mu_k)}{\sqrt{B_n}} \leq x) \rightarrow \Phi(x), \quad \forall x, n \rightarrow \infty
\]

即

\[
\frac{\sum_{k=1}^n(\xi_k - \mu_k)}{\sqrt{B_n}} \xrightarrow{D} N(0, 1), \quad n \rightarrow \infty
\]

!!! example

    假设 $\xi_k, k \geq 1$ 是一列独立随机变量，且

    \[
    P(\xi_k = 1) = p_k, \quad P(\xi_k = 0) = 1 - p_k, \quad 0 < p_k < 1
    \]

    若

    \[
    B_n = \sum_{k=1}^n p_k(1 - p_k) \rightarrow 0, \quad n \rightarrow \infty
    \]

    则

    \[
    \frac{\sum_{k=1}^n(\xi_k - p_k)}{\sqrt{\sum_{k=1}^n p_k(1 - p_k)}} \xrightarrow{D} N(0, 1), \quad n \rightarrow \infty
    \]

## 依概率收敛


设 $X_n, n \geq 1$ 是一列随机变量，$X$ 是另一个随机变量，如果对于任意的 $\varepsilon > 0$，有

\[
P(\omega: \left| X_n(\omega) - X(\omega) \right| > \varepsilon) \rightarrow 0, \quad n \rightarrow \infty
\]

则称 $X_n$ 依概率收敛到 $X$，记作 $X_n \xrightarrow{P} X$。（$X$ 可以是常数）

### 判别法则

若存在 $r > 0$，使得

\[
E|X_n - X|^r \rightarrow 0, \quad n \rightarrow \infty
\]

则 $X_n \xrightarrow{P} X$。

### 基本性质

1. 唯一性
   
    若 $X_n \xrightarrow{P} X$，$X_n \xrightarrow{P} Y$，则 $X = Y$。

2. 运算性质：若 $X_n \xrightarrow{P} X$，$Y_n \xrightarrow{P} Y$，则

    - $X_n + Y_n \xrightarrow{P} X + Y$
    - $X_nY_n \xrightarrow{P} XY$
    - 若 $Y \neq 0$，则 $\dfrac{X_n}{Y_n} \xrightarrow{P} \dfrac{X}{Y}$
    - 若 $g$ 连续，则 $g(X_n) \xrightarrow{P} g(X)$

## 依分布收敛

设 $X_n, n \geq 1$ 是一列随机变量，相应分布函数为 $F_n(x)$，$X$ 是另一个随机变量，分布函数为 $F(x)$，如果对于任意的 $F(x)$ 的连续点 $x$，有

\[
F_n(x) \rightarrow F(x), \quad n \rightarrow \infty
\]

则称 $X_n$ 依分布收敛到 $X$，记作 $X_n \xrightarrow{D} X$ 或 $F_n \xrightarrow{D} F$。

### 判别法则

**Levy 连续性定理**：设 $X_n, n \geq 1$ 是一列随机变量，具有相应特征函数 $\varphi_n(t)$，$X$ 是另一个随机变量，特征函数为 $\varphi(t)$，则

\[
X_n \xrightarrow{D} X \Leftrightarrow \varphi_n(t) \rightarrow \varphi(t), \quad t \in \mathbb{R}, n \rightarrow \infty
\]

!!! note "Levy 连续性定理的另一种形式"

    若 $X_n, n \geq 1$ 是一列随机变量，具有相应密度函数 $\varphi_n(t)$，若

    \[
    \varphi_n(t) \rightarrow \varphi(t), \quad t \in \mathbb{R}, n \rightarrow \infty
    \]

    且 $\varphi(t)$ 在 $0$ 处连续，则 $\varphi(t)$ 一定是特征函数，对应的随机变量 $X$ 满足

    \[
    X_n \xrightarrow{D} X, \quad n \rightarrow \infty
    \]

### 基本性质

1. 依概率收敛意味着依分布收敛

    若 $X_n \xrightarrow{P} X$，则 $X_n \xrightarrow{D} X$。

2. 依分布收敛不意味着依概率收敛

    !!! note "特殊情况"

        \[
        X_n \xrightarrow{D} c \Rightarrow X_n \xrightarrow{P} c
        \]

3. 运算性质

    1. 线性运算

        - 设 $X_n \xrightarrow{D} X$，$b_n \rightarrow b$，则 $X_n + b_n \xrightarrow{D} X + b$
        - 设 $X_n \xrightarrow{D} X$，$a_n \rightarrow a$，则 $a_nX_n \xrightarrow{D} aX$
    
    2. 设 $X_n \xrightarrow{D} X$，$Y_n \xrightarrow{D} c$，则 $X_nY_n \xrightarrow{D} cX$
    3. 若 $g$ 连续，则 $g(X_n) \xrightarrow{D} g(X)$