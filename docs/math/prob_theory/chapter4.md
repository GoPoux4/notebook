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

!!! example

    \[
    \begin{aligned}
    P(a \leq S_n \leq b) &= P(\frac{a - np}{\sqrt{np(1 - p)}} \leq \frac{S_n - np}{\sqrt{np(1 - p)}} \leq \frac{b - np}{\sqrt{np(1 - p)}}) \\
    &\asymp \Phi(\frac{b - np}{\sqrt{np(1 - p)}}) - \Phi(\frac{a - np}{\sqrt{np(1 - p)}})
    \end{aligned}
    \]

### Poisson 极限定理

## 经典极限定理的推广

### Chebyshev 和 Khintchine 大数定律

### Levy-Feller 中心极限定理

## 依概率收敛

## 依分布收敛