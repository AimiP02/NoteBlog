# 汇编语言与接口技术

### 基础

`2^10 = 1KB`

`2^20 = 1MB`

`2^30 = 1GB`

### 微处理器管理模式(8086)

#### 通用寄存器&段寄存器

| 通用寄存器 | 作用                                                |
| ----- | ------------------------------------------------- |
| AX    | the accumulator register (divided into AH / AL).  |
| BX    | the base address register (divided into BH / BL). |
| CX    | the count register (divided into CH / CL).        |
| DX    | the data register (divided into DH / DL).         |
| BP    | base pointer.                                     |
| DI    | destination index register.                       |
| SI    | source index register.                            |
| SP    | stack pointer.                                    |

| 段寄存器 | 作用                                                                                                         |
| ---- | ---------------------------------------------------------------------------------------------------------- |
| CS   | points at the segment containing the current program                                                       |
| DS   | generally points at segment where variables are defined                                                    |
| ES   | extra segment register, it’s up to a coder to define its usage. ES is used as a temporary segment register |
| SS   | points at the segment containing the stack                                                                 |

#### 标志寄存器

| 标志寄存器 | 作用                     |
| ----- | ---------------------- |
| CF    | 进位标志（最高位）              |
| OF    | 溢出标志（检测带符号数运算是否发生溢出）   |
| ZF    | 零标志                    |
| SF    | 符号标志（结果为负SF=1）         |
| AF    | 辅助进位标志（最低4位有进位或借位AF=1） |
| PF    | 结果中二进制位数的奇偶（偶数PF=1）    |
| IF    | 中断允许标志                 |
| DF    | 方向标志                   |
| TF    | 陷阱标志                   |

<figure><img src=".gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

```armasm
MOV AL, 80H
MOV AL, -128
```

在计算机中因为是补码表示，`AL`的值是相同的

```
80H  -> 1000000
-128 -> 1000000
```

#### 实模式

在8086中，`CS`和`IP`共同构成代码段寻址访问，`CS * 0x10H + IP`

`DS`同理

<figure><img src=".gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src=".gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>
