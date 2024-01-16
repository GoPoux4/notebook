# Hashing

## ADT

- **Objects**: name-attribute pairs
- **Operations**: Insert, Delete, Find, ...

## Hash Table

- **Hash function**: \(f(x) = \text{position of} \; x \; \text{in ht[]}\)
- \(T\): \(x\) 所有可能取值的情况数。
- \(n\): total number of identifiers 哈希表中标识符的个数。
- identifier density: \(n/T\)
- loading density: \(n/\text{size of table}\)
- number of identifiers: \(b\) (TableSize)
- number slots of each identifiers: \(s\)

## Hash Function

- 容易计算，尽量减少碰撞。
- 均匀分布，uniform hash function。

若 \(f(x) = x \mod \text{TableSize}\)，则 TableSize 应该为素数。

## Saparate Chain

每个 identifier 对应一个链表。

此时定义 loading density 为 \(n/b\)。

## Open Addressing

如果发生碰撞，则寻找下一个空的位置。不断探测其后第 \(f(0), f(1), f(2), \dots\) 个位置是否为有位置。

**要求 loading density < 0.5**

### Linear Probing

\(f(i) = i\)，\(i = 0, 1, 2, \dots\)。

会导致 clustering，即一旦发生碰撞，后面的碰撞概率会增大。

### Quadratic Probing

\(f(i) = i^2\)，\(i = 0, 1, 2, \dots\)。

当 TableSize 为素数，且 loading density < 0.5 时，总能找到空位置。

### Double Hashing

\(f(i) = i \times hash_2(x)\)，\(i = 0, 1, 2, \dots\)。

一般选择 \(hash_2(x) = R - (x \% R)\)，其中 \(R\) 为素数，且 \(R < \text{TableSize}\)。

## Rehashing

新建一个约两倍大小（一般选择 \(\text{next_prime}(2*n)\)）的哈希表，然后将原来的数据插入新的哈希表中。

Rehashing 的时机：

- 当 loading density > 0.5 时
- 当 loading density 超过一定值时
- 当插入失败时

设 \(n\) 为当前哈希表中的标识符个数，则 rehashing 的时间复杂度为 \(O(n)\)。

## 错题整理

!!! question "Question 1"

    The average search time of searching a hash table with N elements is **Can not be determined**.

!!! question "Question 2"

    Suppose that the numbers `{4371, 1323, 6173, 4199, 4344, 9679, 1989}` are hashed into a table of size 10 with the hash function \(h(X)=X\%10\), and hence have indices `{1, 3, 4, 9, 5, 0, 2}`. What are their indices after rehashing using \(h(X)=X\%\text{TableSize}\) with linear probing?

    rehashing 新建哈希表大小为 23.