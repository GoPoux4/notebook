# NVIDIA CUDA C 

!!! abstract
    GPU 编程原理与 NVIDIA CUDA C 编程实验

## GPU 编程原理

TODO:

## CUDA C/C++ 编程

由 NVIDIA 深度学习培训中心 (DLI) 提供的课程，在由 NVIDIA 提供的云环境上进行实验。

### 使用 CUDA C/C++ 加速程序

#### 加速系统

加速系统又称异构系统，由 CPU 和 GPU 组成。加速系统运行 CPU 程序，由这些程序调度运行于 GPU 上的函数，通过 GPU 实现函数的并行计算。

查询 GPU 信息命令：

```bash linenums="1"
nvidia-smi
```

#### GPU 加速原理

一个简单的 GPU 加速应用程序执行过程：

![](/assert/img/HPC/HPC%20101%20labs/CUDA/img1.png)

+ `(1)` 段：数据由 `cudaMallocManaged()` 函数分配，并能由 CPU 访问处理。
+ `(2)` 段：数据可被迁移至可执行并行工作的 GPU ，并由 GPU 核函数访问，同时 CPU 可继续执行工作（异步执行）。
+ 通过 `cudaDeviceSynchronize()` ，将 CPU 与 GPU 的工作同步。
+ `(3)` 段：经 CPU 访问的数据迁移回 CPU。

#### GPU 加速应用程序编写

CUDA 加速程序文件扩展名为 `.cu` ，一个简单例子：

```cpp linenums="1"
void CPUFunction()
{
  printf("This function is defined to run on the CPU.\n");
}

__global__ void GPUFunction()
{
  printf("This function is defined to run on the GPU.\n");
}

int main()
{
  CPUFunction();

  GPUFunction<<<1, 1>>>();
  cudaDeviceSynchronize();
}
```

一些语法：

`__global__ void GPUFunction()`

  - `__global__` 关键字表明以下函数将在 GPU 上运行并可 **全局** 调用。
  - 通常，将在 CPU 上执行的代码称为 **主机 (Host)** 代码，而将在 GPU 上运行的代码称为 **设备(Device)** 代码。
  - 使用 `__global__` 关键字定义的函数需要返回 `void` 类型。

`GPUFunction<<<1, 1>>>();`

  - 通常，当调用要在 GPU 上运行的函数时，将此种函数称为 **已启动** 的 **核函数** 。
  - 启动核函数时，必须提供 **执行配置** ，即在向核函数传递参数之前使用 `<<<number_of_blocks, threads_per_block>>>` 语法完成配置。
  - 在宏观层面，可通过执行配置为核函数启动指定 **线程层次结构** ，从而定义线程组（称为 **线程块** ）的数量 `number_of_blocks` ，以及要在每个线程块中执行的 **线程** 数量 `threads_per_block` 。如样例代码中，使用了包含 `1` 线程的 `1` 线程块启动核函数。

`cudaDeviceSynchronize();`

  - 核函数启动方式为 **异步** ：CPU 代码将继续执行，无需等待核函数完成启动。
  - 调用 CUDA 运行时提供的函数 `cudaDeviceSynchronize` 使得主机 (CPU) 代码暂作等待，直至设备 (GPU) 代码执行完成，才能在 CPU 上恢复执行。

!!! info "编译运行 CUDA 加速程序"
    使用 `nvcc` 命令编译和运行程序：

    ```bash linenums="1"
    nvcc -arch=sm_70 -o hello-gpu hello-gpu.cu -run
    ```

    - `nvcc` 为编译器命令。
    - `-arch` 选项指定架构类型。
    - `-o` 指定编译程序的输出文件。
    - `-run` 标志执行成功编译的二进制文件。

#### CUDA 线性层次结构

![](/assert/img/HPC/HPC%20101%20labs/CUDA/img2.png)

启动核函数时，核函数代码由每个已配置的线程块中的每个线程执行。

CUDA 提供了一系列线程层次结构变量：

- `gridDim.x` 网格中块数；`blockDim.x` 每块中线程数。
- `blockIdx.x` 当前线程所在线程块索引；`threadIdx.x` 当前线程在线程块中的索引。

#### 加速循环

步骤：

- 必须编写完成 **循环的单次迭代** 工作的核函数。
- 由于核函数与其他正在运行的核函数无关，因此执行配置必须使核函数执行正确的次数，例如循环迭代的次数。（注意：各个线程执行顺序不定，故可加速的循环需要各次迭代无关联且顺序无影响）

例子：

加速前：

```cpp linenums="1"
for (int i = 0; i < N; ++i)
{
  printf("%d\n", i);
}
```

加速后：使用核函数实现单词迭代

```cpp linenums="1"
__global__ void loop() {
    int idx = threadIdx.x + blockIdx.x * blockDim.x;
    if (idx < N)
        printf("%d\n", idx);
}
```

并调用

```cpp linenums="1"
loop<<<number_of_blocks, threads_per_block>>>();
```

此处必须有 `number_of_blocks * threads_per_block >= N` 。

#### 分配将要在 GPU 和 CPU 上访问的内存

回忆一般的 CPU 程序分配并释放内存的方式：

```cpp linenums="1"
int *a;
a = (int *)malloc(size);
...
free(a);
```

要分配和释放内存，并获取可在主机和设备代码中引用的指针，则需要使用 CUDA 提供的函数 `cudaMallocManaged()` 和 `cudaFree()` ：

```cpp linenums="1"
int *a;
cudaMallocManaged(&a, size);
...
cudaFree(a);
```

#### 跨网格循环

当数据集比网格大的时候，可以使每个线程处理多个数据：

```cpp linenums="1"
__global__ void kernel(int *a, int N)
{
  int idx = threadIdx.x + blockIdx.x * blockDim.x;
  int stride = gridDim.x * blockDim.x;

  for (int i = idx; i < N; i += stride)
  {
    // do work on a[i];
  }
}
```

#### 错误处理

- 许多 CUDA 函数会返回类型为 `cudaError_t` 的值，可用于检查调用是否出错。
- 为检查启动核函数时是否发生错误，CUDA 提供了 `cudaGetLastError` 函数，返回类型为 `cudaError_t` 的值。
- 为捕捉异步错误（例如，在异步核函数执行期间），需要检查后续同步 CUDA 运行时 API 调用所返回的状态（例如 `cudaDeviceSynchronize` ），如果之前启动的其中一个核函数失败，则将返回错误。

错误大致可分为同步错误 `synError` 和异步错误 `asynError` 。

对于 `cudaError_t` 值，将其与 CUDA 提供的 `cudaSuccess` 进行比较，即可判断是否出错，同时可以使用函数 `cudaGetErrorString()` 获得错误信息。

一个包装 CUDA 函数调用的宏：

``` cpp linenums="1"
inline cudaError_t checkCuda(cudaError_t result)
{
  if (result != cudaSuccess) {
    fprintf(stderr, "CUDA Runtime Error: %s\n", cudaGetErrorString(result));
    assert(result == cudaSuccess);
  }
  return result;
}

int main()
{

/*
 * The macro can be wrapped around any function returning
 * a value of type `cudaError_t`.
 */

  checkCuda( cudaDeviceSynchronize() )
}
```

#### 进阶内容 - 多维网格和块

可以将网格和线程块定义为最多具有 3 个维度， CUDA 提供 `dim3` 类型定义多维网格和块：

```cpp linenums="1"
dim3 threads_per_block(block_dim_x, block_dim_y, block_dim_z);
dim3 number_of_blocks(grid_dim_x, grid_dim_y, grid_dim_z);
someKernel<<<number_of_blocks, threads_per_block>>>();
```

在核函数中，可以使用 `threadIdx.y` 及类似形式获得相关索引和维度。