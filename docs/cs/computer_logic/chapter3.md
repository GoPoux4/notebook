# Combinational Logic Design

## Combinational Logic

### Functions and Functional Blocks

verilog 中，调用模块会在电路中增加实例，会增加开销。

verilog 中并行运行。

### Multiple-bit Rudiementary Functions

A wide line is used to represent a bus (总线). The bus can be split into individual lines.

### Enable Function

与门 + 非门

<div align=center><img src="/assert/img/CS/computer_logic/chapter3/control.png" width = 60%/></div>

### Decoding

the conversion of an n-bit input code to an m-bit output code with $n \leq m \leq 2^n$ such that each valid code word produces a unique output code

Example:

- 1-to-2-line decoder

    <div align=center><img src="/assert/img/CS/computer_logic/chapter3/1-to-2.png" width = 60%/></div>

- 2-to-4-line decoder

    <div align=center><img src="/assert/img/CS/computer_logic/chapter3/2-to-4.png" width = 80%/></div>

行列译码器：将输入不断二分，例如 4-to-16 拆成 2-to4 和 2-to-4，再将 2-to-4 拆成 1-to-2 和 1-to-2。

### Logic Functions Implementation

实现任意逻辑函数：译码器 + 或门

译码器输出是所有最小项，通过或门实现逻辑函数。

## Design Procedure

### Combinational Circuits

输入与输出严格对应

### Beginning Hierarchical Design

- Top-Down: 从顶层开始设计，逐步细化
- Bottom-Up: 从最底层开始设计，逐步合并

### Design Procedure

- Specification
- Formulation

    Derive a truth table or initial Boolean equations for each output function.

    Apply hierarchical design to the output functions.

- Optimization

    Apply multiple-level logic optimization to the output functions.

    Draw logic diagrams.