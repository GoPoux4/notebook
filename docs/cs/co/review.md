# 复习

## 内存层次架构

### Memory Technology

- SRAM：静态随机存取存储器，数据存储在触发器中，速度快但是需要更大的面积
- DRAM：动态随机存取存储器，数据存储在电容中，速度慢但是面积小

### Memory Hierarchy

- block：转移数据的最小单位
- hit：上层存储器找到目标数据，不用访问下层存储器
    - hit rate：命中率 = hit / access
    - hit time：访问时间 + 判断是否命中的时间
- miss：上层存储器未找到目标数据，需要访问下层存储器
    - miss rate：失效率 = 1 - 命中率
    - miss penalty：替换数据的时间 + 将数据返回给处理器的时间

两个问题：

1. 速度问题：使用 cache
2. 容量问题：使用虚拟内存 Virtual Memory

### Cache

地址都以字节为单位

- cache line：缓存行，cache 的最小单位，最少包括 valid bit、tag、data
- 地址划分：tag + index + offset
    - offset：取决于 cache line 中 data 段的大小，如 data 为 64 字节，则 offset 为 6 位
    - index：取决于 cache 的大小和组数
    - tag：剩下的部分，用于比较是否命中

1. Direct-Mapped Cache：直接映射缓存，每个 block 只能映射到一个 cache line
2. Fully Associative Cache：全相联缓存，每个 block 可以映射到任意一个 cache line
3. Set Associative Cache：组相联缓存，cache 被划分为多个组，每个 block 可以映射到某个组中的任意一个 cache line，组内采用直接映射，组的大小称为 ways。地址中 index 为组号

#### Block Replacement

- Random Replacement：随机替换
- LRU Replacement：最近最少使用替换，需要在每个 cache line 中维护 lru bits
- FIFO Replacement：先进先出替换

#### Write Policy

- Write-Through：写直达，写入 cache 同时写入主存，cache 中的数据永远是最新的
- Write-Back：写回，写入 cache 后不写入主存，只有当 cache line 被替换时才写入主存，需要额外的 dirty bit

写回需要时间，CPU 会进行 write stall 等待写直达完成，可以使用 write buffer 缓解。副作用：读的时候需要在 buffer 中也查找。

当发生 write miss 时，有两种策略：

- Write Allocate：写分配，将 block 读入 cache 后再写入，常与 write-back 结合
- Write Around：写绕过，直接写入主存，不读入 cache

#### Memory System to Support Cache

不同 memory 架构

1. Performance basic memory organization

    CPU - cache - memory

    - 假设
        - 1 cycle send address to cache
        - 15 cycles for each DRAM access initiated by cache
        - 1 cycle to send data
        - block size 4 words
    - 那么
        - transfer 一个 block 的需要 \(1 + 15 + 1 = 17\) cycles
        - miss penalty 为 \(1 + 4 \times (15 + 1) = 65\) cycles
        - 带宽 bandwidth 为 \(\frac{4 \times 4}{65} \approx 0.25\) words/cycle
2. Performance in Wider Main Memory

    CPU - cache - multiple caches - memory

    - 一次可以读出两个 word
        - miss penalty 为 \(1 + 2 \times (15 + 1) = 33\) cycles
        - 带宽 bandwidth 为 \(\frac{4 \times 4}{33} \approx 0.48\) words/cycle
    - 一次性读出 4 个 word
        - miss penalty 为 \(1 + 15 + 1 = 17\) cycles
        - 带宽 bandwidth 为 \(\frac{4 \times 4}{17} \approx 0.98\) words/cycle
3. Performance in Four-way interleaved memory

    CPU - cache - 4 memory banks

    4 个 memory bank，依次进行读取，并行访问

    - miss penalty 为 \(1 + 15 + 4 \times 1 = 20\) cycles
    - 带宽 bandwidth 为 \(\frac{4 \times 4}{20} = 0.8\) words/cycle

### Measuring and improving cache performance

#### Measuring cache performance

- Average memory access time

    \[
    \begin{aligned}
    \mathrm{AMAT} &= \mathrm{hitTime} + \mathrm{missTime} \\
    &= \mathrm{hitRate} \times \mathrm{cacheTime} + \mathrm{missRate} \times \mathrm{memoryTime}
    \end{aligned}
    \]

- CPU Time = (CPU execution clock cycles + Memory stall clock cycles) \(\times\) clock cycle time
- Memory stall clock cycles = Number of instructions \(\times\) miss rate \(\times\) miss penalty = read stall cycles + write stall cycles
    - Read stall cycles = \(\displaystyle\frac{\text{Reads}}{\text{Program}} \times\) read miss rate \(\times\) read miss penalty
    - Write stall cycles = \(\displaystyle\frac{\text{Writes}}{\text{Program}} \times\) write miss rate \(\times\) write miss penalty + write buffer stall cycles
        - 如果 cache block size 为一个 word，那么 write miss penalty 为 0
    - 忽略 write buffer stall cycles 有
    
        \[
        \begin{aligned}
        \text{Memory stall clock cycles} &= \frac{\text{Memory accesses}}{\text{Program}} \times \text{miss rate} \times \text{miss penalty} \\
        &= \frac{\text{Instructions}}{\text{Program}} \times \frac{\text{Misses}}{\text{Instruction}} \times \text{miss penalty}
        \end{aligned}
        \]

#### Improving Cache Performance

提高 cache 命中率，减小 penalty.

- Reducing cache misses by more flexible placement of blocks 灵活的 block 放置
    - 使用 set-associative cache 替代 direct-mapped cache
- Reducing the miss penalty using multilevel caches 多级缓存
    - 增加 L2 cache

### Virtual Memory

把主存当作磁盘缓存。采用全相联 LRU 替换。

Page Table：将虚拟地址映射到物理地址

每一个 process 都有自己的 page table，和一个 page table base register 指向 page table。

!!! example "计算页表大小"
    - 虚拟地址 32 位
    - 页大小 4 KB
    - 页表中每条 entry size 4 bytes

    页表中 entry 个数为 \(2^{32} / 2^{12} = 2^{20}\)，页表大小为 \(2^{20} \times 4 = 4 MB\)

#### TLB

Translation Lookaside Buffer，缓存 page table entry。看作 Page table 的 cache，一般 16-512 entries.

## 存储和 I/O

### Amdahl's Law

串行部分的比例决定了加速的上限。

!!! example "多核加速"
    100 个 CPU，要加速 90 倍，求串行部分比例。

    \(T_{\text{new}} = \frac{T_{\text{parallelizable}}}{100} + T_{\text{serial}}\)

    \[
    \begin{aligned}
    \text{Speedup} &= \frac{T_{\text{parallelizable}} + T_{\text{serial}}}{\frac{T_{\text{parallelizable}}}{100} + T_{\text{serial}}} \\
    &= \frac{1}{\frac{f_{\text{parallelizable}}}{100} + (1 - f_{\text{parallelizable}})} = 90
    \end{aligned}
    \]

    解得 \(f_{\text{parallelizable}} = 0.999\)，串行部分比例为 0.1%

### Accesse Time

- Seek Time：寻道时间，磁头移动到目标磁道的时间
- Rotational Latency：旋转延迟，等待目标数据旋转到磁头下的时间
- Transfer Time：传输时间，读取数据的时间
- Controller Time：控制器时间，处理数据的时间

总的访问时间为上述四者之和。

### Flash Memory

不易失性存储器，读取速度快

- NOR Flash：随机读取，作为指令存储
- NAND Flash：以块为单位读取，作为数据存储

### Dependablity

可靠性

- MTTF：Mean Time To Failure，平均无故障时间
- MTTR：Mean Time To Repair，平均修复时间
- MTBF：Mean Time Between Failure，平均故障间隔 = MTTF + MTTR

可用时间为 \(\frac{MTTF}{MTTF + MTTR}\)

### Redundant Arrays of Inexpensive Disks

磁盘阵列，能够同时访问

- RAID 0：数据分块存储，提高读写速度
- RAID 1：每一个盘的数据都有备份，写的时候同时写入两个盘
- RAID 3：对数据盘上的数据位进行奇偶校验，存储在另一个盘上，但故障时不知道具体是哪一个盘出了问题
- RAID 4：每个盘的校验位没有依赖关系，出错时可以知道具体是哪一个盘出了问题
- RAID 5：将校验位分散存储在所有盘上，提高读写速度
- RAID 6：两个校验位，提高容错能力，但是写入速度慢

### Bus

Synchronous vs. Asynchronous

- Synchronous 同步总线：所有设备频率一致
- Asynchronous 异步总线：使用 handshake 协议

    读数据时：
    
    1. CPU 拉起 read request，并将地址放在 data bus 上
    2. 设备接收到 read request，读取 data bus 上的地址，拉起 ack 信号表示收到 read request
    3. CPU 收到 ack 信号，复位 read request，等待数据；内存看到 read request 复位，将 ack 信号拉低
    4. 设备读出数据，放在 data bus 上，拉起 data ready 信号
    5. CPU 看到 data ready 信号，读取 data bus 上的数据，拉起 ack 信号表示收到数据
    6. 设备收到 ack 信号，复位 data ready 信号；CPU 看到 data ready 信号复位，将 ack 信号拉低

#### Performance

带宽 bandwidth = 数据传输量 / 传输时间

- Increasing data bus width
- Use separate address and data lines
- transfer multiple words

### I/O

CPU 通过 I/O 地址访问设备（这个地址不能进入 cache，要保持最新）

交互方式：

- Polling：CPU 不断查询设备状态
- Interrupts：设备完成后发送中断信号给 CPU
- DMA：直接内存访问，设备直接访问内存，不需要 CPU 参与