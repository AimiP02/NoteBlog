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
* 出栈指令 `POP`
* 全部入栈（通用寄存器） `PUSHAD`
* 全部出栈（通用寄存器） `POPAD`

### 算数运算指令

#### 类型转换指令

* 字节扩展成字指令： `CBW` 将AL寄存器中的符号位值扩展到AH中
* 字扩展成双字指令： `CWD` 将AX的符号位值扩展到DX中
* 双字扩展成四字指令：`CDQ` 将EAX符号位扩展到EDX中
* AX符号位扩展到EAX指令：`CWDE` 将AX寄存器符号位扩展到EAX高16位

#### 二进制加法指令

任何一条二进制加、减法指令均适用于带符号数和无符号数运算

* 加法指令`ADD DST, SRC`&#x20;

功能：(DST)+(SRC) -> DST

标志：影响OF、SF、ZF、AF、PF、CF标志

* 带进位加法指令`ADC DST, SRC`&#x20;

功能：(DST)+(SRC)+CF -> DST

标志：影响OF、SF、ZF、AF、PF、CF标志

* 加1指令`INC DST`

功能：(DST)+1 -> DST

标志：除不影响CF标志外，影响其它五个算术运算特征标志

| 指令           | 功能                  | 影响标志位             |
| ------------ | ------------------- | ----------------- |
| ADD DST, SRC | (DST)+(SRC)->DST    | OF/SF/ZF/AF/PF/CF |
| ADC DST, STC | (DST)+(SRC)+CF->DST | OF/SF/ZF/AF/PE/CF |
| INC DST      | (DST)+1->DST        | OF/SF/ZF/AF/PF    |

#### 二进制减法指令

* 减法指令`SUB DST, SRC`

功能：(DST)-(SRC) -> DST

标志：影响OF、SF、ZF、AF、PF、CF标志

* 带借位减法指令`SBB DST, SRC`&#x20;

功能：(DST)-(SRC)-CF->DST

标志：影响OF、SF、ZF、AF、PF、CF标志

* 减1指令`DEC DST`&#x20;

功能：(DST)-1->DST

标志：除不影响CF标志外，影响其它五个算术运算特征标志

* **比较指令 `CMP DST, SRC`**

功能：(DST)-(SRC)

标志：影响OF、SF、ZF、AF、PF、CF标志

* 求补指令`NEG DST`&#x20;

功能：对目标操作数（含符号位）求反加1，并且把结果送回目标；利用NEG指令可实现求一个数的相反数

标志：影响OF、SF、ZF、AF、PF、CF标志

> NEG对CF和OF的影响如下：
>
> a.对操作数所能表示的最小负数(例若操作数是8位则为－128)求补,原操作数不变,但OF被置1
>
> b.当操作数为0时，清0 CF
>
> c.对非0操作数求补后，置1 CF

| 指令           | 功能                  | 影响标志位             |
| ------------ | ------------------- | ----------------- |
| SUB DST, SRC | (DST)-(SRC)->DST    | OF/SF/ZF/AF/PF/CF |
| SBB DST, SRC | (DST)-(SRC)-CF->DST | OF/SF/ZF/AF/PF/CF |
| DEC DST      | (DST)-1->DST        | OF/SF/ZF/AF/PF    |
| CMP DST, SRC | (DST)-(SRC)         | OF/SF/ZF/AF/PF/CF |
| NEG DST      | 求反加1                | OF/SF/ZF/AF/PF/CF |

### 位运算指令

#### 逻辑运算指令

<figure><img src="../../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

作业：91页代码段改成32位多个双字的减法；3.11、3.12、3.14、3.16
