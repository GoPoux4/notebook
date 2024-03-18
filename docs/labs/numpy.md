# lab2 向量化计算

## 思路

### Step 1

先考虑计算单张图片的情形（ `N = 1` ）。

下设新的 $H_2 \times W_2$ 的图中 $(i,j)$ 处像素所想要采样的 `a` 图中对应点坐标为 $(x_{(i,j)}, y_{(i,j)})$ ，即 `b[n, i, j]` 处所存坐标。

阅读 `baseline.py` ，可知基准代码采用了如下等式计算每个采样点 `(x, y)` 的采样结果：

$$
\begin{aligned}
    f(x,y) \approx
    {\frac {1}{(x_{2}-x_{1})(y_{2}-y_{1})}} 
    {\big (}
    &f(Q_{11})(x_{2}-x)(y_{2}-y) + f(Q_{21})(x-x_{1})(y_{2}-y)\\
    +&f(Q_{12})(x_{2}-x)(y-y_{1}) + f(Q_{22})(x-x_{1})(y-y_{1})
    {\big )}
\end{aligned}
$$

在本次实验给出的情景中，
$x_2 - x_1 = y_2 - y_1 = 1, x_1 = \lfloor x \rfloor, y_1 = \lfloor y \rfloor$ 
，记 
$x' = x - \lfloor x \rfloor， y' = y - \lfloor y \rfloor$ 
，上述等式可简化为：

$$
\displaystyle
\begin{aligned}
    f(x,y) \approx 
    \ & f(Q_{11})(1 - x')(1 - y')+f(Q_{21})(x')(1 - y') \\
    + \ & f(Q_{12})(1 - x')(y')+f(Q_{22})(x')(y') \\
\end{aligned}
$$

由于每个采样点的计算都是独立的，我们可以定义如下几个矩阵，

$$
\mathbf{F} = \big( f(x_{(i,j)}, y_{(i,j)})\big)_{H2 \times W2} 
$$

$$
\begin{aligned}
    \mathbf{F_{00}} = \big( f(Q_{11})\big)_{H2 \times W2} \quad
    \mathbf{F_{01}} = \big( f(Q_{12})\big)_{H2 \times W2} \\
    \mathbf{F_{10}} = \big( f(Q_{21})\big)_{H2 \times W2} \quad
    \mathbf{F_{11}} = \big( f(Q_{22})\big)_{H2 \times W2} \\
\end{aligned}
$$

$$
\begin{aligned}
    \mathbf{X'} = \big( x'_{(i, j)} \big)_{H2 \times W2} \\
    \mathbf{Y'} = \big( y'_{(i, j)} \big)_{H2 \times W2}
\end{aligned}
$$

使用逐元素乘法（ `*` ）和矩阵加法即可实现向量化计算矩阵 $\mathbf{F}$ 中的采样信息：

$$
\begin{aligned}
    \mathbf{F}
    & = \mathbf{F_{00}} * (1 - \mathbf{X'}) * (1 - \mathbf{Y'}) + \mathbf{F_{10}} * \mathbf{X'} * (1 - \mathbf{Y'}) \\
    & + \mathbf{F_{01}} * (1 - \mathbf{X'}) * \mathbf{Y'} + \mathbf{F_{11}} * \mathbf{X'} * \mathbf{Y'} 
\end{aligned}
$$

据此即可使用向量化去掉内层两层 `for` 循环，代码实现如下：

```py linenums="1" title="vectorize1.py"
def bilinear_interp_vectorized(a: np.ndarray, b: np.ndarray) -> np.ndarray:
    """
    This is the vectorized implementation of bilinear interpolation.
    - a is a ND array with shape [N, H1, W1, C], dtype = int64
    - b is a ND array with shape [N, H2, W2, 2], dtype = float64
    - return a ND array with shape [N, H2, W2, C], dtype = int64
    """
    # get axis size from ndarray shape
    N, H1, W1, C = a.shape
    N1, H2, W2, _ = b.shape
    assets N == N1

    # Do iteration
    b = b.transpose(0, 3, 1, 2)
    res = np.empty((N, H2, W2, C), dtype=int64)

    for n in range(N):
        X, Y = b[n]
        X_idx , Y_idx = np.floor(X).astype(int64), np.floor(Y).astype(int64)
        _X, _Y = X - X_idx, Y - Y_idx
        A00 = a[n, X_idx, Y_idx].transpose(2, 0, 1)
        A01 = a[n, X_idx, Y_idx + 1].transpose(2, 0, 1)
        A10 = a[n, X_idx + 1, Y_idx].transpose(2, 0, 1)
        A11 = a[n, X_idx + 1, Y_idx + 1].transpose(2, 0, 1)
        res00 = A00 * (1 - _X) * (1 - _Y)
        res10 = A10 * _X * (1 - _Y)
        res01 = A01 * (1 - _X) * _Y
        res11 = A11 * _X * _Y
        res[n] = res00.transpose(1, 2, 0) + res01.transpose(1, 2, 0) + \
                 res10.transpose(1, 2, 0) + res11.transpose(1, 2, 0)
    return res
```

!!! note
    代码中使用了 `np.transpose` 函数进行矩阵转置，使得 `X, Y` 能容易地从 `b` 中取出，并且使得在进行矩阵逐元素乘法时能满足广播规则，同时对 `C` 个通道的信息进行处理。

### Step 2

现在考虑同时对 `N` 张图片进行处理。

对 `vectorize1.py` 进行进一步的修改。

我们要将 `N` 张图片的信息进行整合处理，一个可行的方法为在 `X, Y` 的基础上增加一个代表图片索引的轴，使其形状从 `(H2, W2)` 变为 `(N, H2, W2)` 。具体实现如下（ `X_idx, Y_idx, _X, _Y` 的获取一并给出）：

```py linenums="1"
X, Y = b.transpose(3, 0, 1, 2)
X_idx , Y_idx = np.floor(X).astype(int64), np.floor(Y).astype(int64)
_X, _Y = X - X_idx, Y - Y_idx
```

同理，`A00, A01, A10, A11` 均需要增加一个轴，于是新增一个索引数组 `N_idx` 对 `a` 数组的第一个轴 （ `axis = 0` ） 进行索引。具体实现如下：

```py linenums="1"
N_idx = np.arange(N).reshape(N, 1, 1)

A00 = a[N_idx, X_idx, Y_idx].transpose(3, 0, 1, 2)
A01 = a[N_idx, X_idx, Y_idx + 1].transpose(3, 0, 1, 2)
A10 = a[N_idx, X_idx + 1, Y_idx].transpose(3, 0, 1, 2)
A11 = a[N_idx, X_idx + 1, Y_idx + 1].transpose(3, 0, 1, 2)
```

!!! note
    `N_idx` 的形状与 `X_idx, Y_idx` 并不匹配，之所以能够像这样对 `a` 数组进行 fancy indexing ，是因为进行索引的时候对 `N_idx` 的后两个轴（ `axis=1, axis=2` ）进行了 broadcast 。

完整代码如下：

```py linenums="1" title="vectorize.py"
def bilinear_interp_vectorized(a: np.ndarray, b: np.ndarray) -> np.ndarray:
    """
    This is the vectorized implementation of bilinear interpolation.
    - a is a ND array with shape [N, H1, W1, C], dtype = int64
    - b is a ND array with shape [N, H2, W2, 2], dtype = float64
    - return a ND array with shape [N, H2, W2, C], dtype = int64
    """
    # get axis size from ndarray shape
    N, _, _, C = a.shape
    N1, H2, W2, _ = b.shape
    assets N == N1

    # Do iteration
    res = np.empty((N, H2, W2, C), dtype=int64)

    # Get the matrices of coordinates
    X, Y = b.transpose(3, 0, 1, 2)
    X_idx , Y_idx = np.floor(X).astype(int64), np.floor(Y).astype(int64)
    _X, _Y = X - X_idx, Y - Y_idx

    # Generate the indices array
    N_idx = np.arange(N).reshape(N, 1, 1)

    A00 = a[N_idx, X_idx, Y_idx].transpose(3, 0, 1, 2)
    A01 = a[N_idx, X_idx, Y_idx + 1].transpose(3, 0, 1, 2)
    A10 = a[N_idx, X_idx + 1, Y_idx].transpose(3, 0, 1, 2)
    A11 = a[N_idx, X_idx + 1, Y_idx + 1].transpose(3, 0, 1, 2)
    res = A00 * (1 - _X) * (1 - _Y) + A01 * (1 - _X) * _Y + \
          A10 * _X * (1 - _Y) + A11 * _X * _Y
    return res.transpose(1, 2, 3, 0).astype(int64)
```

## 正确性与加速比

运行 `main.py` ，查看输出：

```
Generating Data...
Executing Baseline Implementation...
Finished in 99.58129525184631s
Executing Vectorized Implementation...
Finished in 3.138871192932129s
[PASSED] Results are identical.
Speed Up 31.725193272051396x
```

可见运行结果正确，加速比为 31.725193272051396 。

## Reference

- 初探Numpy中的花式索引：[https://zhuanlan.zhihu.com/p/123858781](https://zhuanlan.zhihu.com/p/123858781)
- Numpy中transpose()函数的可视化理解：[https://zhuanlan.zhihu.com/p/61203757](https://zhuanlan.zhihu.com/p/61203757)
- NumPy 中文文档，广播（Broadcasting）：[https://numpy.org.cn/user/basics/broadcasting.html](https://numpy.org.cn/user/basics/broadcasting.html)