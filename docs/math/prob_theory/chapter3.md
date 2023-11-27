# 数字特征

## 数学期望

### 离散型随机变量

$$
X \sim
\begin{pmatrix}
    x_1 & x_2 & \cdots & x_n \\
    p_1 & p_2 & \cdots & p_n
\end{pmatrix}
$$

期望

$$
E(X) = \sum_{i=1}^n x_i p_i
$$

!!! example "二项 Bernoulli 分布"
    $$
    X \sim B(n, p), P(X = k) = C_n^k p^k (1-p)^{n-k}
    $$
    
    $$
    E(X) = \sum_{k=0}^n k C_n^k p^k (1-p)^{n-k} = np
    $$

!!! example "Poisson 分布"
    $$
    X \sim P(\lambda), P(X = k) = \frac{\lambda^k}{k!} e^{-\lambda}
    $$
    
    $$
    E(X) = \sum_{k=0}^\infty k \frac{\lambda^k}{k!} e^{-\lambda} = \lambda
    $$

!!! example "几何分布"
    $$
    X \sim G(p), P(X = k) = (1-p)^{k-1} p
    $$
    
    $$
    E(X) = \sum_{k=1}^\infty k (1-p)^{k-1} p = \frac{1}{p}
    $$

### 运算性质

## 方差

## 协方差

## 矩