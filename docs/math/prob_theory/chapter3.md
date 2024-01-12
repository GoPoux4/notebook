# 数字特征

## 数学期望

### 离散型随机变量

\[
X \sim
\begin{pmatrix}
    x_1 & x_2 & \cdots & x_n \\
    p_1 & p_2 & \cdots & p_n
\end{pmatrix}
\]

期望

\[
E(X) = \sum_{i=1}^n x_i p_i
\]

#### 绝对可求和

要求

\[
\sum_{i=1}^\infty |x_i| p_i < \infty
\]

即 $E(|X|)$ 绝对收敛。

!!! note
    条件 $E(|X|) < \infty$ 表示期望 $\displaystyle\sum_{i=1}^{\infty}x_ip_i$ 的求和不受求和顺序的影响。

    当这个条件不满足时，期望不存在。

### 连续性随机变量

设 $X \sim p(x)$，则 $X$ 的期望为

\[
EX = \int_{-\infty}^{\infty} x p(x) \ \mathrm{d}x
\]

存在条件：绝对可积，即

\[
\int_{-\infty}^{\infty} |x| p(x) \ \mathrm{d}x < \infty
\]

### 期望的性质

1. $a \leq X \leq b \Rightarrow a \leq EX \leq b$
2. 线性运算、加法定理

    \[
    E(aX + bY) = aEX + bEY
    \]

### 随机变量函数的数学期望

设 $f: \mathbb{R} \rightarrow \mathbb{R}$ 是一个函数，$X$ 是一个随机变量，求 $Ef(X)$。

- $X$ 是离散型随机变量

    \[
    P(X = x_k) = p_k, k = 1, 2, \cdots, N
    \]

    则

    \[
    Ef(X) = \sum_{k=1}^{N} f(x_k) p_k
    \]

- $X$ 是连续型随机变量

    \[
    X \sim p(x), -\infty < x < \infty
    \]

    则

    \[
    Ef(X) = \int_{-\infty}^{\infty} f(x) p(x) \ \mathrm{d}x
    \]

- $X$ 是混合型随机变量

    $X$ 有分布函数 $F(x)$，则
    
    \[
    Ef(X) = \int_{-\infty}^{\infty} f(x) \ \mathrm{d}F(x)
    \]

    （黎曼积分）

## 方差

### 方差的定义

衡量随机变量偏差的大小。

标准差：$\sqrt{E((X - EX)^2)}$

方差：$VarX = E((X - EX)^2)$

### 方差的计算

公式：

\[
VarX = E(X^2) - (EX)^2
\]

### 方差的性质

1. $Var(X + a) = VarX$
2. $Var(aX) = a^2 VarX$
3. $Var(a + bX) = b^2 VarX$
4. $Var(X + Y) = VarX + VarY + 2E(X - EX)(Y - EY) = VarX + VarY + 2Cov(X, Y)$
5. 当 $X$ 和 $Y$ 相互独立时，$Var(X + Y) = VarX + VarY$，独立和的方差等于方差之和。
6. 当 $c \not= EX$ 时，$Var(X) < E((X - c)^2)$

    !!! note "证明"

        \[
        \begin{aligned}
        Var(X) &= E((X - EX)^2) \\
        &= E((X - c - (EX- c))^2) \\
        &= E((X - c)^2 - 2(X - c)(EX - c) + (EX - c)^2) \\
        &= E((X - c)^2) - (EX - c)^2 \\
        &< E((X - c)^2)
        \end{aligned}
        \]

### Chebyshev 不等式

设 $X$ 是一个随机变量，$\forall \varepsilon > 0$，有

\[
P(|X - EX| > \varepsilon) \leq \frac{VarX}{\varepsilon^2}
\]

!!! note "证明"

    设 $X \sim p(x)$，则

    \[
    \begin{aligned}
    P(|X - EX| > \varepsilon) &= \int_{|x-EX| > \varepsilon} p(x) \ \mathrm{d}x \\
    &\leq \int_{|x-EX| > \varepsilon} \frac{(x - EX)^2}{\varepsilon^2} p(x) \ \mathrm{d}x \\
    &\leq \frac{1}{\varepsilon^2} \int_{-\infty}^{\infty} (x - EX)^2 p(x) \ \mathrm{d}x \\
    &= \frac{VarX}{\varepsilon^2}
    \end{aligned}
    \]

!!! info "推广"

    设 $f$ 是单调不减严格正函数，则

    \[
    P(X > \varepsilon) \leq \frac{E(f(X))}{f(\varepsilon)}
    \]

    !!! note "证明"

        \[
        \begin{aligned}
        P(X > \varepsilon) &= \int_{\varepsilon}^{\infty} p(x) \ \mathrm{d}x \\
        &\leq \int_{\varepsilon}^{\infty} \frac{f(x)}{f(\varepsilon)} p(x) \ \mathrm{d}x \\
        &= \frac{1}{f(\varepsilon)} \int_{-\infty}^{\infty} f(x) p(x) \ \mathrm{d}x \\
        &= \frac{E(f(X))}{f(\varepsilon)}
        \end{aligned}
        \]
    
#### 应用

1. 设 $X \sim N(\mu, \sigma^2)$，则

    \[
    P(|X - \mu| > 3\sigma) \leq \frac{\sigma^2}{9\sigma^2} = \frac{1}{9}
    \]

    !!! note "这种方法上界与真实值相差很大"

2. 设 $S_n \sim B(n, p)$，其中 $p$ 未知，想要用 $\dfrac{S_n}{n}$ 来估计 $p$，则试验次数 $n$ 和精度 $\varepsilon$ 的大致关系：

    \[
    \begin{aligned}
    P(|\frac{S_n}{n} - p| > \varepsilon) &\leq \frac{Var(\frac{S_n}{n})}{\varepsilon^2} \\
    &= \frac{Var(S_n)}{n^2 \varepsilon^2} \\
    &= \frac{p(1-p)}{n \varepsilon^2} \\
    &\leq \frac{1}{4n \varepsilon^2}
    \end{aligned}
    \]

    !!! note "改进"

        令 $f(x) = x^4$，则

        \[
        \begin{aligned}
        P(|\frac{S_n}{n} - p| > \varepsilon) &\leq \frac{E(\frac{S_n}{n} - p)^4}{\varepsilon^4} \\
        &= \frac{E(S_n - np)^4}{n^4 \varepsilon^4} \\
        &\approx \frac{c}{n^2 \varepsilon^4}
        \end{aligned}
        \]

3. 当 $VarX = 0$，则

    \[
    P(X = EX) = 1
    \]

    !!! note "证明"

        即证 $P(|X - EX| > 0) = 0$。

        对于 $\forall \varepsilon > 0$，有

        \[
        P(|X - EX| > \varepsilon) \leq \frac{VarX}{\varepsilon^2} = 0
        \]

        因此

        \[
        \begin{aligned}
        P(|X - EX| > 0) &= P(\bigcup_{n=1}^{\infty} \{|X - EX| > \frac{1}{n}\}) \\
        &\leq \sum_{n=1}^{\infty} P(|X - EX| > \frac{1}{n}) \\
        &= 0
        \end{aligned}
        \]

        证毕。

## 协方差

- 均值向量：$\vec{\mu} = (EX, EY)$ 表示向量均值。

- 协方差：假设 $X$ 和 $Y$ 的方差存在，令

    \[
    Cov(X, Y) = E[(X - EX)(Y - EY)]
    \]

    表示 $X$ 和 $Y$ 的协方差。

    计算公式：$Cov(X, Y) = EXY - EXEY$

### Cauchy-Schwarz 不等式

\[
E|X - EX||Y - EY| \leq \sqrt{E(X - EX)^2E(Y - EY)^2}
\]

!!! note "证明"

    不妨设 $EX = EY = 0$，则需要证明

    \[
    E|XY| \leq \sqrt{EX^2 EY^2}
    \]

    对于 $\forall t > 0$，有

    \[
    \begin{aligned}
    0 &\leq E(|X| + t|Y|)^2 \\
    &= EX^2 + 2tE|XY| + t^2 EY^2 , \forall t > 0
    \end{aligned}
    \]

    则 $EX^2 + 2tE|XY| + t^2 EY^2 \geq 0$，故

    \[
    \Delta = 4E^2|XY| - 4EX^2EY^2 \leq 0
    \]

    即

    \[
    E^2|XY| \leq EX^2EY^2 \Rightarrow E|XY| \leq \sqrt{EX^2EY^2}
    \]

### 协方差矩阵

假设 $X$ 和 $Y$ 的方差存在，令

\[
\Sigma =
\begin{pmatrix}
    VarX & Cov(X, Y) \\
    Cov(X, Y) & VarY
\end{pmatrix}
\]

称为 $X$ 和 $Y$ 的协方差矩阵。

#### 协方差矩阵的性质

1. $\Sigma$ 非负定，对于 $\forall x, y \in \mathbb{R}$，有

    \[
    (x, y)
    \begin{pmatrix}
        VarX & Cov(X, Y) \\
        Cov(X, Y) & VarY
    \end{pmatrix}
    \begin{pmatrix}
        x \\
        y
    \end{pmatrix}
    \geq 0
    \]

    !!! note "证明"

        \[
        \begin{aligned}
        (x, y)
        \begin{pmatrix}
            VarX & Cov(X, Y) \\
            Cov(X, Y) & VarY
        \end{pmatrix}
        \begin{pmatrix}
            x \\
            y
        \end{pmatrix}
        &= x^2 VarX + 2xy Cov(X, Y) + y^2 VarY \\
        &= x^2 E(X - EX)^2 + 2xy E[(X - EX)(Y - EY)] + y^2 E(Y - EY)^2 \\
        &= E[x(X - EX) + y(Y - EY)]^2 \\
        &\geq 0
        \end{aligned}
        \]

2. 如果 $X,Y$ 独立，则

    \[
    Cov(X, Y) = 0
    \]

    !!! note "证明"

        \[
        \begin{aligned}
        Cov(X, Y) &= E[(X - EX)(Y - EY)] \\
        &= E(X - EX)E(Y - EY) \\
        &= 0
        \end{aligned}
        \]

!!! info "定义"

    若

    \[
    Cov(X, Y) = 0
    \]

    则称 $X$ 和 $Y$ 不相关。

    如果 $X$ 和 $Y$ 独立，则 $X$ 和 $Y$ 不相关，但反之不成立。

3. $X,Y$ 不相关并不意味着 $X,Y$ 独立。

    !!! example "例"

        设 $\theta \sim U(0, 2\pi)$，$X = \cos \theta$，$Y = \sin \theta$，则

        \[
        EX = EY = 0, VarX = VarY = \frac{1}{2}, Cov(X, Y) = 0
        \]

        但

        \[
        X^2 + Y^2 \equiv 1
        \]

        因此 $X,Y$ 不独立。

!!! example "二元联合正态分布"

    设 $(X,Y) \sim N(\mu_1, \sigma_1^2, \mu_2, \sigma_2^2, \rho)$，则

    \[
    Cov(X, Y) = \rho \sigma_1 \sigma_2
    \]

#### 线性变换后的协方差矩阵

设 $N$ 维随机向量 $\mathbf{X} = (X_1, X_2, \cdots, X_N)^T$ 服从均值 $E(\mathbf{X}) = 0$，协方差矩阵为 $\Sigma_{\mathbf{X}}$ 的正态分布，设随机向量

\[
\mathbf{Y} = \mathbf{A} \mathbf{X}
\]

则随机向量 $\mathbf{Y}$ 的均值和协方差矩阵满足：

\[
\begin{aligned}
E(\mathbf{Y}) = \mathbf{A} E(\mathbf{X}) = 0 \\
\Sigma_{\mathbf{Y}} = \mathbf{A} \Sigma_{\mathbf{X}} \mathbf{A}^T
\end{aligned}
\]

!!! note "证明"

    $E(\mathbf{Y}) = \mathbf{A} E(\mathbf{X}) = 0$ 显然。

    设 $\mathbf{A} = (a_{ij})_{N \times N}$，则对于 $\forall 1 \leq i, j \leq N$，有

    \[
    \begin{aligned}
    Cov(Y_i, Y_j) &= E((Y_i - EY_i)(Y_j - EY_j)) \\
    &= E(Y_i Y_j) \\
    &= E(\sum_{k=1}^N a_{ik} X_k \sum_{l=1}^N a_{jl} X_l) \\
    &= \sum_{k=1}^N \sum_{l=1}^N a_{ik} E(X_k X_l) a_{jl}
    \end{aligned}
    \]

    又有 $Cov(X_i, X_j) = E((X_i - EX_i)(X_j - EX_j)) = E(X_i X_j)$，因此

    \[
    \begin{aligned}
    Cov(Y_i, Y_j) &= \sum_{k=1}^N \sum_{l=1}^N a_{ik} Cov(X_k, X_l) a_{jl} \\
    &= \sum_{l=1}^N a_{jl} \sum_{k=1}^N a_{ik} Cov(X_k, X_l)
    \end{aligned}
    \]

    由于 $\Sigma_{\mathbf{X}} = (Cov(X_i, X_j))_{N \times N}, \Sigma_{\mathbf{Y}} = (Cov(Y_i, Y_j))_{N \times N}$，因此可以看出

    \[
    \Sigma_{\mathbf{Y}} = \mathbf{A} \Sigma_{\mathbf{X}} \mathbf{A}^T
    \]

### 相关系数

定义

\[
\gamma = \frac{Cov(X, Y)}{\sqrt{VarX} \sqrt{VarY}}
\]

称为 $X$ 和 $Y$ 的相关系数，反映 $X$ 和 $Y$ 之间的相关程度。

#### 相关系数的性质

1. 由 Cauchy-Schwarz 不等式可知

    \[
    |\gamma| \leq 1
    \]

2. 当 $\gamma = 1$ 时，存在 $t_0 = \sqrt{\dfrac{VarX}{VarY}}$，使得

    \[
    P(X = t_0(Y - EY) + EX) = 1
    \]

    即 $X$ 和 $Y$ 是正线性相关的。

    !!! note "证明"

        即证 $X - EX - t_0(Y - EY) = 0$。

        由 $\gamma = 1$ 可知

        \[
        Cov(X, Y) = \sqrt{VarX} \sqrt{VarY}
        \]

        取 $t_0 = \sqrt{\dfrac{VarX}{VarY}}$，有

        \[
        \begin{aligned}
        E(X - EX - t_0(Y - EY))^2 &= E(X - EX)^2 + t_0^2 E(Y - EY)^2 - 2t_0 E(X - EX)(Y - EY) \\
        &= VarX + t_0^2 VarY - 2t_0 Cov(X, Y) \\
        &= 2VarX - 2\sqrt{\frac{VarX}{VarY}} (\sqrt{VarX} \sqrt{VarY}) \\
        &= 0
        \end{aligned}
        \]

        因此

        \[
        X - EX - t_0(Y - EY) = E(X - EX - t_0(Y - EY)) = 0
        \]

        证毕。

    类似，当 $\gamma = -1$ 时，存在 $t_0 = -\sqrt{\dfrac{VarX}{VarY}}$，使得

    \[
    P(X = t_0(Y - EY) + EX) = 1
    \]

    即 $X$ 和 $Y$ 是负线性相关的。

3. 有

    \[
    \gamma = 0 \Leftrightarrow \text{不相关}
    \]

    “不相关”可以解释为“不线性相关”。

### 条件期望与全期望公式

#### 离散型随机变量的条件期望

给定 $Y = y_j$，$X$ 的条件期望为

\[
E(X|Y = y_j) = \sum_{i=1}^\infty x_i P(X = x_i | Y = y_j)
\]

要求

\[
\sum_{i=1}^\infty |x_i| P(X = x_i | Y = y_j) < \infty
\]

否则条件期望不存在。

#### 连续型随机变量的条件期望

给定 $Y = y$，$X$ 的条件期望为

\[
E(X|Y = y) = \int_{-\infty}^{\infty} x P(X = x | Y = y) \ \mathrm{d}x
\]

要求

\[
\int_{-\infty}^{\infty} |x| P(X = x | Y = y) \ \mathrm{d}x < \infty
\]

否则条件期望不存在。

!!! example "二元联合正态分布的条件期望"

    $(X, Y) \sim N(\mu_1, \sigma_1^2, \mu_2, \sigma_2^2, \rho)$，则

    给定 $Y = y$，$X$ 的条件分布为

    \[
    X \sim N(\mu_1 + \rho \frac{\sigma_1}{\sigma_2}(y - \mu_2), \sigma_1^2(1 - \rho^2))
    \]

    因此

    \[
    E(X|Y = y) = \mu_1 + \rho \frac{\sigma_1}{\sigma_2}(y - \mu_2)
    \]

    令 $g(y) = E(X|Y = y)$，则

    \[
    \begin{aligned}
    Eg(Y) &= E(\mu_1 + \rho \frac{\sigma_1}{\sigma_2}(Y - \mu_2)) \\
    &= \mu_1 + \rho \frac{\sigma_1}{\sigma_2}(EY - \mu_2) \\
    &= EX
    \end{aligned}
    \]

    即 $E_Y(E_X(X|Y)) = EX$。

### 全期望公式

设 $X,Y$ 是随机变量，令

\[
g(y) = E(X|Y = y)
\]

即 $g(Y) = E(X|Y)$，则

\[
\begin{aligned}
Eg(Y) &= \sum_{j=1}^\infty g(y_j) P(Y = y_j) \\
&= \sum_{j=1}^\infty \sum_{i=1}^\infty x_i P(X = x_i | Y = y_j) P(Y = y_j) \\
&= \sum_{i=1}^\infty x_i \sum_{j=1}^\infty P(X = x_i | Y = y_j) P(Y = y_j) \\
&= \sum_{i=1}^\infty x_i P(X = x_i), \qquad \text{全概率公式} \\
&= EX
\end{aligned}
\]

结论 $E(E(X|Y)) = EX$ 称为全期望公式。

## 矩

k 阶矩（原点矩）：$E(X^k)$

k 阶中心矩：$E[(X - EX)^k]$

!!! question "随机变量的分布是否由其矩唯一确定"

    一般情况下不能。

    !!! note "定理"

        设 $X$ 和 $Y$ 是两个随机变量，如果对于 $\forall k \geq 1$，有

        \[
        E(X^k) = E(Y^k) = m_k < \infty
        \]

        若下列三个条件之一成立：

        1. $\displaystyle\sum_{k=1}^\infty \dfrac{m_{2k}t^{2k}}{(2k)!} < \infty, \exists t > 0$
        2. $\displaystyle\sum_{k=1}^\infty m_{2k}^{-\frac{1}{2k}} < \infty$
        3. $\displaystyle\lim_{k \rightarrow \infty} \sup |m_k|^{\frac{1}{k}} < \infty$

        则 $X$ 和 $Y$ 的分布函数相同。

## 特征函数

设 $X \sim F(x)$，定义

\[
\varphi(t) = E(e^{itX}), t \in \mathbb{R}
\]

为 $X$ 的特征函数。其中 $Ee^{itX} = E\cos tX + iE\sin tX$ 存在且有限。

计算公式：

\[
\begin{aligned}
\varphi(t) &= E(e^{itX}) \\
&= \int_{-\infty}^{\infty} e^{itx} p(x) \ \mathrm{d}x \\
&= \int_{-\infty}^{\infty} e^{itx} \ \mathrm{d}F(x)
\end{aligned}
\]

### 特征函数的性质

1. $\varphi(0) = 1$
2. $|\varphi(t)| \leq 1 = \varphi(0)$

    !!! note "证明"

        \[
        \begin{aligned}
        |\varphi(t)| &= |E(e^{itX})| \\
        &= |\int_{-\infty}^{\infty} e^{itx} \ \mathrm{d}F(x)| \\
        &\leq \int_{-\infty}^{\infty} |e^{itx}| \ \mathrm{d}F(x) \\
        &= 1
        \end{aligned}
        \]

3. $\varphi(-t) = \overline{\varphi(t)}$
4. $\varphi(t)$ 在 $\mathbb{R}$ 上一致连续

    !!! note "证明"

        即证，对于 $\forall \varepsilon > 0$，$\exists \delta > 0, \forall h < \delta, \forall t \in \mathbb{R}$，有

        \[
        |\varphi(t + h) - \varphi(t)| < \varepsilon
        \]

        由 $\varphi(t)$ 的定义可知

        \[
        \begin{aligned}
        |\varphi(t + h) - \varphi(t)| &= |E(e^{i(t+h)X}) - E(e^{itX})| \\
        &= |E(e^{itX}(e^{ihX} - 1))| \\
        &\leq E|e^{ihX} - 1|
        \end{aligned}
        \]

        固定 $\varepsilon > 0$，设 $X \sim F(x)$，则 $\exists M > 0$，使得

        \[
        P(|X| > M) = 1 - F(M) + F(-M) \leq \frac{\varepsilon}{4}
        \]

        则

        \[
        \begin{aligned}
        |\varphi(t + h) - \varphi(t)| &\leq E|e^{ihX} - 1| \\
        &= \int_{-\infty}^{\infty} |e^{ihx} - 1| \ \mathrm{d}F(x) \\
        &= \int_{|x| > M} |e^{ihx} - 1| p(x) \ \mathrm{d}x + \int_{|x| \leq M} |e^{ihx} - 1| p(x) \ \mathrm{d}x \\
        &< I_1 + I_2
        \end{aligned}
        \]

        其中 $I_1 < 2 \cdot \dfrac{\varepsilon}{4} = \dfrac{\varepsilon}{2}$，对于 $I_2$：

        \[
        \begin{aligned}
        |e^{ihx} - 1| &= |e^{i\frac{h}{2}x}||e^{i\frac{h}{2}x} - e^{-i\frac{h}{2}x}| \\
        &= 2|\sin \frac{hx}{2}| \\
        &\leq 2|\frac{hx}{2}| \\
        &= |hx| \\
        &\leq hM
        \end{aligned}
        \]

        因此

        \[
        |\varphi(t + h) - \varphi(t)| < I_1 + I_2 < \frac{\varepsilon}{2} + hM
        \]

        取 $\delta = \dfrac{\varepsilon}{2M}$，则当 $h < \delta$ 时，有

        \[
        |\varphi(t + h) - \varphi(t)| < \frac{\varepsilon}{2} + hM < \frac{\varepsilon}{2} + \frac{\varepsilon}{2} = \varepsilon
        \]

        证毕。

5. Bochner 非负定性

    对任何实数 $t_1, t_2, \cdots, t_n$，复数 $a_1, a_2, \cdots, a_n$，有

    \[
    \sum_{i=1}^n \sum_{j=1}^n a_i \overline{a_j} \varphi(t_i - t_j) \geq 0
    \]

    !!! note "证明"

        令 $X \sim F(x)$，则

        \[
        \begin{aligned}
        \sum_{i=1}^n \sum_{j=1}^n a_i \overline{a_j} \varphi(t_i - t_j) &= \sum_{i=1}^n \sum_{j=1}^n a_i \overline{a_j} E(e^{i(t_i - t_j)X}) \\
        &= E\sum_{i=1}^n \sum_{j=1}^n a_i \overline{a_j} e^{it_iX} \overline{e^{it_jX}} \\
        &= E|\sum_{i=1}^n a_i e^{it_iX}|^2 \\
        &\geq 0
        \end{aligned}
        \]

6. 可微性

    设 $EX$ 存在且 $EX = \mu$，则 $\varphi(t)$ 可微

    则 $\varphi'(0) = i\mu$

    \[
    \begin{aligned}
    \varphi'(t) &= \frac{\mathrm{d}}{\mathrm{d}t} E(e^{itX}) \\
    &= \int_{-\infty}^{\infty} \frac{\mathrm{d}}{\mathrm{d}t} e^{itx} p(x) \ \mathrm{d}x \\
    &= i \int_{-\infty}^{\infty} xe^{itx} \ \mathrm{d}F(x)
    \end{aligned}
    \]

    类似，如果 $E|X|^k < \infty$，则

    \[
    \varphi^{(k)}(0) = i^k \int_{-\infty}^{\infty} x^k e^{itx} \ \mathrm{d}F(x)
    \]

    $\varphi(t)$ 在 0 处可以进行 k 次展开：

    \[
    \begin{aligned}
    \varphi(t) &= \varphi(0) + \varphi'(0)t + \frac{\varphi''(0)}{2!}t^2 + \cdots + \frac{\varphi^{(k)}(0)}{k!}t^k + o(t^k) \\
    &= 1 + iE(X)t - \frac{1}{2}E(X^2)t^2 + \cdots + i^k \frac{E(X^k)}{k!}t^k + o(t^k)
    \end{aligned}
    \]

### 特征函数的运算性质

1. 令 $X$ 的特征函数为 $\varphi_X(t)$，则

    \[
    \varphi_{aX+c}(t) = E(e^{it(aX+c)}) = e^{itc}E(e^{i(at)X}) = e^{itc}\varphi_X(at)
    \]

    !!! example

        设 $X \sim N(0, 1), Y \sim N(\mu, \sigma^2)$，则

        \[
        Y = \sigma X + \mu
        \]

        因此

        \[
        \varphi_Y(t) = e^{it\mu} \varphi_X(\sigma t) = e^{it\mu - \frac{1}{2} \sigma^2 t^2}
        \]

2. 乘法公式

    若 $X, Y$ 相互独立，则 $Z = X + Y$ 的特征函数为

    \[
    \varphi_Z(t) = \varphi_X(t) \varphi_Y(t)
    \]

    !!! note "推广"

        若 $X_1, X_2, \cdots, X_n$ 相互独立，则 $Z = \sum_{i=1}^n X_i$ 的特征函数为

        \[
        \varphi_Z(t) = \prod_{i=1}^n \varphi_{X_i}(t)
        \]

3. 唯一性问题：如果 $\varphi_X(t) = \varphi_Y(t)$，则
    
    \[
    X \overset{d}{=} Y, \qquad F_X \equiv F_Y
    \]

    !!! note "推论"

        1. **连续性情况**
            
            若 $X$ 的特征函数 $\varphi(t)$ 绝对可积，则 $X$ 具有密度函数 $p(x)$，且

            \[
            p(x) = \frac{1}{2\pi} \int_{-\infty}^{\infty} e^{-itx} \varphi(t) \ \mathrm{d}t
            \]

            !!! info "对偶公式"

                已知 $X \sim p(x)$，则

                \[
                \varphi(t) = \int_{-\infty}^{\infty} e^{itx} p(x) \ \mathrm{d}x
                \]
            
            ??? example

                $\varphi(t) = e^{-|t|}$ 是一个特征函数，求其对应的密度函数。
                
                !!! info "解"

                    $\varphi(t)$ 绝对可积，因此存在密度函数 $p(x)$，且

                    \[
                    \begin{aligned}
                    p(x) &= \frac{1}{2\pi} \int_{-\infty}^{\infty} e^{-itx} e^{-|t|} \ \mathrm{d}t \\
                    &= \frac{1}{2\pi} \left[ \int_{0}^{\infty} e^{-itx} e^{-t} \ \mathrm{d}t + \int_{-\infty}^{0} e^{-itx} e^{t} \ \mathrm{d}t \right] \\
                    &= \frac{1}{\pi} \cdot \frac{1}{1 + x^2}
                    \end{aligned}
                    \]

                    故 $X \sim p(x) = \dfrac{1}{\pi} \cdot \dfrac{1}{1 + x^2}$。

        2. **离散性情况**

            设 $\varphi(t)$ 是一个特征函数，若

            \[
            \varphi(t) = \sum_{k=-\infty}^{\infty} a_k e^{itk}
            \]

            且

            \[
            a_k \geq 0, \qquad \sum_{k=-\infty}^{\infty} a_k = 1
            \]

            则

            \[
            P(X = k) = a_k
            \]

            ??? example

                设 $\varphi(t) = \cos t$，则有

                \[
                \varphi(t) = \frac{1}{2} e^{it} + \frac{1}{2} e^{-it}
                \]

                因此

                \[
                P(X = 1) = \frac{1}{2}, \qquad P(X = -1) = \frac{1}{2}
                \]

    利用唯一性，可以利用已知的分布判断随机变量的分布。

    !!! example

        设 $X_k \sim N(\mu_k, \sigma_k^2), k = 1, 2, \cdots, n$，求

        \[
        Z = \sum_{k=1}^n X_k
        \]

        的分布。

        ??? info "解"

            1. 先求 $Z$ 的特征函数

                \[
                \begin{aligned}
                \varphi_Z(t) &= \prod_{k=1}^n \varphi_{X_k}(t) \\
                &= \prod_{k=1}^n e^{it\mu_k - \frac{1}{2} \sigma_k^2 t^2} \\
                &= e^{it\sum_{k=1}^n \mu_k - \frac{1}{2} t^2 \sum_{k=1}^n \sigma_k^2}
                \end{aligned}
                \]

            2. 将其与正态分布的特征函数比较，可得

                \[
                Z \sim N(\sum_{k=1}^n \mu_k, \sum_{k=1}^n \sigma_k^2)
                \]

### 二元随机向量的特征函数

设 $(X, Y)$ 是二维随机向量，其特征函数为

\[
\varphi(t_1, t_2) = E(e^{i(t_1X + t_2Y)})
\]

当 $X, Y$ 相互独立时，有

\[
\varphi(t_1, t_2) = \varphi_X(t_1) \varphi_Y(t_2)
\]

!!! example

    设 $(X, Y) \sim N(\mu_1, \sigma_1^2, \mu_2, \sigma_2^2, \rho)$，求 $\varphi(t_1, t_2)$。

    !!! info "解"

        假设 $(X, Y) \sim N(0, 1, 0, 1, \rho)$，令

        \[
        \Sigma = 
        \begin{pmatrix}
            1 & \rho \\
            \rho & 1
        \end{pmatrix}
        \]

        设

        \[
        \begin{pmatrix}
            U \\
            V
        \end{pmatrix}
        = \Sigma^{-\frac{1}{2}}
        \begin{pmatrix}
            X \\
            Y
        \end{pmatrix}
        \]

        根据 3.2.2 节中的结论，有

        \[
        \begin{aligned}
        E(U, V)' &= \Sigma^{-\frac{1}{2}} E(X, Y)' = 0 \\
        \Sigma_{UV} &= \Sigma^{-\frac{1}{2}} \Sigma (\Sigma^{-\frac{1}{2}})' = \Sigma^{-\frac{1}{2}} \Sigma \Sigma^{-\frac{1}{2}} = I
        \end{aligned}
        \]

        因此 $Cov(U, V) = 0 \Rightarrow \gamma_{UV} = 0$，即 $(U, V) \sim N(0, 1, 0, 1, 0)$，即 $U, V$ 相互独立。则

        \[
        \begin{aligned}
        \varphi_{UV}(t_1, t_2) &= \varphi_U(t_1) \varphi_V(t_2) \\
        &= e^{-\frac{1}{2}(t_1^2 + t_2^2)} \\
        &= e^{-\frac{1}{2}(t_1, t_2) (t_1, t_2)'}
        \end{aligned}
        \]

        因此

        \[
        \begin{aligned}
        \varphi(t_1, t_2) &= E(e^{i(t_1X + t_2Y)}) \\
        &= E(e^{i(t_1, t_2)(X, Y)'}) \\
        &= E(e^{i(t_1, t_2)\Sigma^{\frac{1}{2}}(U, V)'}) \\
        &= \varphi_{UV}((t_1, t_2)\Sigma^{\frac{1}{2}}) \\
        &= e^{-\frac{1}{2}(t_1, t_2)\Sigma^{\frac{1}{2}} \Sigma^{\frac{1}{2}} (t_1, t_2)'} \\
        &= e^{-\frac{1}{2}(t_1, t_2)\Sigma (t_1, t_2)'}
        \end{aligned}
        \]

        回到原问题，已经得到当 $(\xi, \eta) \sim N(0, 1, 0, 1, \rho)$ 时的特征函数为

        \[
        \varphi_{\xi \eta}(t_1, t_2) = e^{-\frac{1}{2}(t_1, t_2)
        \begin{pmatrix}
            1 & \rho \\
            \rho & 1
        \end{pmatrix}
        (t_1, t_2)'}
        \]

        原 $(X, Y) \sim N(\mu_1, \sigma_1^2, \mu_2, \sigma_2^2, \rho)$，有

        \[
        \begin{pmatrix}
            X \\
            Y
        \end{pmatrix}
        =
        \begin{pmatrix}
            \sigma_1 \xi + \mu_1 \\
            \sigma_2 \eta + \mu_2
        \end{pmatrix}
        \]

        因此

        \[
        \begin{aligned}
        \varphi(t_1, t_2) &= E(e^{i(t_1X + t_2Y)}) \\
        &= E(e^{i(t_1(\sigma_1 \xi + \mu_1) + t_2(\sigma_2 \eta + \mu_2))}) \\
        &= e^{i(t_1 \mu_1 + t_2 \mu_2)} E(e^{i(t_1 \sigma_1 \xi + t_2 \sigma_2 \eta)}) \\
        &= e^{i(t_1 \mu_1 + t_2 \mu_2)} \varphi_{\xi \eta}(t_1 \sigma_1, t_2 \sigma_2) \\
        &= e^{i(t_1 \mu_1 + t_2 \mu_2) - \frac{1}{2}(t_1 \sigma_1, t_2 \sigma_2) \Sigma (t_1 \sigma_1, t_2 \sigma_2)'} \\
        \end{aligned}
        \]

## 常见随机变量的期望、方差

### 离散型随机变量

!!! example "退化分布"
    \[
    X \sim \delta(x - c), P(X = c) = 1
    \]

    - **期望**
    
        \[
        E(X) = c
        \]
    
    - **方差**

        \[
        VarX = E(X^2) - (EX)^2 = c^2 - c^2 = 0
        \]

    - **特征函数**

        \[
        \varphi(t) = E(e^{itX}) = e^{itc}
        \]

!!! example "二项 Bernoulli 分布"
    \[
    X \sim B(n, p), P(X = k) = \binom{n}{k} p^k (1-p)^{n-k}
    \]

    - **期望**
    
        \[
        E(X) = \sum_{k=0}^n k \binom{n}{k} p^k (1-p)^{n-k} = np
        \]

    - **方差**

        \[
        \begin{aligned}
        VarX &= E(X^2) - (EX)^2 \\
        &= n(n-1)p^2 + np - n^2p^2 \\
        &= np(1-p)
        \end{aligned}
        \]

        ??? info "过程"
            \[
            \begin{aligned}
            E(X^2) &= \sum_{k=0}^n k^2 \binom{n}{k} p^k (1-p)^{n-k} \\
            &= \sum_{k=1}^n k(k-1) \binom{n}{k} p^k (1-p)^{n-k} + \sum_{k=1}^n k \binom{n}{k} p^k (1-p)^{n-k} \\
            &= \sum_{k=2}^n \frac{n!}{(k - 2)! (n - k)!} p^k (1-p)^{n-k} + np \\
            &= n(n-1)p^2\sum_{k=2}^n \binom{n-2}{k-2} p^{k-2} (1-p)^{(n-2)-(k-2)} + np \\
            &= n(n-1)p^2 + np \\
            \end{aligned}
            \]

        !!! note "使用方差的性质"

            令 $X_i$ 表示第 $i$ 次试验中成功的次数。

            \[
            X_i =
            \begin{cases}
                1, & p \\
                0, & 1 - p
            \end{cases}
            \]

            则

            \[
            VarX = \sum_{i=1}^n VarX_i = np(1-p)
            \]

    - **特征函数**

        \[
        \begin{aligned}
        \varphi(t) &= E(e^{itX}) \\
        &= \sum_{k=0}^n e^{itk} \binom{n}{k} p^k (1-p)^{n-k} \\
        &= \sum_{k=0}^n \binom{n}{k} (pe^{it})^k (1-p)^{n-k} \\
        &= (pe^{it} + 1 - p)^n
        \end{aligned}
        \]

        !!! note "使用每次试验的独立性"

            \[
            \begin{aligned}
            \varphi(t) &= E(e^{itX}) \\
            &= E(e^{it(X_1 + X_2 + \cdots + X_n)}) \\
            &= E(\prod_{i=1}^n e^{itX_i}) \\
            &= \prod_{i=1}^n E(e^{itX_i}) \\
            &= \prod_{i=1}^n (pe^{it} + 1 - p) \\
            &= (pe^{it} + 1 - p)^n
            \end{aligned}
            \]

!!! example "Poisson 分布"
    \[
    X \sim P(\lambda), P(X = k) = \frac{\lambda^k}{k!} e^{-\lambda}
    \]

    - **期望**
    
        \[
        \begin{aligned}
        E(X) &= \sum_{k=0}^\infty k \frac{\lambda^k}{k!} e^{-\lambda} \\
        &= \lambda e^{-\lambda} \sum_{k=0}^\infty \frac{\lambda^k}{k!} \\
        &= \lambda e^{-\lambda} e^\lambda \\
        &= \lambda
        \end{aligned}
        \]

    - **方差**

        \[
        \begin{aligned}
        VarX &= E(X^2) - (EX)^2 \\
        &= \lambda^2 + \lambda - \lambda^2 \\
        &= \lambda
        \end{aligned}
        \]

        ??? info "过程"
            \[
            \begin{aligned}
            E(X^2) &= \sum_{k=0}^\infty k^2 \frac{\lambda^k}{k!} e^{-\lambda} \\
            &= \sum_{k=1}^\infty k(k-1) \frac{\lambda^k}{k!} e^{-\lambda} + \sum_{k=1}^\infty k \frac{\lambda^k}{k!} e^{-\lambda} \\
            &= \sum_{k=2}^\infty \frac{\lambda^k}{(k-2)!} e^{-\lambda} + \lambda \\
            &= \lambda^2 + \lambda
            \end{aligned}
            \]
        
    - **矩**

        \[
        EX(X-1)\cdots(X-k+1) = \lambda^k, k \geq 1
        \]

        !!! note "证明"

            \[
            \begin{aligned}
            EX(X-1)\cdots(X-k+1) &= \sum_{x=0}^\infty x(x-1)\cdots(x-k+1) \frac{\lambda^x}{x!} e^{-\lambda} \\
            &= \sum_{x=k}^\infty \frac{x!}{(x-k)!} \frac{\lambda^x}{x!} e^{-\lambda} \\
            &= \sum_{x=k}^\infty \frac{\lambda^x}{(x-k)!} e^{-\lambda} \\
            &= \lambda^k \sum_{x=k}^\infty \frac{\lambda^{x-k}}{(x-k)!} e^{-\lambda} \\
            &= \lambda^k
            \end{aligned}
            \]
        
        则

        \[
        \begin{aligned}
        E(X^2) &= EX(X-1) + EX = \lambda^2 + \lambda \\
        E(X^3) &= EX(X-1)(X-2) + 3EX(X-1) + EX = \lambda^3 + 3\lambda^2 + \lambda \\
        E(X^4) &= EX(X-1)(X-2)(X-3) + 6EX(X-1)(X-2) + 7EX(X-1) + EX = \lambda^4 + 6\lambda^3 + 7\lambda^2 + \lambda \\
        \cdots
        \end{aligned}
        \]

    - **特征函数**

        \[
        \begin{aligned}
        \varphi(t) &= E(e^{itX}) \\
        &= \sum_{k=0}^\infty e^{itk} \frac{\lambda^k}{k!} e^{-\lambda} \\
        &= e^{-\lambda} \sum_{k=0}^\infty \frac{(\lambda e^{it})^k}{k!} \\
        &= e^{-\lambda} e^{\lambda e^{it}} \\
        &= e^{\lambda(e^{it} - 1)}
        \end{aligned}
        \]

!!! example "几何分布"
    \[
    X \sim G(p), P(X = k) = (1-p)^{k-1} p
    \]
    
    \[
    E(X) = \sum_{k=1}^\infty k (1-p)^{k-1} p = \frac{1}{p}
    \]

!!! example "超几何分布"
    \[
    X \sim H(N, M, n), P(X = k) = \frac{C_M^k C_{N-M}^{n-k}}{C_N^n}
    \]

    - **期望**

        令 $X_i$ 表示第 $i$ 次抽检时的次品个数。

        \[
        X_i =
        \begin{cases}
            1, & p = \frac{M}{N} \\
            0, & 1 - p = \frac{N-M}{N}
        \end{cases}
        \]

        得到 $E(X_i) = \dfrac{M}{N}$，因此
        
        \[
        E(X) = \sum_{k=0}^n E(X_i) = n \frac{M}{N}
        \]

        ??? note "不放回抽样每次抽样同分布不独立"

            设 $A_i$ 表示第 $i$ 次抽检时抽到次品。

            \[
            \begin{aligned}
            p(A_2) &= p(A_1) p(A_2 | A_1) + p(\overline{A_1}) p(A_2 | \overline{A_1}) \\
            &= \frac{M}{N} \times \frac{M-1}{N-1} + \frac{N-M}{N} \times \frac{M}{N-1} \\
            &= \frac{M^2 - M + NM - M^2}{N(N-1)} \\
            &= \frac{M}{N}
            \end{aligned}
            \]

            由数学归纳法可知，$p(A_i) = \dfrac{M}{N}$。

### 连续型随机变量

!!! example "均匀分布"
    \[
    X \sim U(a, b), p(x) = \frac{1}{b-a}
    \]

    - **期望**
    
        \[
        EX = \int_a^b \frac{x}{b-a} \ \mathrm{d}x = \frac{a+b}{2}
        \]
    
    - **方差**

        \[
        \begin{aligned}
        VarX &= E(X^2) - (EX)^2 \\
        &= \int_a^b \frac{x^2}{b-a} \ \mathrm{d}x - \left(\frac{a+b}{2}\right)^2 \\
        &= \frac{b^3 - a^3}{3(b-a)} - \left(\frac{a+b}{2}\right)^2 \\
        &= \frac{(b-a)^2}{12}
        \end{aligned}
        \]

    - **特征函数**

        \[
        \begin{aligned}
        \varphi(t) &= E(e^{itX}) \\
        &= \int_a^b \frac{e^{itx}}{b-a} \ \mathrm{d}x \\
        &= \frac{e^{itb} - e^{ita}}{it(b-a)}
        \end{aligned}
        \]

!!! example "指数分布"
    \[
    X \sim E(\lambda), p(x) = \lambda e^{-\lambda x}, x > 0
    \]
    
    - **期望**

        \[
        \begin{aligned}
        EX &= \int_0^\infty \lambda x e^{-\lambda x} \ \mathrm{d}x \\
        &= -\int_0^\infty x \ \mathrm{d}e^{-\lambda x} \\
        &= \left. -x e^{-\lambda x} \right|_0^\infty + \int_0^\infty e^{-\lambda x} \ \mathrm{d}x \\
        &= \frac{1}{\lambda} \\
        \end{aligned}
        \]

    - **方差**

        \[
        \begin{aligned}
        VarX &= E(X^2) - (EX)^2 \\
        &= \frac{2}{\lambda^2} - \frac{1}{\lambda^2} \\
        &= \frac{1}{\lambda^2}
        \end{aligned}
        \]

        ??? info "过程"
            \[
            \begin{aligned}
            E(X^2) &= \int_0^\infty \lambda x^2 e^{-\lambda x} \ \mathrm{d}x \\
            &= -\int_0^\infty x^2 \ \mathrm{d}e^{-\lambda x} \\
            &= \left. -x^2 e^{-\lambda x} \right|_0^\infty + \int_0^\infty 2x e^{-\lambda x} \ \mathrm{d}x \\
            &= \frac{2}{\lambda^2}
            \end{aligned}
            \]

    - **特征函数**

        \[
        \begin{aligned}
        \varphi(t) &= E(e^{itX}) \\
        &= \int_0^\infty \lambda e^{-\lambda x} e^{itx} \ \mathrm{d}x \\
        &= \int_0^\infty \lambda e^{-(\lambda - it)x} \ \mathrm{d}x \\
        &= -\lambda \left. \frac{e^{-(\lambda - it)x}}{\lambda - it} \right|_0^\infty \\
        &= \frac{\lambda}{\lambda - it}
        \end{aligned}
        \]

!!! example "正态分布"
    \[
    X \sim N(\mu, \sigma^2), p(x) = \frac{1}{\sqrt{2\pi}\sigma} e^{-\frac{(x-\mu)^2}{2\sigma^2}}
    \]

    - **期望**
    
        \[
        EX = \int_{-\infty}^{\infty} \frac{x}{\sqrt{2\pi}\sigma} e^{-\frac{(x-\mu)^2}{2\sigma^2}} \ \mathrm{d}x = \mu
        \]
    
    - **方差**

        \[
        \begin{aligned}
        VarX &= E(X^2) - (EX)^2 \\
        &= \sigma^2 + \mu^2 - \mu^2 \\
        &= \sigma^2
        \end{aligned}
        \]

        ??? info "过程"
            \[
            \begin{aligned}
            E(X^2) &= \int_{-\infty}^{\infty} \frac{x^2}{\sqrt{2\pi}\sigma} e^{-\frac{(x-\mu)^2}{2\sigma^2}} \ \mathrm{d}x \\
            &= \int_{-\infty}^{\infty} \frac{(x-\mu+\mu)^2}{\sqrt{2\pi}\sigma} e^{-\frac{(x-\mu)^2}{2\sigma^2}} \ \mathrm{d}x \\
            &= \int_{-\infty}^{\infty} \frac{(x-\mu)^2}{\sqrt{2\pi}\sigma} e^{-\frac{(x-\mu)^2}{2\sigma^2}} \ \mathrm{d}x + \int_{-\infty}^{\infty} \frac{2\mu(x-\mu)}{\sqrt{2\pi}\sigma} e^{-\frac{(x-\mu)^2}{2\sigma^2}} \ \mathrm{d}x + \int_{-\infty}^{\infty} \frac{\mu^2}{\sqrt{2\pi}\sigma} e^{-\frac{(x-\mu)^2}{2\sigma^2}} \ \mathrm{d}x \\
            &= \sigma^2 + 0 + \mu^2 \\
            &= \sigma^2 + \mu^2
            \end{aligned}
            \]
    
    - **矩**

        设 $X \sim N(0, \sigma^2)$，则

        \[
        E(X^{2k+1}) = 0, E(X^{2k}) = (2k-1)!! \sigma^{2k}
        \]

        !!! note "证明"

            \[
            \begin{aligned}
            E(X^{2k}) &= \int_{-\infty}^{\infty} \frac{x^{2k}}{\sqrt{2\pi}\sigma} e^{-\frac{x^2}{2\sigma^2}} \ \mathrm{d}x \\
            &= -\int_{-\infty}^{\infty} \frac{x^{2k-1}\sigma}{\sqrt{2\pi}} \ \mathrm{d}e^{-\frac{x^2}{2\sigma^2}} \\
            &= -\left. \frac{x^{2k-1}\sigma}{\sqrt{2\pi}} e^{-\frac{x^2}{2\sigma^2}} \right|_{-\infty}^{\infty} + \int_{-\infty}^{\infty} \frac{(2k-1)x^{2k-2}\sigma^2}{\sqrt{2\pi}\sigma} e^{-\frac{x^2}{2\sigma^2}} \ \mathrm{d}x \\
            &= (2k-1)\sigma^2 E(X^{2k-2}) \\
            &= (2k-1)!! \sigma^{2k}
            \end{aligned}
            \]

            对于 $X^{2k+1}$，由于 $X$ 是偶函数，因此

            \[
            E(X^{2k+1}) = 0
            \]

    - **特征函数**

        !!! info

            对于 $X \sim N(0, 1)$，有

            \[
            \int_{-\infty}^\infty \frac{1}{\sqrt{2\pi}} e^{-\frac{x^2}{2}} \ \mathrm{d}x = 1
            \]

            得到

            \[
            \int_{-\infty}^\infty e^{-\frac{x^2}{2}} \ \mathrm{d}x = \sqrt{2\pi}
            \]

        对于 $X \sim N(\mu, \sigma^2)$，有

        \[
        \begin{aligned}
        \varphi(t) &= E(e^{itX}) \\
        &= \int_{-\infty}^\infty \frac{1}{\sqrt{2\pi}\sigma} e^{-\frac{(x-\mu)^2}{2\sigma^2}} e^{itx} \ \mathrm{d}x \\
        &= \int_{-\infty}^\infty \frac{1}{\sqrt{2\pi}\sigma} e^{-\frac{(x-\mu)^2 - 2i\sigma^2tx}{2\sigma^2}} \ \mathrm{d}x \\
        &= \frac{1}{\sqrt{2\pi}\sigma} \int_{-\infty}^\infty e^{-\frac{(x-\mu - i\sigma^2t)^2 + \sigma^4t^2 - 2\mu i\sigma^2t}{2\sigma^2}} \ \mathrm{d}x \\
        &= \frac{1}{\sqrt{2\pi}} e^{\mu it - \frac{1}{2}\sigma^2t^2} \int_{-\infty}^\infty e^{-\frac{1}{2}(\frac{x-\mu - i\sigma^2t}{\sigma})^2} \ \mathrm{d}\frac{x-\mu - i\sigma^2t}{\sigma} \\
        &= e^{\mu it - \frac{1}{2}\sigma^2t^2}
        \end{aligned}
        \]

        得到

        \[
        \varphi(t) = e^{\mu it - \frac{1}{2}\sigma^2t^2}
        \]

        对于标准正态分布，有 $\varphi(t) = e^{-\frac{1}{2}t^2}$。