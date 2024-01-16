# Sorting

## Shell Sort

Define an increment sequence \(1 = h_1 < h_2 < \cdots < h_t\).

Define \(h\)-sort: 用插入排序排序原数组的子序列 \(a_0, a_h, a_{2h}, \cdots\)。

Shell sort: 按照 $k = t, t-1, \cdots, 1$ 的顺序，对每个 $k$ 进行 $h_k$-sort。

### Time Complexity

与增量序列有关。

#### Shell's increment sequence

\(h_t = \lfloor \frac{n}{2} \rfloor, h_{k+1} = \lfloor \frac{h_k}{2} \rfloor\)

最坏情况下，只有最后一次排序进行了交换，复杂度为 \(O(n^2)\)。

#### Hibbard's increment sequence

\(h_k = 2^k - 1\)

最坏情况下，复杂度为 \(O(n^{3/2})\)。

平均情况下，复杂度为 \(O(n^{5/4})\)。

## Heap Sort

两种实现：

- 单独建堆，然后每次取出最小值。
- 在原数组上建堆，每次取出最小值。

一般采用第二种实现。第一种实现空间翻倍。

第二种实现的平均比较次数为 \(2n \log n - O(n \log \log n)\)。

## Merge Sort

Each merge step takes \(O(n)\) time. The number of merge steps is \(O(\log n)\).

The total time complexity is \(O(n \log n)\).

## Quick Sort

每次选取一个元素作为 pivot，然后将数组分为两部分，左边的元素都小于 pivot，右边的元素都大于 pivot。递归进行排序。

复杂度取决于 pivot 的选择。

- Wrong choice: `pivot = a[0]`
- Safe choice: `pivot = a[rand() % n]` 但是生成随机数存在额外开销
- Better choice: `pivot = median(a[0], a[n/2], a[n-1])`

### Partitioning Strategy

1. 选取 pivot，将其放到最后。
2. 两个指针 `i` 初始指向第一个元素，`j` 初始指向除了 pivot 的最后一个元素。
3. `i` 从左向右扫描找到第一个小于 pivot 的元素，`j` 从右向左扫描找到第一个大于 pivot 的元素，交换两个元素。
4. 重复 3 直到 `j < i`，此时 `i` 指向第一个大于 pivot 的元素，交换 `a[i]` 和 `a[n-1]`。
5. 递归排序 `a[0..i-1]` 和 `a[i+1..n-1]`。

若遇到与 pivot 相同的数，则 `i` 和 `j` 都不移动，进行交换，使得最后 pivot 在中间。复杂度为 \(O(n\log n)\)。

如果不交换，可能会出现最坏情况，复杂度为 \(O(n^2)\)。

### Small Arrays

Cutoff：**在递归过程中**，对于小数组，使用插入排序，大数组使用快速排序。

### Implementation

- **Median3**: 将左中右三个元素进行排序，取中间值作为 pivot。
- **Qsort**: 递归排序。

### Analysis

\[T(n) = T(i) + T(n-i-1) + O(n)\]

- **Worsest Case**: \(T(n) = T(n-1) + O(n)\)，复杂度为 \(O(n^2)\)。
- **Best Case**: \(T(n) = 2T(n/2) + O(n)\)，复杂度为 \(O(n \log n)\)。
- **Average**: \(T(n) = O(n \log n)\)。

!!! note "Find the k-th"

    每次排序好之后，能够得出 pivot 是第几大的元素，然后只对左边或者右边进行排序。

### Sorting Large Structures

用指针排序。

!!! note

    只需要一个多余的空间，即可将最后的结果的原始数据存储在数组中。

## General Lower Bound for Sorting

Any algorithm that sorts by comparisons only must have a worst case computing time of \(\Omega(n \log n)\).

## Bucket Sort

数据的范围有限，将相同数据根据数据内容分配到不同的桶中，然后按顺序输出桶中的数据。

假设数据有 \(n\) 个，范围大小为 \(m\)，则时间复杂度为 \(O(n + m)\)。

## Radix Sort

将数据按照位进行排序。Sort according to the Least Significant Digit (LSD) first.

先按照个位排序，然后按照十位排序，以此类推。

若每个位的范围为 \(b\)，数据的位数为 \(p\)，则时间复杂度为 \(O(p(n + b))\)。

符号表达：

- \(K_i^j\): 第 \(i\) 个关键字的第 \(j\) 位
- \(K_i^0\): the most significant digit (MSD)
- \(K_i^{p-1}\): the least significant digit (LSD)

不同排序顺序：

- MSD (Most Significant Digit) sort: first sort \(K^0\)
- LSD (Least Significant Digit) sort: first sort \(K^{p-1}\)

## Stablity

对于一个序列，如果存在两个相等的元素，

- 排序后它们的相对位置不变，则称这个排序算法是稳定的
- 排序后它们的相对位置发生了变化，则称这个排序算法是不稳定的

稳定排序：冒泡、归并、插入、基数、桶排

不稳定排序：快排、希尔、堆排、**选择**