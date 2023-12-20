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

#### 常见离散型随机变量的期望

!!! example "二项 Bernoulli 分布"
    \[
    X \sim B(n, p), P(X = k) = C_n^k p^k (1-p)^{n-k}
    \]
    
    \[
    E(X) = \sum_{k=0}^n k C_n^k p^k (1-p)^{n-k} = np
    \]

!!! example "Poisson 分布"
    \[
    X \sim P(\lambda), P(X = k) = \frac{\lambda^k}{k!} e^{-\lambda}
    \]
    
    \[
    E(X) = \sum_{k=0}^\infty k \frac{\lambda^k}{k!} e^{-\lambda} = \lambda
    \]

!!! example "几何分布"
    \[
    X \sim G(p), P(X = k) = (1-p)^{k-1} p
    \]
    
    \[
    E(X) = \sum_{k=1}^\infty k (1-p)^{k-1} p = \frac{1}{p}
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

#### 常见连续型随机变量的期望

!!! example "均匀分布"
    \[
    X \sim U(a, b), p(x) = \frac{1}{b-a}
    \]
    
    \[
    EX = \int_a^b \frac{x}{b-a} \ \mathrm{d}x = \frac{a+b}{2}
    \]

!!! example "正态分布"
    \[
    X \sim N(\mu, \sigma^2), p(x) = \frac{1}{\sqrt{2\pi}\sigma} e^{-\frac{(x-\mu)^2}{2\sigma^2}}
    \]
    
    \[
    EX = \int_{-\infty}^{\infty} \frac{x}{\sqrt{2\pi}\sigma} e^{-\frac{(x-\mu)^2}{2\sigma^2}} \ \mathrm{d}x = \mu
    \]

## 方差

## 协方差

## 矩

## 特征函数