# Digital Hardware Implementation

## Programmable Implementation Technologies

### Programmable Technologies

- Control connections 连接控制

    - Mask programming: 生产时进行连接。不可再编程。
    - Fuse: 保险丝，加大电流烧断保险丝，断开连接。不可再编程。
    - Antifuse: 电流烧断保险丝，短接连接。不可再编程。
    - Single-bit storage element: 存储某个位是否连接。可再编程。

- Build lookup tables: 多路选择器。可再编程。
- Control transistor switching 选择控制

    - Stored charge on a floating transistor gate: 电荷存储在浮动晶体管栅极上。可再编程。

        1. EPROM: 电子可擦除可编程只读存储器。用紫外线擦除。
        2. EEPROM: 电子可擦除可编程只读存储器。用电擦除。
        3. Flash memory: 闪存。用电擦除。可再编程。


!!! note "分类"

    1. Permanent: 

        - Mask programming
        - Fuse
        - Antifuse

    2. Reprogrammable:
        - Volatile: 电源断电后数据丢失

            - Single-bit storage element

        - Nonvolatile: 电源断电后数据不丢失

            - EPROM
            - EEPROM
            - Flash memory

### Programmable Configurations

- Read Only Memory (ROM): a fixed array of AND gates and a programmable array of OR gates
- Programmable Array Logic (PAL): a programmable array of AND gates feeding a fixed array of OR gates.
- Programmable Logic Array (PLA): a programmable array of AND gates feeding a programmable array of OR gates.
- Complex Programmable Logic Device (CPLD) / Field-Programmable Gate Array (FPGA): complex enough to be called "architectures", use lookup tables.

<div align=center><img src="/assets/img/CS/computer_logic/chapter5/programmable.png" width = 70%/></div>

#### Read Only Memory

- N input lines, 
- M output lines,
- 2N decoded minterms.

<div align=center><img src="/assets/img/CS/computer_logic/chapter5/rom.png" width = 40%/></div>

交叉点画 X，表示该交叉点连接。可编程。

!!! example

    实现输入 $x$，输出 $x^2$。

    <div align=center><img src="/assets/img/CS/computer_logic/chapter5/square.png" width = 50%/></div>

    由于 $B_0 = A_0, B_1 = 0$，故只需要 $8\times 4$ ROM。

    <div align=center><img src="/assets/img/CS/computer_logic/chapter5/square1.png" width = 30%/></div>

    根据真值表编程 ROM：

    <div align=center><img src="/assets/img/CS/computer_logic/chapter5/squaretable.png" width = 30%/></div>

#### Programmable Array Logic

Having a programmable set of ANDs combined with fixed ORs.

<div align=center><img src="/assets/img/CS/computer_logic/chapter5/pal.png" width = 40%/></div>

（F1 返回到输入：缓解了输入的限制）

#### Programmable Logic Array

Having a programmable set of ANDs combined with a programmable set of ORs.

<div align=center><img src="/assets/img/CS/computer_logic/chapter5/pla.png" width = 60%/></div>

异或门：重用与项，缓解与门数量的限制。

!!! example

    <div align=center><img src="/assets/img/CS/computer_logic/chapter5/plaexample.png" width = 60%/></div>

    （求整体取反之后的化简结果：将 k-map 反转）

    <div align=center><img src="/assets/img/CS/computer_logic/chapter5/plaexample1.png" width = 60%/></div>