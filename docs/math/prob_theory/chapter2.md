# 随机变量与分布函数

## 随机变量与分布函数

随机变量是定义在概率空间上取实数值的可测函数

!!! example "随机变量"
    * 掷骰子出现的点数
    * 测量灯泡的寿命
    * 测量物体的长度

### 离散型随机变量与分布列

取有限个或者可列个值的随机变量

假设 $(\Omega, \mathcal{A}, P)$ 是概率空间，定义 $X:\Omega \mapsto \mathbb{R}$ .

取值 $x_1, \cdots, x_N, N\leq\infty$

取每个值的概率大小

$$
P(X=x_i)=p_i, \quad i = 1, 2, \cdots, N
$$

分布列：

$$
X \sim
\begin{pmatrix}
x_1 & x_2 & \cdots & x_i & \cdots & x_N\\
p_1 & p_2 & \cdots & p_i & \cdots & p_N
\end{pmatrix}
$$

!!! note
    * $p_i$

        $$
        p_i > 0, \sum_{i=1}^N p_i = 1
        $$
    
    * 对于任意 Borel 集 $B$ ，

        $$
        P(X\in B) = \sum_{i:x_i\in B}p_i
        $$

        特别，

        $$
        \begin{align}
            P(X \leq x) &= \sum_{i:x_i \leq x}p_i \\
            P(X > x) &= \sum_{i:x_i > x}p_i \\
            P(a < X \leq b) &= \sum_{i:a< x_i \leq b}p_i
        \end{align}
        $$

#### 离散型随机变量的典型例子

!!! example "退化分布"
    $$
    X \sim
    \begin{pmatrix}
        c \\
        1
    \end{pmatrix}
    $$

    常数可以看作退化随机变量

!!! example "两点分布"
    $$
    X \sim
    \begin{pmatrix}
        1 & 0\\
        p & 1 - p
    \end{pmatrix}
    \quad 0<p<1
    $$

    适用于描述“正面、反面”、“成功、失败”等随机现象

!!! example "二项分布"
    $$
    X \sim
    \begin{pmatrix}
        0 & 1 & \cdots & k & \cdots & n\\
        (1-p)^n & np(1-p)^{n-1} & \cdots & {n \choose k}p^k(1-p)^{n-k} & \cdots & p^n
    \end{pmatrix}
    \quad 0<p<1
    $$

    简记 $X \sim B(n, p)$

    * 二项分布适用于 n 重 Bernoulli 试验
    * 二项展开系数

        $$
        (p+q)^n = \sum_{k = 0}^n{n \choose k}p^kq^{n-k}
        $$
    
    * $X$ 最可能的值

        $$
        \begin{align}
        \frac{p_k}{p_{k+1}}
        =& \frac{{n \choose k}p^k(1-p)^{n-k}}{{n \choose k + 1}p^{k+1}(1-p)^{n-k-1}} \\
        =& \frac{(k+1)(1-p)}{(n-k)p} \\
        &\begin{cases}
            <1, &\quad k+1 < (n + 1)p \\
            >1, &\quad k+1 > (n + 1)p
        \end{cases}
        \end{align}
        $$

!!! example "Poisson 分布"
    $$
    X \sim
    \begin{pmatrix}
        0 & 1 & \cdots & k & \cdots\\
        e^{-\lambda} & \lambda e^{-\lambda} & \cdots & \frac{\lambda^k}{k!}e^{-\lambda} & \cdots
    \end{pmatrix}
    \quad \lambda > 0
    $$

    简记 $X \sim \mathcal{P}(\lambda)$

    $X$ 取非负整数值，

    $$
    P(X=k) = \frac{\lambda^k}{k!}e^{-\lambda}, \quad k=0, 1, \cdots
    $$

    !!! note
        * $\sum p_i = 1$
        * Poisson 分布是 Poisson 过程的基础，用于描述随机服务系统
        * 当 $k$ 足够大

            $$
            P(X\geq k) = \sum_{l = k}^\infty\frac{\lambda^l}{l!}e^{-\lambda} \asymp \frac{\lambda^k}{k!}e^{-\lambda}
            $$

            $X$ 只集中在小值中。稀有事件。

    !!! note "Poisson 分布和二项分布之间的关系(Poisson 极限定理)"
        假设 $S_n \sim B(n, p_n)$。当 $n \to \infty, np_n \to \lambda > 0$。对于任意 $k \leq 0$

        $$
        P(S_n = k) \to \frac{\lambda^k}{k!}e^{-\lambda} = P(X = k), \quad n \to \infty
        $$

        ???+ proof
            $$
            \begin{align}
            P(S_n = k)
            =& {n \choose k}p_n^k(1-p_n)^{n-k} \\
            =& \frac{1}{k!} \cdot n(n-1)\cdots(n-k+1)\cdot \frac{1}{n^k}\cdot (np_k)^k \cdot (1 - \frac{\lambda}{n} + o(\frac{1}{n}))^{n-k} \\
            =& \left[(1-\frac{1}{n})(1-\frac{2}{n})\cdots(1-\frac{k-1}{n})\right] \left[\frac{\lambda^k}{k!}\right] \left[(1-\frac{\lambda}{n})^{n-k}\right] \\
            \to & \frac{\lambda^k}{k!} e^{-\lambda}, \quad n\to \infty
            \end{align}
            $$

!!! example "几何分布"
    随机试验 $E$ 和事件 $A$ ，$P(A) = p, 0<p<1$。独立重复 $E$ 直到 $A$ 发生，记所做的试验次数记为 $X$

    $$
    P(X=k) = p(1-p)^{k-1}
    $$

    其中

    $$
    \sum_{k=1}^{\infty} p(1-p)^{k-1} = \frac{p}{1-(1-p)} = 1
    $$

    分布列：

    $$
    X \sim
    \begin{pmatrix}
    1 & 2 & \cdots & k & \cdots \\
    p & p(1-p) & \cdots & p(1-p)^{k-1} & \cdots
    \end{pmatrix}
    $$

!!! example "超几何分布"
    $N$ 件产品中有 $M$ 件次品，随机抽样 $n<N$ 件，用 $X$ 表示 $n$ 件产品中次品数。

    $0\leq X \leq \min\{M,n\}$

    $$
    P(X=k) = \frac{{M \choose k}{N - M \choose n - k}}{{N \choose n}}
    $$

### 连续型随机变量与密度函数

连续型随机变量特点：

* 随机变量取值是一个或几个区间
* 存在函数 $p(x)$

    $$
    p(x)\geq 0, \quad \int_{-\infty}^{\infty}p(x)\text{d}x = 1
    $$

    使得对任何 Borel 集 $B$，

    $$
    P(X\in B) = \int_{B} p(x)\text{d}x
    $$

    简记 $X \sim p(x)$
    
称 $p(x)$ 为 $X$ 的密度函数

!!! note
    * $P(X=x) = 0, \quad x\in \mathbb{R}$
    * $P(X\in(a,b]) = \int_{a}^{b} p(x)\text{d}x$

#### 连续型随机变量的例子

!!! example "均匀分布"
    向 $(a,b)$ 上随机投点，记落点的位置为 $X$

    * $X$ 落在 $(a,b)$ 上每一点等可能 $P(X=x) = 0$
    * $P(X\in A) = \frac{|A|}{b-a}$

    简记 $X \sim U(a,b)$

    $$
    X \sim p(x) = 
    \begin{cases}
    \frac{1}{b-a}, & x\in (a,b) \\
    0            , & \text{其他}
    \end{cases}
    $$

!!! example "指数分布"
    如果 $X$ 取非负实数，且

    $$
    p(x) =
    \begin{cases}
    \lambda e^{-\lambda x}, & x\geq 0 \\
    0                     , & \text{其他}
    \end{cases}
    \quad \lambda > 0
    $$

    简记 $X \sim \exp(\lambda)$

    !!! note
        * 通常描述寿命
        * 和 Poisson 分布有密切联系：Poisson 过程
        * $X$ 取大值的可能性迅速衰减
            
            $$
            P(X>x) = e^{\lambda x}, \quad x \geq 0
            $$
        * 无记忆性。使用了 $y$ 小时之后还能使用 $x$ 的概率

            $$
            \begin{align}
            P(X>x+y | X>y) =& \frac{P(X > x+y)}{P(X > y)} \\
                           =& \frac{e^{-\lambda(x+y)}}{e^{-\lambda y}} \\
                           =& e^{-\lambda x} \\
                           =& P(X>x)
            \end{align}
            $$

!!! example "正态分布"
    随机变量 $X$ 取所有实数值，密度

    $$
    p(x) = \frac{1}{\sqrt{2\pi}\sigma}e^{-\frac{(x-\mu)^2}{2\sigma^2}},
    \quad -\infty < x < \infty
    $$

    称 $X$ 是服从正态分布的随机变量，简记 $X \sim N(\mu,\sigma^2)$

    !!! note
        * $-\infty < \mu < \infty, \sigma > 0$
        * 标准正态分布 $\mu = 0, \sigma^2 = 1$，称 $X \sim N(0,1)$

    !!! note "验证"
        验证

        $$
        \int_{-\infty}^{\infty}p(x)\text{d}x = 1
        $$

        令 $t = \frac{x-\mu}{\sigma}$ ，只需证明

        $$
        \int_{-\infty}^{\infty}\frac{1}{\sqrt{2\pi}}e^{-\frac{x^2}{2}}\text{d}x = 1
        $$

        考虑

        $$
        \left(\int_{-\infty}^{\infty}\frac{1}{\sqrt{2\pi}}e^{-\frac{x^2}{2}}\text{d}x\right)^2 = \int_{-\infty}^{\infty}\int_{-\infty}^{\infty}\frac{1}{2\pi}e^{-\frac{x^2+y^2}{2}}\text{d}x\text{d}y
        $$

        极坐标变换：

        $$
        \int_{-\infty}^{\infty}\int_{-\infty}^{\infty}\frac{1}{2\pi}e^{-\frac{x^2+y^2}{2}}\text{d}x\text{d}y = \int_{0}^\infty\int_{0}{2\pi}\frac{1}{2\pi}e^{-\frac{\rho^2}{2}}\rho\text{d}\rho\text{d}\theta = 1
        $$

    !!! note "正态分布的性质"
        * 对称性：关于 $x = \mu$ 对称
        * 光滑性：$p(x)$ 任意次可微
        * 单调性
        * 渐近线 $y = 0$
        * 最大值：$P(x)$ 在 $x=\mu$ 初取最大值 $\frac{1}{\sqrt{2\pi}\sigma}$
        * $\sigma$ 变大，曲线变平坦；$\sigma$ 变小，曲线变陡峭
        * 类似积分

            $$
            \begin{align}
            P(X > \mu + \sigma x) =& \int_{\mu + \sigma x}^{\infty} \frac{1}{\sqrt{2\pi}\sigma}e^{-\frac{(u - \mu)^2}{2\sigma^2}}\text{d}u \\
            \sim & \frac{1}{\sqrt{2\pi}x}e^{-\frac{x^2}{2}}, \quad x\to \infty
            \end{align}
            $$

            同理

            $$
            P(X < \mu - \sigma x) \sim \frac{1}{\sqrt{2\pi}x}e^{-\frac{x^2}{2}}, \quad x\to \infty
            $$

### 一般型随机变量与分布函数

假定 $(\Omega, \mathcal{A}, P)$ 是概率空间，$(\mathbb{R}, \mathcal{B})$ 为 Borel $\sigma$-域

## 随机变量的函数

## 随机向量与联合分布函数

### 离散型随机变量

### 连续型随机变量

### 独立随机变量

## 随机向量的运算