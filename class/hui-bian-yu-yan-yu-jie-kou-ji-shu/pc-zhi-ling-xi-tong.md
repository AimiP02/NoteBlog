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

## 操作指令

