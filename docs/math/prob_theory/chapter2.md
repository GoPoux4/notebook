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

- 分布函数
    
    $$
    F(x) = P(\omega: X(\omega) \leq x), \quad -\infty < x < \infty
    $$

- 分布函数的性质

    * $\lim_{x\to -\infty}F(x) = 0, \lim_{x\to \infty}F(x) = 1$
    * $F(x_1) \leq F(x_2), \quad x_1 \leq x_2$
    * $F(x)$ 左极限和右极限存在
       
        $$
        \lim_{x\to x_0-0}F(x) = F(x_0-0), \quad \lim_{x\to x_0+0}F(x) = F(x_0+0)
        $$

        !!! note
            * $F(x_0-0) = P(X < x_0)$
            * $P(x = x_0) = F(x_0+0) - F(x_0-0)$
            * $P(X \in (x_1, x_2]) = F(x_2) - F(x_1)$

- 连续性随机变量的分布函数

    $$
    \begin{align*}
    F(x) =& P(X \leq x) \\
         =& \int_{-\infty}^{x}p(u)\text{d}u
    \end{align*}
    $$

    $F(x)$ 是连续函数，且 $F'(x) = p(x)$

    !!! note "正态分布的分布函数"
        假定 $X \sim N(0, 1)$

        $$
        F(x) = \frac{1}{\sqrt{2\pi}}\int_{-\infty}^{x}e^{-\frac{u^2}{2}}\text{d}u
        $$

        没有显性表达式，用 $\Phi(x)$ 表示分布函数（Probit 函数）

        误差函数

        $$
        erf(x) = \frac{2}{\sqrt{\pi}}\int_{0}^{x}e^{-u^2}\text{d}u
        $$

        采用误差函数表示 $\Phi(x)$

        $$
        \Phi(x) = \frac{1}{2}\left[1 + \frac{1}{\sqrt{2}}erf(x)\right]
        $$

        $\Phi(x)$ 的值：查表。

## 随机变量的函数

考虑

$$
Y = f(X), Y(\omega) = f(X(\omega))
$$

!!! note "$Y$ 是随机变量的条件"
    $X$ 是随机变量，$f$ 是可测函数

- 当 $X$ 是离散型随机变量时，$Y$ 也是离散型随机变量

    $$
    X \sim
    \begin{pmatrix}
    x_1 & x_2 & \cdots & x_i & \cdots & x_N \\
    p_1 & p_2 & \cdots & p_i & \cdots & p_N
    \end{pmatrix}
    $$

    $$
    P(Y = y_i) = \sum_{x_j: f(x_j) = y_i}p_j
    $$

- 当 $X$ 是连续型随机变量时，$Y$ 不一定是连续型随机变量。无统一公式。

    一般地，假设 $X \sim p_X(x)$，$f$ 具有反函数 $f^{-1}$，且 $f^{-1}$ 可导。则 $Y$ 是连续型随机变量，其密度函数为

    $$
    Y \sim p_Y(y) = p_X(f^{-1}(y))\left|\frac{\text{d}f^{-1}(y)}{\text{d}y}\right|
    $$

    !!! example
        假设 $X$ 具有连续的分布函数 $F(x)$，求 $Y = F(X)$ 的分布

        $$
        \begin{align}
        P(Y \leq y) =& P(F(X) \leq y) \\
                    =& P(X \leq F^{-1}(y)) \\
                    =& F(F^{-1}(y)) \\
                    =& y, \quad 0 \leq y \leq 1
        \end{align}
        $$

        则 $Y \sim U(0,1)$

## 随机向量与联合分布函数

### 离散型随机变量

$(X,Y)$ 是 2-随机向量。$X$ 取值 $x_1, x_2, \cdots$，$Y$ 取值 $y_1, y_2, \cdots$。称 $(X,Y)$ 为离散型随机向量。

$$
p_{ij} = P(X = x_i, Y = y_j), \quad i,j = 1,2,\cdots
$$

称

$$
((x_i, y_j), p_{ij})_{i,j=1}^{\infty}
$$

为 $(X,Y)$ 的联合分布。

- 边际分布

    $$
    \begin{align}
    P(X = x_i) =& \sum_{j=1}^{\infty}p_{ij} \\
    P(Y = y_j) =& \sum_{i=1}^{\infty}p_{ij}
    \end{align}
    $$

    !!! warning
        边际分布由联合分布唯一确定，反之不成立。

- 条件分布

    $$
    P(X = x_i | Y = y_j) = \frac{P(X = x_i, Y = y_j)}{P(Y = y_j)} = \frac{p_{ij}}{p_{\cdot j}}
    $$

    条件分布列

    $$
    X|Y = y_j \sim
    \begin{pmatrix}
    x_1 & x_2 & \cdots & x_i & \cdots \\
    \frac{p_{1j}}{p_{\cdot j}} & \frac{p_{2j}}{p_{\cdot j}} & \cdots & \frac{p_{ij}}{p_{\cdot j}} & \cdots
    \end{pmatrix}
    $$

- 独立离散型随机变量

    $$
    P(X = x_i, Y = y_j) = P(X = x_i)P(Y = y_j), \forall i,j
    $$

    即

    $$
    p_{ij} = p_{i\cdot}p_{\cdot j}, \quad \forall i,j
    $$

### 连续型随机变量

联合密度函数 $p(x,y)$

$$
p(x,y) \geq 0, \quad \int_{-\infty}^{\infty}\int_{-\infty}^{\infty}p(x,y)\text{d}x\text{d}y = 1
$$

并且

$$
P(X \in A, Y \in B) = \int_{A}\int_{B}p(x,y)\text{d}x\text{d}y
$$

称 $(X,Y)$ 为连续型随机向量。

- 边际密度

    $$
    X \sim p_X(x), \quad Y \sim p_Y(y)
    $$

    $$
    p_X(x) = \int_{-\infty}^{\infty}p(x,y)\text{d}y, \quad p_Y(y) = \int_{-\infty}^{\infty}p(x,y)\text{d}x
    $$

!!! example "联合正态分布"
    $$
    p(x,y) = \frac{1}{2\pi\sigma_1\sigma_2\sqrt{1-\rho^2}}e^{-\frac{1}{2(1-\rho^2)}\left[\frac{(x-\mu_1)^2}{\sigma_1^2} - 2\rho\frac{(x-\mu_1)(y-\mu_2)}{\sigma_1\sigma_2} + \frac{(y-\mu_2)^2}{\sigma_2^2}\right]}
    $$

    称 $(X,Y)$ 服从二元正态分布，简记 $(X,Y) \sim N(\mu_1, \mu_2, \sigma_1^2, \sigma_2^2, \rho)$

    - 边际分布

        $$
        X \sim N(\mu_1, \sigma_1^2), \quad Y \sim N(\mu_2, \sigma_2^2)
        $$

- 条件分布

    $$
    p_{Y|X}(y|x) = \frac{p(x,y)}{p_X(x)}
    $$

    $$
    P(Y \leq y | X = x) = \frac{\int_{-\infty}^{y}p(x,v)\text{d}v}{p_X(x)}
    $$

- 独立连续型随机变量

    $$
    p(x,y) = p_X(x)p_Y(y), \quad \forall x,y
    $$

### 一般型随机变量

一般利用分布函数

$$
F(x,y) = P(X \leq x, Y \leq y)
$$

!!! note "$F(x)$ 性质"
    - $F(-\infty, y) = F(x, -\infty) = 0, F(+\infty, +\infty) = 1$
    - $F(x, y)$ 关于 $x$ 和 $y$ 都是非减函数
    - $F(x, y)$ 关于 $x$ 和 $y$ 左极限存在，右连续
    - $P(a < X \leq b, c < Y \leq d) = F(b,d) - F(a,d) - F(b,c) + F(a,c)$

- 边际分布

    $$
    F_X(x) = F(x, +\infty), \quad F_Y(y) = F(+\infty, y)
    $$

- 条件分布

    $$
    \begin{align}
    P(Y \leq y | X = x) =& \lim_{\epsilon \to 0} P(Y \leq y | x - \epsilon < X \leq x + \epsilon) \\
                        =& \lim_{\epsilon \to 0} \frac{F(x+\epsilon, y) - F(x-\epsilon, y)}{F_X(x+\epsilon) - F_X(x-\epsilon)}
    \end{align}
    $$

- 独立一般型随机变量

    $$
    F(x,y) = F_X(x)F_Y(y), \quad \forall x,y
    $$

    如果 $X, Y$ 相互独立，那么对于任意 Borel 函数 $f,g$，$f(X)$ 和 $g(Y)$ 也相互独立。

    !!! proof
        $$
        \begin{align}
        P(f(X) \in A, g(Y) \in B) =& P(X \in f^{-1}(A), Y \in g^{-1}(B)) \\
                                   =& P(X \in f^{-1}(A))P(Y \in g^{-1}(B)) \\
                                   =& P(f(X) \in A)P(g(Y) \in B)
        \end{align}
        $$

### 多维随机向量

$\mathbf{X} = (X_1, X_2, \cdots, X_n)$ 是 $n$-维随机向量。

- $n$-元联合分布函数

    $$
    F_{\mathbf{X}}(\mathbf{x}) = P(X_1 \leq x_1, X_2 \leq x_2, \cdots, X_n \leq x_n)
    $$

- 边际分布

    $$
    F_{X_i}(x_i) = F_{\mathbf{X}}(\infty, \cdots, \infty, x_i, \infty, \cdots, \infty) = P(X_i \leq x_i)
    $$

- 独立随机变量

    $$
    F_{\mathbf{X}}(\mathbf{x}) = \prod_{i=1}^{n}F_{X_i}(x_i)
    $$

    则 $X_1, X_2, \cdots, X_n$ 相互独立。

## 随机向量的运算

### 随机向量的加减

- 离散型随机变量

    分布为

    $$
    P(X = x_i, Y = y_j) = p_{ij}
    $$

    记 $Z = X + Y$，则

    $$
    P(Z = z_k) = \sum_{i,j: x_i + y_j = z_k}p_{ij}
    $$

- 连续型随机变量

    $$
    \begin{align*}
    F_Z(z) = P(X + Y \leq z) =& \int_{-\infty}^{\infty}\int_{-\infty}^{z-x}p(x,y)\text{d}y\text{d}x \\
                             =& \int_{-\infty}^{z}\int_{-\infty}^{\infty}p(x, y - x)\text{d}y\text{d}x
    \end{align*}
    $$

    密度函数

    $$
    p_Z(z) = \int_{-\infty}^{\infty}p(x, z - x)\text{d}x
    $$

    减法类似：$Z = Y - X$

    $$
    p_Z(z) = \int_{-\infty}^{\infty}p(x, x + z)\text{d}x
    $$

!!! note "$\Gamma(\alpha, \beta)$ 分布"
    令 $\alpha, \beta > 0$，定义

    $$
    \Gamma(\alpha) = \int_{0}^{\infty}x^{\alpha - 1}e^{-x}\text{d}x
    $$

    $\Gamma(n) = (n - 1)!, \Gamma(\alpha + 1) = \alpha\Gamma(\alpha)$

    分布密度函数

    $$
    p(x; \alpha, \beta) = \frac{\beta^\alpha}{\Gamma(\alpha)}x^{\alpha - 1}e^{-\beta x}, \quad x > 0
    $$

### 随机向量的乘除

- 乘法

    假设 $(X,Y)$ 是连续型随机向量，$Z = XY$，则

    $$
    \begin{align*}
    F_Z(z) =& P(XY \leq z) \\
        =& \int_{xy \leq z}p(x, y)\text{d}x\text{d}y \\
        =& \int_{-\infty}^{0}\text{d}x\int_{z/x}^{\infty}p(x, y)\text{d}y + \int_{0}^{\infty}\text{d}x\int_{-\infty}^{z/x}p(x, y)\text{d}y \\
        =& -\int_{-\infty}^{0}\text{d}x\int_{-\infty}^{z}\frac{1}{x}p(x, \frac{y}{x})\text{d}y + \int_{0}^{\infty}\text{d}x\int_{-\infty}^{z}\frac{1}{x}p(x, \frac{y}{x})\text{d}y \\
        =& -\int_{-\infty}^{z}\int_{-\infty}^{0}\frac{1}{x}p(x, \frac{y}{x})\text{d}x\text{d}y + \int_{-\infty}^{z}\int_{0}^{\infty}\frac{1}{x}p(x, \frac{y}{x})\text{d}x\text{d}y
    \end{align*}
    $$

    密度函数

    $$
    \begin{align*}
    p_Z(z) =& -\int_{-\infty}^{0}\frac{1}{x}p(x, \frac{z}{x})\text{d}x + \int_{0}^{\infty}\frac{1}{x}p(x, \frac{z}{x})\text{d}x \\
        =& \int_{-\infty}^{\infty}\frac{1}{|x|}p(x, \frac{z}{x})\text{d}x
    \end{align*}
    $$

- 除法

    $Z = \frac{Y}{X}$ 的密度函数

    $$
    p_Z(z) = \int_{-\infty}^{\infty}|x|p(x, xz)\text{d}x
    $$

!!! note "Cauchy 分布"
    $$
    p(x; x_0, \gamma) = \frac{1}{\pi}\frac{\gamma}{(x - x_0)^2 + \gamma^2}
    $$

### 随机向量的变换

$(X, Y)$ 为连续型随机向量，有联合密度函数 $p_{XY}(x, y)$，变换如下

$$
\begin{cases}
U = f_1(X, Y) \\
V = f_2(X, Y)
\end{cases}
$$

- 基本方法

    $$
    \begin{align*}
    P(U \leq u, V \leq v) =& P(f_1(X, Y) \leq u, f_2(X, Y) \leq v) \\
        =& \iint_{f_1(x, y) \leq u, f_2(x, y) \leq v}p_{XY}(x, y)\text{d}x\text{d}y
    \end{align*}
    $$

- 特殊情形

    假设 $f_1, f_2$ 存在逆变换：

    $$
    \begin{cases}
    X = g_1(u, v) \\
    Y = g_2(u, v)
    \end{cases}
    $$

    假设 $g_1, g_2$ 可微，且雅可比式

    $$
    J = \frac{\partial(x, y)}{\partial(u, v)}
    $$

    那么 $(U, V)$ 的为连续型随机向量，其密度函数为

    $$
    p_{UV}(u, v) = p_{XY}(x, y)|J|
    $$

### 极值随机变量

$X_1, X_2, \cdots, X_n$ 是独立随机变量，有相同的分布函数 $F(x)$，记

$$
X_{(1)} \leq X_{(2)} \leq \cdots \leq X_{(n)}
$$

即 $X_{(k)}$ 为第 k 小值。$X_{(1)} = \min\{X_1, X_2, \cdots, X_n\}$，$X_{(n)} = \max\{X_1, X_2, \cdots, X_n\}$。

- $X_{(n)}$ 的分布

    $$
    \begin{align*}
    F_{X_{(n)}}(x) =& P(X_{(n)} \leq x) \\
        =& P(X_1 \leq x, X_2 \leq x, \cdots, X_n \leq x) \\
        =& P(X_1 \leq x)P(X_2 \leq x)\cdots P(X_n \leq x) \\
        =& F^n(x)
    \end{align*}
    $$

    密度函数

    $$
    p_{X_{(n)}}(x) = \frac{\text{d}F_{X_{(n)}}}{\text{d}x} = nF^{n-1}(x)F'(x)
    $$

- $X_{(1)}$ 的分布

    $$
    \begin{align*}
    F_{X_{(1)}}(x) =& P(X_{(1)} \leq x) \\
        =& 1 - P(X_{(1)} > x) \\
        =& 1 - P(X_1 > x, X_2 > x, \cdots, X_n > x) \\
        =& 1 - P(X_1 > x)P(X_2 > x)\cdots P(X_n > x) \\
        =& 1 - (1 - F(x))^n
    \end{align*}
    $$

    密度函数

    $$
    p_{X_{(1)}}(x) = \frac{\text{d}F_{X_{(1)}}}{\text{d}x} = n(1 - F(x))^{n-1}F'(x)
    $$

- $X_{(k)}$ 的分布

    $$
    \begin{align*}
    F_{X_{(k)}}(x) =& P(X_{(k)} \leq x) \\
        =& {n \choose k} F^k(x)(1 - F(x))^{n-k}
    \end{align*}
    $$

## 总结

- 二项 Bernoulli 分布

    $$
    P(X = k) = {n \choose k}p^k(1-p)^{n-k}, \quad k = 0, 1, \cdots, n
    $$

    Poisson 分布：

    $$
    P(X = k) = \frac{\lambda^k}{k!}e^{-\lambda}, \quad k = 0, 1, \cdots
    $$

    !!! note "Poisson 极限定理"
        $S_n \sim B(n, p), X \sim \mathcal{P}(\lambda)$

        $$
        P(S_n = k) = {n \choose k}p^k(1-p)^{n-k} \to \frac{\lambda^k}{k!}e^{-\lambda} = P(X = k), \quad n \to \infty
        $$

    几何分布：不断重复直到事件发生

    $$
    P(X = k) = p(1-p)^{k-1}, \quad k = 1, 2, \cdots
    $$

    超几何分布：$N$ 件产品 $M$ 件次品，随机抽样 $n$ 件，$X$ 表示 $n$ 件产品中次品数

    $$
    P(X = k) = \frac{{M \choose k}{N - M \choose n - k}}{{N \choose n}}, \quad k = 0, 1, \cdots, \min\{M, n\}
    $$

    指数分布

    $$
    p(x) =
    \begin{cases}
    \lambda e^{-\lambda x}, & x\geq 0 \\
    0                     , & \text{其他}
    \end{cases}
    \quad \lambda > 0
    $$

- 连续型随机变量

    密度函数 $p(x)$，分布函数 $F(x)$

    $$
    p(x) \geq 0, \int_{-\infty}^{\infty}p(x)\text{d}x = 1
    $$

    $$
    F(x) = P(X \leq x) = \int_{-\infty}^{x}p(u)\text{d}u
    $$