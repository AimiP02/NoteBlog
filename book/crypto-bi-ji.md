# Crypto笔记

### 笔记

#### DES

DES解密通过将**子密钥**反转即可

简单的题一般都是利用Feistel结构改写DES，解密的方法是一样的

#### AES

**Byte-at-a-Time攻击**

AES\_CBC和AES\_ECB模式下，第一个块加密的数据是明文异或$$\rm IV$$后的数据，$$\rm IV$$是确定的，所以相同的数据加密出来的结果一定是一样的。可以利用这个特性，通过构造数据来获取需要的数据的某一位，再逐字节获取完整的数据

**CBC-IV-Detection**

此攻击可以在CBC模式下获取未知的$$\rm IV$$值，首先在CBC模式下解密

$$
\begin{aligned} P_1 & = D(C_1) \oplus {\rm IV} \\ P_2 & = D(C_2) \oplus C_1 \\ P_3 & = D(C_3) \oplus C_2 \end{aligned}
$$

假设此时的$$C_1$$和$$C_3$$相等，并且$$C_2$$为一个全0的块，即

$$
\begin{aligned} P_1 & = D(C_1) \oplus {\rm IV} \\ P_2 & = D({\rm 'x00'*BLOCK\_LEN}) \oplus C_1 \\ P_3 & = D(C_1) \oplus ({\rm 'x00'*BLOCK\_LEN}) = D(C_1) \end{aligned}
$$

此时可以知道$$D(C_1)$$的值，和$$P_1$$异或一下就可以得到$$\rm IV$$的值

**CBC-Padding-Oracle**

如果在与服务器进行交互的时候，可以从服务器得知Padding是否正常，就可能可以用这种攻击方式。在使用分组密码算法时，数据是分组进行加密的，数据长度不足的情况下会用**PKCS#7填充算法**

#### 线性同余生成器LCG

标准的LCG的生成序列满足下列递推式：

$$
x_{n+1} = (Ax_n+B) \mod{M}
$$

在已知$$M$$的情况下，如果能获取连续的两个$$x_i$$，就可以建立一个关于$$A$$和$$B$$的方程，获取多个$$x_i$$，则可以获得方程组

$$
\begin{aligned} x_{i+1} & = (Ax_i + B) \mod{M} \\ x_{j+1} & = (Ax_j + B) \mod{M} \end{aligned}
$$

#### 线性反馈移位寄存器LFSR

LFSR进行密钥流生成时，每次从移位寄存器中移出一位作为当前的结果，而移入的位由反馈函数对寄存器中的某些位进行计算来确定。

为了让LFSR获得最大的周期，即$n$位的LFSR获得$$2^n-1$$的周期，对于反馈函数$$F$$的选取有以下方法：选取$$GF(2)$$上的一个$$n$$次的本原多项式，当$$n=32$$时，选取

$$
x^{32}+x^{7}+x^{5}+x^{3}+x^{2}+x+1
$$

那么可以得到$$F$$函数为

$$
F = s_{32} \oplus s_{7} \oplus s_{5} \oplus s_{3} \oplus s_{2} \oplus s_{1}
$$

#### RSA

RSA通常有下面几种常见的攻击方式

**因式分解**

当$$n=pq$$不是很大的时候，可以通过sage或者factordb直接分解出$$p$$和$$q$$

如果$$p$$和$$q$$的差距比较小的话，也可以枚举$$p-q$$的方式算出来

$$
(\frac{p+q}{2})^2 - pq = (\frac{p-q}{2})^2
$$

**低加密指数小明文攻击**

如果被加密的$$m$$非常小，而且公钥$$e$$较小，导致加密后的$$c$$仍然小于$$n$$，那么可以直接对密文$$c$$开$$e$$次方，即可得到明文$$m$$

**共模攻击**

如果使用了相同的$$n$$，不同的模数$$e_1$$、$$e_2$$，且$$e_1$$和$$e_2$$互素，对同一组明文进行加密，得到了密文$$c_1$$、$$c_2$$，那么可以在不计算私钥的情况下计算出明文$$m$$

设

$$
\begin{aligned} c_1 & = m^{e_1} \mod{n} \\ c_2 & = m^{e_2} \mod{n} \end{aligned}
$$

由于$$e_1$$、$$e_2$$互素，那么

$$
xe_1+ye_2=1
$$

这是一个二元不定方程，用扩展欧几里得就可以得到$$x$$和$$y$$的值，再计算

$$
c^x_1 \times c^y_2 \mod{n} = m^{xe_1} \times m^{ye_2} \mod{n} = m^1 \mod{n} = m
$$

**广播攻击**

对于相同的明文$$m$$，使用相同的指数$$e$$和不同的模数$$n_1$$、$$n_2$$、...、$$n_i$$，加密的到$$i$$组明文时（$$i \geq e$$），可以使用中国剩余定理解出明文

$$
\left\{ \begin{matrix} c_1 & = m^e \mod{n_1} \\ c_2 & = m^e \mod{n_2} \\ c_3 & = m^e \mod{n_3} \\ ...\\ c_i & = m^e \mod{n_i} \\ \end{matrix} \right.
$$

使用中国剩余定理，可以求得一个$$c_x$$满足

$$
c_i \equiv m^e \mod{\prod^i_{j=1}n_j}
$$

**低解密指数攻击**

当解密指数$$d$$较低时，设$$ed=1+k\varphi(n)$$，当$$q<p<2q$$时，若满足

$$
d < \frac{1}{3}n^{\frac{1}{4}}
$$

则通过搜索连分数$$e/N$$的收敛，可以有效率地找到$$k/d$$，从而恢复正确的$$d$$

**RSA LSB Oracle**

**Coppersmith's High Bits Attack**

### 题目

#### KeyBoard

空格分开的每组在键盘上围住了一个字母，记下来即可得到flag

`flag:n1book{password}`

#### N1DES

一个模仿DES写的Feistel结构的对称加密算法，计算出SBox的逆`inv_sbox`，再将子密钥反向就可以得到flag

`flag:n1book{4_3AsY_F3istel_n3tw0rk~~}`

#### N1AES

一个模仿AES写的对称加密算法，原题已经给出SBox的逆`inv_sbox`，只要将加密操作完全反过来就是解密操作

在最后一步列混淆的时候，要先用sage计算出逆矩阵，代码如下

```python
k.<a> = GF(2)[]
l.<x> = GF(2^8, modulus = a^8 + a^6 + a^2 + 1)
res = []
for i in range(4):
    res2 = []
    t = [3, 10, 8 ,6]
    for j in range(4):
        res2.append(l.fetch_int(t[(j+i)%4]))
    res.append(res2)
res = Matrix(res)
res.inverse()
```

`flag:n1book{~w0w~_Y0u_1e4rN3d_AES!~~}`

#### LFSR

还没看懂

#### BabyRSA

直接分解n计算$\varphi(n)$再计算私钥就可以得到flag

`flag:n1book{ju5t_f4ctor1z3_N}`

#### CommonModulus

共模攻击，用扩展欧几里得解即可得到flag

`flag:n1book{c0mm0n_m0du1u5_4ttacK}`
