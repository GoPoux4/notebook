# llama.cpp 6.4

## llama.cpp 计算图构建

有两次计算图构建：

1. context 初始化阶段，在实际推理之前，会先进行计算图的构建，预留最坏情况下的计算图内存。
2. 推理阶段，根据实际输入数据，动态构建计算图。

计算图构建在 `llm_build_llama::llm_build_llama()` 函数中进行，基本逻辑是将一部分 tensor 进行连接后，用 `ggml_build_forward_expand()` 函数将这些 tensor 扩展计算图中。

## Tensor 连接中的检查

Tensor 之间使用 GGML 框架提供的 `ggml_add` 等函数进行连接，相关实现在 `ggml/src/ggml.c` 中，会在连接之前进行一些基本的检查。下面列出了一些包含检查操作的函数。

<!-- ### ggml_dup

无检查。

`ggml_sqr` / `ggml_sqrt` / `ggml_log` / `ggml_sin` / `ggml_cos` / `ggml_sum` / `ggml_sum_rows` / `ggml_mean` / `ggml_silu_back` / `ggml_norm` / `ggml_rms_norm` / `ggml_rms_norm_back` / `ggml_group_norm` / `ggml_l2_norm_impl` 均无检查。 -->

### ggml_add

- 检查 a tensor 的形状能否由 b tensor 进行广播得到（`ggml_can_repeat`）。

`ggml_sub` / `ggml_mul` / `ggml_div` / `ggml_repeat` / `ggml_repeat_back` 的检查相同。

### ggml_add_cast

- 检查 a tensor 的形状能否由 b tensor 在不广播第一维的情况下进行广播得到（`ggml_can_repeat_rows`）。
- 检查 a tensor 的类型是否为量化类型或 F16、BF16。

### ggml_add1

- 检查 b tensor 是否为标量。
- `GGML_ASSERT(ggml_is_padded_1d(a))`

### ggml_acc

- 检查 b tensor 的元素数量是否小于等于 a tensor 的元素数量。
- a tensor 连续
- 两个 tensor 类型均为 F32。

### ggml_argmax

- 检查 a tensor 是否为矩阵
- 检查 a tensor 第一维长度是否在 int32 范围内

### ggml_count_equal

- 检查 a tensor 和 b tensor 的形状是否相同

### ggml_concat

- 拼接维度 dim 在 `[0, GGML_MAX_DIMS)` 范围内
- a tensor 和 b tensor 类型相同
- a tensor 和 b tensor 除 dim 维度外的其他维度形状相同

### ggml_unary

一元操作

- 检查 a tensor 是否连续（`ggml_is_contiguous_1`）

包含操作 `ggml_abs` / `ggml_sgn` / `ggml_neg` / `ggml_step` / `ggml_tanh` / `ggml_elu` / `ggml_relu` / `ggml_leaky_relu` / `ggml_sigmoid` / `ggml_gelu` / `ggml_gelu_quick` / `ggml_silu` / `ggml_hardswish` / `ggml_hardsigmoid` / `ggml_exp`。

### ggml_mul_mat

- 检查 b tensor 能否广播到 a tensor 的形状（`ggml_can_mul_mat`）。
- a tensor 没有被转置（`GGML_ASSERT(!ggml_is_transposed(a))`）。

### ggml_mul_mat_id

