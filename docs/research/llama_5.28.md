# Llama CUDA Check Rules

llama.cpp 后端在调用 CUDA kernel 计算之前会对 tensor 进行检查。

## Check Rules

- 数据类型一致性：
    - 检测 `src0`、`src1` 和 `dst` 的数据类型是否能被当前操作支持。
    - 检查某两个张量的类型是否相同。
- 内存布局：
    - 检查张量的内存布局是否连续（`ggml_is_contiguous`）。
    - 检查最内层维度的元素是否连续，即字节步长是否等于数据类型的大小。（确保能被 GPU 高效访问）
- 张量形状、大小检查：
    - 有些操作对张量的形状有限制，可能限制为二维或三维张量，或者限制某些维度的奇偶性。
    - 检查张量之间的形状是否匹配，比如广播操作
    - 检查张量总元素个数是否匹配，比如生成等差数列必须张量元素个数和等差数列长度相同。
    - 检查计算过程中涉及的张量元素个数是否在某些范围内，比如 `ggml_cuda_count_equal` 中的元素个数限制在 int32 范围内。
- 参数有效性：
    - 从 `ggml_tensor` 的 `op_params` 中获取参数，检查参数是否在合理范围内。
- 后端检查：
    - 部分操作需要限制 tensor 的后端为 CUDA。

## 检查规则

### ggml_cuda_argmax

找每一行的最大值列索引。

- 检查 `src` 和 `dst` 类型
- 检查 `src` 连续性（`ggml_is_contiguous`）

### ggml_cuda_count_equal

计算 `src0` 和 `src1` 中对应位置的元素相等的个数。

- 检查 `src0` 和 `src1` 类型是否相同
- `dst` 必须是 I64 类型
- `src0` 和 `src1` 形状必须相同
- 检查 `src0`、`src1` 和 `dst` 连续性
- 限制 `src0` 和 `src1` 的元素总数在 int32 范围内（cuda `atomicAdd` 只能处理 int32）
- 为 `dst` 对应内存清零，并且检查是否成功
- 当 `src0` 和 `src1` 类型为 I32 时才进行计算

### ggml_cuda_op_repeat_back

对 repeat 操作的反向传播。

- 检查 `src0` 和 `dst` 的类型是否相同
- 检查 `dst` 连续性
- 检查 `src0` 是否能由 `dst` 通过 repeat 操作得到
- `GGML_ASSERT(ne2*ne3 <= (1 << 15));`：检查 `dst` 的第三和第四维度（`ne2` 和 `ne3`）的乘积是否在 int16 范围内（可能和 cuda kernel 网格大小和块大小有关？）
- `dst` 类型必须为 F32

### ggml_cuda_op_get_rows

从 `src0` 中获取 `src1` 指定的行向量，组成新的张量存到 `dst` 中。

- 检查 `src1` 的类型是否为 I32
- 检查 `dst` 的类型是否为 F32
- 分别检查 `src0`、`src1` 和 `dst` 维度 0 中的元素在内存中是否连续
- 对 `src0` 的类型分类
    - F32、F16：进一步检查 `src1` 的第四个维度为 1
    - 量化类型 Q4_0、Q8_0 等：检查 `src0` 最内层维度是偶数，应该是和量化的分块机制有关

### ggml_cuda_op_get_rows_back

对 `ggml_cuda_op_get_rows` 的反向传播。

- 检查 `src0`、`src1` 和 `dst` 的类型是否分别为 F32、I32 和 F32
- 检查三个张量的连续性
- 检查三个张量的第三四维度（`ne2` 和 `ne3`）的乘积是否为 1，即三个张量都为二维张量

### ggml_cuda_dup

将 `src` 的数据复制到 `dst`。直接调用 `ggml_cuda_cpy`。

### ggml_cuda_cpy

将 `src0` 的数据复制到 `src1`。

- 检查 `src0` 和 `src1` 元素个数是否相同
- 检查 `src0` 和 `src1` 占用字节数是否在 int32 范围内

计算部分：

- 如果 `src0` 和 `src1` 的类型相同、占用字节数相同并且都连续：直接调用 `cudaMemcpyAsync` 复制。
- 类型不同或者不连续：对支持的类型进行排列组合
    - 非量化类型（F32、F16）：无额外检查
    - 量化类型（Q4_0、Q8_0...）：`src1` 是量化类型时，会检查 `src0` 的元素个数是否能和量化类型对应的块大小对齐

### ggml_cuda_op_bin_bcast

执行二元操作的广播，模板函数。

- 限定 `src1` 类型为 F32 或 F16
- 对 `src0` 、`src1` 和 `dst` 的类型进行排列组合，再进入 `bin_bcast_cuda.operator()` 进行计算

`bin_bcast_cuda.operator()` 进行广播操作，相关逻辑还在看。

??? note "问了 Gemini"

    在您提供的 `bin_bcast_cuda` 代码中，`GGML_ASSERT` 语句主要用于执行以下数据范围和完整性检查：

    1.  **步长（Stride）与对齐检查：**
        *   `GGML_ASSERT(nb0 % sizeof(dst_t) == 0);`
        *   `GGML_ASSERT(nb1 % sizeof(dst_t) == 0);`
        *   `GGML_ASSERT(nb2 % sizeof(dst_t) == 0);`
        *   `GGML_ASSERT(nb3 % sizeof(dst_t) == 0);`
        *   以及针对 `src0` 和 `src1` 的类似检查（`nb00` 到 `nb03`，`nb10` 到 `nb13`）。

        这些断言检查确保了每个维度（从 `nb0` 到 `nb3` 代表目标张量 `dst` 的步长，`nb00` 到 `nb03` 代表 `src0` 的步长，`nb10` 到 `nb13` 代表 `src1` 的步长）的字节步长是对应数据类型大小（例如 `sizeof(float)` 或 `sizeof(__half)`）的精确倍数。在 GGML 中，张量数据以行主序存储，并且 `ggml_tensor` 结构包含每个维度的元素数量 (`ne`) 和字节步长 (`nb`) [1, 2, 3, 4, 5, 6]。这种设计允许存储非连续内存中的张量，这对于转置和置换等操作非常有用 [1, 2, 3, 4, 5]。如果步长不是数据类型大小的精确倍数，那么在内存中遍历张量时进行指针算术将导致内存访问错位或数据解释错误，可能引发程序崩溃或静默的数据损坏。因此，这些检查对于确保正确的内存访问和数据完整性至关重要。

    2.  **最内层步长检查（元素连续性）：**
        *   `GGML_ASSERT(s0 == 1);`
        *   `GGML_ASSERT(s00 == 1);`
        *   `GGML_ASSERT(s10 == 1);`

        这些断言检查确保了目标张量 (`dst`)、第一个源张量 (`src0`) 和第二个源张量 (`src1`) 的最内层维度（维度 0）的元素步长 (`s0`, `s00`, `s10`) 为 1。这意味着最内层维度的元素在内存中是严格连续的。这种连续性对于 GPU 上高效的逐元素操作至关重要，因为它允许在 CUDA 内核的最内层循环中进行简单的线性访问，从而促进内存合并（memory coalescing）和向量化。非单位步长将需要更复杂的索引计算，并可能导致性能下降。

属于 `ggml_cuda_op_bin_bcast` 的操作有：

- `ggml_cuda_op_repeat`：把 `src0` 的数据进行重复，存到 `dst` 中。
- `ggml_cuda_op_add`：tensor 元素相加
- `ggml_cuda_op_sub`：tensor 元素相减
- `ggml_cuda_op_mul`：tensor 元素相乘
- `ggml_cuda_op_div`：tensor 元素相除

### ggml_cuda_op_acc

将 `src0` 和 `src1` 的数据相加，存到 `dst` 中。

- 检查 `src0`、`src1` 和 `dst` 的类型是否都为 F32
- 检查 `dst` 是否为 3D 张量

### ggml_cuda_op_unary

执行一元操作，模板函数。

- 检查 `src0` 是否连续
- 检查 `src0` 和 `dst` 的类型是否为 F32 或 F16，并且 `dst` 的类型必须和 `src0` 相同

对于不同种类的一元操作并没有额外检查。

### ggml_cuda_op_norm

对 `src0` 进行归一化操作，存到 `dst` 中。

- 检查 `src0` 和 `dst` 的类型是否为 F32
- 从 `dst.op_params` 中获取 `eps` 参数，检查是否为正数
- `src0` 张量的最内层维度（维度 0）的字节步长必须等于其数据类型的大小，即最内层维度的元素必须连续

### ggml_cuda_op_group_norm

对 `src0` 进行组归一化操作，存到 `dst` 中。

- 检查 `src0` 和 `dst` 的类型是否为 F32
- 检查 eps 参数是否为正数

### ggml_cuda_op_l2_norm

对 `src0` 进行 L2 归一化操作，存到 `dst` 中。

检查与 `ggml_cuda_op_norm` 相同。

### ggml_cuda_op_concat

将两个 32 位浮点型张量 `src0` 和 `src1` 沿着指定的维度进行拼接，结果存储在 `dst` 中。

- 检查 `src0`、 `src1` 和 `dst` 的类型是否为 F32
- 之后根据 `src0` 和 `src1` 的连续性采用不同的计算方式

### ggml_cuda_op_upscale

对 `src0` 进行上采样操作，存到 `dst` 中。

- 检查 `src0` 和 `dst` 的类型是否为 F32

### ggml_cuda_op_pad

对 `src0` 进行填充操作，存到 `dst` 中。

- 检查 `src0` 和 `dst` 的类型是否为 F32
- 限定 `src0` 和 `dst` 为 3D 张量

### ggml_cuda_op_arange

生成一个一维张量 `dst`，其元素为等差数列。

- 检查 `dst` 的类型是否为 F32
- 从 `dst.op_params` 中获取 `start`、`stop` 和 `step` 参数，检查生成的元素个数是否和 `dst` 的元素个数相同

### ggml_cuda_op_timestep_embedding

为 `src0` 生成时间步嵌入，存到 `dst` 中。

- 检查 `src0` 和 `dst` 的类型是否为 F32

### ggml_cuda_op_leaky_relu

对 `src0` 进行 Leaky ReLU 操作，存到 `dst` 中。

- 检查 `src0` 是否连续
- 检查 `src0` 和 `dst` 的类型是否为 F32 或 F16，并且 `dst` 的类型必须和 `src0` 相同

### ggml_cuda_op_silu_back

对 `src0` 进行 SiLU 操作的反向传播，存到 `dst` 中。

检测与 `ggml_cuda_op_leaky_relu` 相同。

### ggml_cuda_op_rms_norm

对 `src0` 进行 RMS 归一化操作，存到 `dst` 中。

检查与 `ggml_cuda_op_norm` 相同。

### ggml_cuda_op_rms_norm_back

对 `src0` 进行 RMS 归一化操作的反向传播，存到 `dst` 中。

- 检查 `src0`、`src1` 和 `dst` 的类型是否为 F32
- 检查 eps 参数是否为正数

### ggml_cuda_mul_mat

矩阵乘法。

根据不同输入选择不同的计算方式：

- `use_mul_mat_vec`：`src0` 类型为非量化类型（F32、F16、BF16），且 `src1` 为 F32 向量。
- `use_mul_mat_vec_q`：`src0` 为量化类型，且 `src1` 为第二维大小不超过 `MMVQ_MAX_BATCH_SIZE` 的 F32 张量。
- `use_mul_mat_q`：`src0` 为量化类型，且 `src1` 为 F32 张量。
- 其他情况使用通用的矩阵乘法 `ggml_cuda_op_mul_mat`

#### ggml_cuda_mul_mat_vec

矩阵和向量相乘。

- 检查 `src1` 和 `dst` 的类型是否为 F32
- 检查 `src1` 第四维度和 `dst` 的第四维度是否相等
- 检查 `src0`、`src1` 和 `dst` 最内层维度元素是否连续

#### ggml_cuda_mul_mat_vec_q

量化矩阵和向量相乘。

检查和 `ggml_cuda_mul_mat_vec` 类似。

#### ggml_cuda_mul_mat_batched_cublas

使用 cuBLAS 进行批量矩阵乘法。

- 检查 `src0` 和 `src1` 是否为非转置布局（`nb0 > nb1`）
- 检查 `src0` 的后端是否为 CUDA 且类型为 F16
- 检查 `src1` 的第三维是否为 `src0` 第三维的整数倍
- 检查 `src1` 的第四维是否为 `src0` 第四维的整数倍（这两个检查应该是为了保证广播操作的正确性）

#### ggml_cuda_op_mul_mat

通用矩阵乘法。