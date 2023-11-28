# Layout

## Layout 描述

### 历史阶段

- BLAS: row/column major + leading dimension
- Tensor: shape + stride
- Hierarchy Tensor

### 一维向量

shape(8), stride(1) 表示包括 8 个逻辑位置，逻辑位置和物理位置映射时每个元素之间的差为 1

其计算逻辑为 $index_{physical} = index_{logical} * stride$

```text
shape(8), stride(1)
logical:   0 1 2 3 4 5 6 7
physical:  0 1 2 3 4 5 6 7
```

```text
shape(8), stride(2)
logical:   0   1   2   3   4   5     6     7
physical:  0 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15
```

```text
shape(8), stride(0)
logical:   0 1 2 3 4 5 6 7
physical:  0
```

```text
shape(8), stride(-1)
logical:                        0 1 2 3 4 5 6 7
physical:  -7 -6 -5 -4 -3 -2 -1 0
```

### 二维矩阵

shape(2, 3), stride(3, 1) 表示 2 行 3 列的矩阵。对于每行之间，逻辑位置和物理位置映射时每个元素之间的差为 3；对于每列之间，逻辑位置和物理位置映射时每个元素之间的差为 1

二维空间的列优先描述：shape(3, 4), stride(1, 3) ；行优先描述：shape(3, 4), stride(4, 1)

```text
shape(2, 3), stride(3, 1)

logical:   (0,0) (0,1) (0,2)
           (1,0) (1,1) (1,2)

offset:    0 1 2
           3 4 5

pyhsical:  0 1 2 3 4 5
```

其映射关系为 $index_{physical} = \sum_{i:dim} index_{logical,i} * stride_i$

### 有层次的 Layout

![Alt text](https://pic1.zhimg.com/80/v2-f392da122542ca7d1609cb1ae85a912c_1440w.webp)

其中

```text
A:
(4, 8) <- shape
(1, 4) <- stride
```

表示 4 行 8 列，每行物理位置之差为 1，每列物理位置之差为 4

```text
C:
[4, (2, 4)] <- shape
[2, (1, 8)] <- stride
```

表示 4 行 8 列的矩阵，且对列进行分块大小为 2 的分块，每行物理位置之差为 1，每列中分块内相邻元素物理位置之差为 1，相邻分块之间物理位置之差为 8。

cute 提供了 `make_shape` 和 `make_stride` 两个函数来构造层次的 shape 和 stride.

常量 shape：

```c++
auto shape = make_shape(Int<2>{}, Int<3>{});
auto shape1 = make_shape(shape, Int<3>{});
```

变量 shape：

```c++
auto shape = make_shape(m, n);
```

## Layout 的代数和几何解释

### 基本属性

![Alt text](https://pic4.zhimg.com/80/v2-9c45d4061a5fda3ee48d4bb4a882dff7_1440w.webp)

如上 Layout ，基本属性如下表：

| shape | stride | size | rank | depth | coshape | cosize |
|:-----:|:------:|:----:|:----:|:-----:|:-------:|:------:|
| ((2, 4), <br>(3, 5)) | ((3, 6), <br>(1, 24)) | 120 | 2 | 2 | 120 | 120 |

- shape 和 stride 表示 layout 的逻辑形状和每个维度在地址中的步长
- size 表示逻辑空间的大小
- rank 表示 layout 的秩，等于 shape 第一层的元素个数
- depth 表示 layout 的嵌套深度，定义非嵌套 layout 的 depth 为 1
- coshape 表示 codomain 的空间大小
- cosize 表示占用空间大小，如果 stride 不紧凑，则 cosize 可能大于 size

### Coordinate 坐标

- 一层 layout：指定行列坐标```auto coord = make_coord(1, 2);``` 
- 多层：将 make_coord 进行嵌套：

    ```c++
    auto coord = make_coord(make_coord(1, 3), make_coord(2, 4));
    ```

    注意 make_coord 参数从内到外。上述指定的坐标如下：

    ![Alt text](https://pic4.zhimg.com/80/v2-dbf1d65ebb1b94080d149c8052dfebef_1440w.webp)

### Slice 切片

cute 提供了 Underscore 类型对某个维度进行全选，对应变量为 `_`，类似 python 中 `:` 。

### Complement 补集

当 codomain 不连续时，可以构造原 layout 的补集 layout2 补上 codomain 空缺的空间（注意：layout 的 codomain 和 layout2 的 codomain 可能有重合部分），为了表示的简洁性，补集会被压缩为最小表示，周期性重复的部分会被约掉。

![Alt text](https://pic4.zhimg.com/80/v2-d9bac5fef72c489238bf31bd1a660d67_1440w.webp)

### Product 乘法

