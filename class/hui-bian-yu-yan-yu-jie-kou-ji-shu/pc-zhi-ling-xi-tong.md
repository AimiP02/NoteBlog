---
description: 第三章
---

# PC指令系统

## 寻址方式

### 立即寻址方式

操作数直接包含在指令中，紧跟在操作码之后的寻址方式称为立即寻址方式，把该操作数称为立即数

```armasm
; e.g
MOV AX, 1234H ; AX = 1234H
MOV EAX, 12345678H ; EAX = 12345678H
```

其中1234H就是立即寻址

### 寄存器寻址方式

操作数直接包含在寄存器中，由指令指定寄存器号的寻址方式

寄存器可以是8位、16位、32位通用寄存器或16位段寄存器（但CS不能用于目标）

```armasm
; e.g
MOV EBX, EAX ; EBX = EAX
MOV EDI, 5678H ; EDI = 5678H
```

其中EBX、EAX、EDI就是寄存器寻址

### 直接寻址方式

```armasm
;e.g
MOV EAX, [00404011H]
MOV EBX, ES:[78H]
```

\[00404011H]、ES:\[78H]为直接寻址方式

### 寄存器间接寻址方式

```armasm
;e.g
MOV AL, [BX] ; AL = (DS:[BX])
```

### 寄存器相对寻址方式

```armasm
;e.g
MOV AL, 8[BX] ; => MOV AL, [BX + 8]
              ; => AL = (DS:[BX + 8])
```

### 基址变址寻址方式

```armasm
;e.g
MOV AL, [BX][SI] ; AL = (DS:[BX + SI])
```

### 相对基址变址寻址方式

```armasm
;e.g
MOV AL, ARY[BX][SI] ; AL = (DS:[BX + SI + ARY])
```

利用这种寻址方式可以访问形如ARY\[3]\[3]的二维数组

### 比例变址寻址方式

```armasm
;e.g
MOV CX, [EAX+2*EDX]
MOV EBX, [EBP+4*ECX+10H]
MOV EDX, ES:ARY[4*EBX] ; EDX = (ES:[4*EBX+ARY])
```

## 指令系统

### 数据传送指令

* 传送指令 `MOV DST, SRC`

功能：DST=SRC

> 注意：
>
> * 源操作数和目标操作数的数据类型匹配问题，即应同为字节、字或双字型数据。
> * 立即数不能作为目标操作数
> * 立即数不能直接送段寄存器
> * 目标寄存器不能是CS
> * 两个段寄存器间不能直接传送
> * 两个存储单元之间不能直接传送

内存赋值时必须添加显式说明

```armasm
MOV WORD PTR [BX], 10 ; WORD PTR表示BX指向的内存单元为2字节
```

* 带符号扩展的数据传送指令 `MOVSX DST, SRC (386以上)`

功能：DST=SRC，DST空出的位用SRC的符号位扩展

> DST必须是16位或32位寄存器操作数，SRC可以是8位或16位寄存器操作数或存储器操作数，但不能是立即数

* 带零扩展的数据传送指令`MOVZX DST, STC (386以上)`

功能：DST=SRC，DST空出的位用0填充

> DST必须是16位或32位寄存器操作数，SRC可以是8位或16位寄存器操作数或存储器操作数，但不能是立即数

* 交换指令 `XCHG OPR1, OPR2`

功能：交换两个操作数

### 栈操作指令

程序内存的栈是LIFO的

SS是栈基址，SP是栈顶地址

* 进栈指令 `PUSH`

``
