# lab2.5 手写 SIMD 向量化

## 思路

首先分析需要向量化的部分：

```cpp linenums="1"
for (int i = 0; i < MAXN; ++i)
{
    c[i] += a[i] * b[i];
}
```

可以分为三个部分：

- 需要计算的数据： `a[i], b[i], c[i]` 。
- 计算操作： `tmp = a[i] * b[i]` 和 `c[i] = c[i] + tmp` 。

于是根据向量化的基本流程，大致需要几个操作：

- Load `a, b, c` 到 `__m256` 类型变量 `A, B, C` 。
- 令 `C = C + A * B` 。（此处运算符只代表操作的含义）
- 将 `C` 的数据存回 `float` 数组 `c` 中 。

如何实现？

Load 操作：

`__m256 _mm256_loadu_ps (float const * mem_addr)` 

: 从 `mem_addr` 指向的内存地址中 load 256 位（8 个 `float` 元素）的数据到向量寄存器。

计算操作：

`__m256 _mm256_mul_ps (__m256 a, __m256 b)`

: 计算 `a` 和 `b` 中 `float` 元素逐位相乘的结果。

`__m256 _mm256_add_ps (__m256 a, __m256 b)`

: 计算 `a` 和 `b` 中 `float` 元素逐位相加的结果。

Store 操作：

`void _mm256_storeu_ps (float * mem_addr, __m256 a)`

: 将 `a` 中的 256 位（8 个 `float` 元素）的数据 store 到 `mem_addr` 指向的内存地址中。

于是就可以写出向量化的代码了：

```cpp linenums="1"
#define LEN (MAXN / 8)
for (int i = 0; i < LEN; ++i) {
    __m256 A, B, C;

    // Load data
    A = _mm256_loadu_ps(a + i * 8);
    B = _mm256_loadu_ps(b + i * 8);
    C = _mm256_loadu_ps(c + i * 8);

    // Calculate
    C = _mm256_add_ps(C, _mm256_mul_ps(A, B));

    // Store data
    _mm256_storeu_ps(c + i * 8, C);
}
```

!!! note
    由于 `__m256` 类型变量只能同时操作 256 位数据，即 8 个 `float` ，向量化时需对每 8 个数据进行一次向量化

## 正确性和加速比

编译运行 `add.cpp` ，程序输出：

```
time=1.916000
Check Passed
```

向量化后计算结果正确，加速比为 1.916 。

## 汇编代码分析

利用 [godbolt](https://godbolt.org/) 进行汇编代码分析。

向量化前，需要向量化的代码部分的汇编代码如下：

```asm linenums="1"
...
        mov     DWORD PTR [rbp-20], 0
        jmp     .L11
.L12:
        mov     eax, DWORD PTR [rbp-20]
        cdqe
        vmovss  xmm1, DWORD PTR c[0+rax*4]
        mov     eax, DWORD PTR [rbp-20]
        cdqe
        vmovss  xmm2, DWORD PTR a[0+rax*4]
        mov     eax, DWORD PTR [rbp-20]
        cdqe
        vmovss  xmm0, DWORD PTR b[0+rax*4]
        vmulss  xmm0, xmm2, xmm0
        vaddss  xmm0, xmm1, xmm0
        mov     eax, DWORD PTR [rbp-20]
        cdqe
        vmovss  DWORD PTR c[0+rax*4], xmm0
        add     DWORD PTR [rbp-20], 1
.L11:
        cmp     DWORD PTR [rbp-20], 99999999
        jle     .L12
...
```

可知，`for` 循环内部的代码被顺次执行了 `MAXN` 次，每次只对 1 组数据（ `a[i], b[i], c[i]` ） 进行操作。

向量化后，被向量化的代码部分的汇编代码如下：

```asm linenums="1"
...
        mov     DWORD PTR [rbp-36], 0
        jmp     .L11
.L17:
        mov     eax, DWORD PTR [rbp-36]
        sal     eax, 3
        cdqe
        sal     rax, 2
        add     rax, OFFSET FLAT:a
        mov     QWORD PTR [rbp-392], rax
        mov     rax, QWORD PTR [rbp-392]
        vmovups ymm0, YMMWORD PTR [rax]
        vmovaps YMMWORD PTR [rbp-112], ymm0
        mov     eax, DWORD PTR [rbp-36]
        sal     eax, 3
        cdqe
        sal     rax, 2
        add     rax, OFFSET FLAT:b
        mov     QWORD PTR [rbp-384], rax
        mov     rax, QWORD PTR [rbp-384]
        vmovups ymm0, YMMWORD PTR [rax]
        vmovaps YMMWORD PTR [rbp-144], ymm0
        mov     eax, DWORD PTR [rbp-36]
        sal     eax, 3
        cdqe
        sal     rax, 2
        add     rax, OFFSET FLAT:c
        mov     QWORD PTR [rbp-376], rax
        mov     rax, QWORD PTR [rbp-376]
        vmovups ymm0, YMMWORD PTR [rax]
        vmovaps YMMWORD PTR [rbp-176], ymm0
        vmovaps ymm0, YMMWORD PTR [rbp-112]
        vmovaps YMMWORD PTR [rbp-336], ymm0
        vmovaps ymm0, YMMWORD PTR [rbp-144]
        vmovaps YMMWORD PTR [rbp-368], ymm0
        vmovaps ymm0, YMMWORD PTR [rbp-336]
        vmulps  ymm0, ymm0, YMMWORD PTR [rbp-368]
        vmovaps ymm1, YMMWORD PTR [rbp-176]
        vmovaps YMMWORD PTR [rbp-272], ymm1
        vmovaps YMMWORD PTR [rbp-304], ymm0
        vmovaps ymm0, YMMWORD PTR [rbp-272]
        vaddps  ymm0, ymm0, YMMWORD PTR [rbp-304]
        vmovaps YMMWORD PTR [rbp-176], ymm0
        mov     eax, DWORD PTR [rbp-36]
        sal     eax, 3
        cdqe
        sal     rax, 2
        add     rax, OFFSET FLAT:c
        mov     QWORD PTR [rbp-184], rax
        vmovaps ymm0, YMMWORD PTR [rbp-176]
        vmovaps YMMWORD PTR [rbp-240], ymm0
        vmovaps ymm0, YMMWORD PTR [rbp-240]
        mov     rax, QWORD PTR [rbp-184]
        vmovups YMMWORD PTR [rax], ymm0
        nop
        add     DWORD PTR [rbp-36], 1
.L11:
        cmp     DWORD PTR [rbp-36], 12499999
        jle     .L17
...
```

可知程序使用了 `vmoups` 等汇编指令和 256 位寄存器 `ymm0, ymm1` ，同时对 8 组 `float` 类型数据进行操作，循环次数减少到 `MAXN / 8` 次。

## Reference

- Intel® Intrinsics Guide：[https://www.intel.com/content/www/us/en/docs/intrinsics-guide/index.html](https://www.intel.com/content/www/us/en/docs/intrinsics-guide/index.html)
- n方过百万 暴力碾标算——指令集优化的基础使用：[https://www.luogu.com.cn/blog/ouuan/avx-optimize](https://www.luogu.com.cn/blog/ouuan/avx-optimize)
- 汇编语言笔记（六）——SIMD指令：[https://zhuanlan.zhihu.com/p/424475308](https://zhuanlan.zhihu.com/p/424475308)