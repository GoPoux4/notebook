# 图像的处理

## Image processing basics

- Contrast：S curve，\(\mathrm{output}(x, y) = f(\mathrm{input}(x, y))\)
- Invert：\(\mathrm{output}(x, y) = 1 - \mathrm{input}(x, y)\)
- Blur 模糊
- Sharpen 锐化
- Edge detection 边缘检测

### Gaussian Blur 高斯模糊

Obtain filter kernel by 2D Gaussian function:

\[
    f(i, j) = \frac{1}{2\pi\sigma^2}e^{-\frac{i^2+j^2}{2\sigma^2}}
\]

3x3 Gaussian kernel:

\[
    \begin{bmatrix}
        .075 & .124 & .075 \\
        .124 & .204 & .124 \\
        .075 & .124 & .075
    \end{bmatrix}
\]

### Sharpen Filter 锐化滤波

\[
    \begin{bmatrix}
        0 & -1 & 0 \\
        -1 & 5 & -1 \\
        0 & -1 & 0
    \end{bmatrix}
\]

Sharpening is adding high frequencies.

### Bilateral Filter 双边滤波

Kernel depends on both spatial distance and intensity difference.

## Image Sampling 图像采样

Reducing image size – down-sampling

### Aliasing 混叠

产生原因：Signals are changing too fast but sampled too slow. 信号变化太快，采样太慢。

Higher frequency signals need faster sampling.

Under-sampling：高频信号采样不足，采样结果错误地显示低频信号。

### Fourier Transform 傅里叶变换

Represent a function as a weighted sum of sines and cosines. 将函数表示为正弦和余弦的加权和。

\[
    F(u) = \int_{-\infty}^{\infty} f(x)e^{-2\pi i ux}\mathrm{d}x
\]

