## 1. 分块矩阵乘法与遍历

&emsp;&emsp;假设矩阵 $A$ 为 $M×K$ 大小的矩阵，矩阵 $B$ 为 $K×N$ 大小的矩阵，根据《线性代数》的定义，矩阵 $A$ 和矩阵 $B$ 的相乘的结果为 $M×N$ 大小的矩阵 $C$ ，矩阵 $C$ 的元素可由下述公式计算：

$$
C_{ij}=\sum_{k=1,2,⋯,K} A_{ik}⋅B_{kj}, \text{ } i=1,2,⋯,M; \text{ } j=1,2,⋯,N
$$

&emsp;&emsp;假设矩阵 $A$、矩阵 $B$ 和矩阵 $C$ 可按《线性代数》中分块矩阵乘法的定义划分成如图2-1所示的子矩阵。由分块矩阵乘法的定义可知 $C_{00}=A_{00}⋅B_{00}+A_{01}⋅B_{10}+A_{02}⋅B_{20}$。要完成 $C_{00}$ 的计算必须沿 $K$ 维度完成矩阵 $A$ 和矩阵 $B$ 所有分块的点乘的计算。

&emsp;&emsp;一般情况下，通常会先遍历结果矩阵的两个维度，最后再遍历剩下的一个维度。如上面介绍的矩阵 $C$ 元素的计算公式就是典型的 $ijk$ 遍历方式。这种遍历方式的优点在于最后一个维度遍历完即可完成矩阵 $C$ 的一个元素的计算。

&emsp;&emsp;除了 $ijk$ 遍历方式外，$kij$ 遍历方式（也称秩 $K$ 更新）的使用也较为普遍。不同于 $ijk$ 遍历方式，使用 $kij$ 遍历方式计算矩阵 $C$ 时需要存中间结果。由图2-1可知，遍历矩阵 $A$ 的一列和矩阵 $B$ 的一行，只能得到矩阵 $C$ 每个元素的一部分结果，只有当矩阵 $A$ 和矩阵 $B$ 的所有分块全遍历完时才能得到矩阵 $C$ 的最终结果。相较于 $ijk$ 遍历方式，$kij$ 遍历方式能较好的保证空间访问的局部性，提高缓存的利用率。

<center><img src="../assets/2-1.png" width = 430></center>
<center>图2-1 $kij$ 遍历的矩阵乘法算法</center>



## 2. x86-64简述

&emsp;&emsp;x86-64是x86的64位扩展，其在保持良好兼容性的基础上将寄存器和虚拟地址空间扩展到64位，并提升了指令集的功能和性能。

### 2.1 寄存器概述

&emsp;&emsp;x86-64架构包含状态和控制寄存器、程序指针寄存器、通用寄存器、向量寄存器等多种类型的寄存器，如表2-1所示。

<center>表2-1 x86-64架构的部分寄存器</center>
<center><table>
    <tr>
        <th align="center">类型</th>
        <th align="center">名称</th>
        <th align="center">位宽</th>
        <th align="center">说明</th>
    </tr>
    <tr>
        <td rowspan=1 style="vertical-align: middle;" align="center">控制和状态<br>寄存器</td>
        <td style="vertical-align: middle;" align="center">RFLAGS</td>
        <td style="vertical-align: middle;" align="center">64</td>
        <td align="left">处理器标志位信息，包括零标志（ZF）、符号位（SF）、进位位（CF）、溢出位（OF）、中断使能位（IF）等</td>
    </tr>
    <tr>
        <td rowspan=1 style="vertical-align: middle;" align="center">指针寄存器</td>
        <td style="vertical-align: middle;" align="center">RIP</td>
        <td style="vertical-align: middle;" align="center">64</td>
        <td align="left">当前指令的地址</td>
    </tr>
    <tr>
        <td rowspan=9 style="vertical-align: middle;" align="center">通用寄存器</td>
        <td style="vertical-align: middle;" align="center">RAX</td>
        <td style="vertical-align: middle;" align="center">64</td>
        <td align="left">常用作累加、存放<u><b>乘积或被除数的低64位</b></u>、存放<u><b>除法的商</b></u>，或传递<u><b>子程序的返回值</b></u></td></td>
    </tr>
    <tr>
        <td style="vertical-align: middle;" align="center">RBX</td>
        <td style="vertical-align: middle;" align="center">64</td>
        <td align="left">常用作基数寄存器</td>
    </tr>
    <tr>
        <td align="center">RCX</td>
        <td style="vertical-align: middle;" align="center">64</td>
        <td align="left">常用作计数器，或传递<u><b>子程序调用时的第4个参数</b></u></td>
    </tr>
    <tr>
        <td style="vertical-align: middle;" align="center">RDX</td>
        <td style="vertical-align: middle;" align="center">64</td>
        <td align="left">常用作数据寄存器、存放<u><b>乘积或被除数的高64位</b></u>、存放<u><b>除法的余数</b></u>，或传递<u><b>子程序调用时的第3个参数</b></u></td>
    </tr>
    <tr>
        <td style="vertical-align: middle;" align="center">RSI</td>
        <td style="vertical-align: middle;" align="center">64</td>
        <td align="left">常用作字符串操作的源索引（Source Index）寄存器，或传递<u><b>子程序调用时的第2个参数</td>
    </tr>
    <tr>
        <td style="vertical-align: middle;" align="center">RDI</td>
        <td style="vertical-align: middle;" align="center">64</td>
        <td align="left">常用作字符串操作的目标索引（Destination Index）寄存器，或传递<u><b>子程序调用时的第1个参数</td>
    </tr>
    <tr>
        <td style="vertical-align: middle;" align="center">RSP</td>
        <td style="vertical-align: middle;" align="center">64</td>
        <td align="left">栈指针</td>
    </tr>
    <tr>
        <td style="vertical-align: middle;" align="center">RBP</td>
        <td style="vertical-align: middle;" align="center">64</td>
        <td align="left">栈基址指针</td>
    </tr>
    <tr>
        <td style="vertical-align: middle;" align="center">R8~R15</td>
        <td style="vertical-align: middle;" align="center">64</td>
        <td align="left">通用；R8、R9分别用作传递<u><b>子程序调用时的第5、6个参数</td>
    </tr>
    <tr>
        <td rowspan=2 style="vertical-align: middle;" align="center">向量寄存器</td>
        <td style="vertical-align: middle;" align="center">XMM0~XMM15</td>
        <td align="center">128</td>
        <td align="left">SSE (Streaming SIMD Extension) 寄存器；YMM的低128位</td>
    </tr>
    <tr>
        <td style="vertical-align: middle;" align="center">YMM0~YMM15</td>
        <td align="center">256</td>
        <td align="left"><a href="https://www.intel.com/content/www/us/en/developer/technical-library/overview.html?f:@stm_10309_en=%5BIntel%C2%AE%20Technologies%3BIntel%C2%AE%20Data%20Center%20Technologies%3BIntel%C2%AE%20Advanced%20Vector%20Extensions%20(Intel%C2%AE%20AVX)%5D" target=_blank>AVX (Advanced Vector eXtension)</a> 寄存器</td>
    </tr>
</table></center>

### 2.2 寻址方式

&emsp;&emsp;x86-64架构的常用寻址方式包括立即数寻址、寄存器寻址、直接寻址、寄存器间接寻址、基址变址寻址和比例变址寻址，如表2-2所示。

<center>表2-2 x86-64架构的寻址方式</center>
<center><table>
    <tr>
        <th align="center">寻址方式</th>
        <th align="center">使用方法</th>
        <th align="center">说明</th>
    </tr>
    <tr>
        <td align="center">立即数寻址</td>
        <td align="left">`mov $0x1234, %rax`</td>
        <td align="left">RAX $\leftarrow$ `0x1234`</td>
    </tr>
    <tr>
        <td align="center">寄存器寻址</td>
        <td align="left">`mov %r8, %rax`</td>
        <td align="left">RAX $\leftarrow$ R8</td>
    </tr>
    <tr>
        <td align="center">直接寻址</td>
        <td align="left">`mov 0x1234, %rax`</td>
        <td align="left">RAX $\leftarrow$ MEM[`0x1234`]</td>
    </tr>
    <tr>
        <td align="center">寄存器间接寻址</td>
        <td align="left">`mov (%rsp), %rax`</td>
        <td align="left">RAX $\leftarrow$ MEM[ RSP ]</td>
    </tr>
    <tr>
        <td align="center">基址变址寻址</td>
        <td align="left">`mov -20(%rsp), %rax`</td>
        <td align="left">RAX $\leftarrow$ MEM[ RSP - 20 ]</b></u></td>
    </tr>
    <tr>
        <td align="center">比例变址寻址</td>
        <td align="left">`mov -20(%rsp, %rcx, 4), %rax`</td>
        <td align="left">RAX $\leftarrow$ MEM[ RSP - 20 + RCX*4 ]</td>
    </tr>
</table></center>

&emsp;&emsp;由表2-2可知，直接寻址、寄存器间接寻址和基址变址寻址均可视为比例变址寻址的特殊情况。

### 2.3 常用指令

<embed src = "../assets/x86-64-reference.pdf" width="100%" height="910">



## 3. x87 FPU

### 3.1 寄存器概述

&emsp;&emsp;<a href="../assets/x87.pdf" target=_blank>x87 FPU浮点运算单元</a>包含8个80位的通用寄存器 `R0` ~ `R7`、3个16位的状态和控制寄存器、2个48位的指针寄存器以及1个11位的操作码寄存器，如图2-2所示。

<center><img src="../assets/2-2.png" width = 430></center>
<center>图2-2 x87 FPU的寄存器</center>

&emsp;&emsp;其中，通用寄存器用于存储计算数据。数据被写入通用寄存器时将被FPU自动扩展至80位的双扩展精度浮点数（Double Extended-Precision Floating-Point）；而当通用寄存器中的数据被写入内存时，可以在不同精度的浮点数、整数或BCD码的多种格式当中灵活选择一种进行写入。

&emsp;&emsp;x87 FPU指令支持以栈的形式访问8个通用寄存器，且处于栈顶的数据始终被标记为 `st(0)`，如图2-3中的 (a) ~ (d)；同时也支持通过相对 `st(0)` 的偏移量来访问任意一个通用寄存器，如图2-3中的 (e)。

<center><img src="../assets/2-3.png" width = 600></center>
<center>图2-3 通用寄存器访问示意图</center>

### 3.2 常用指令

&emsp;&emsp;x87 FPU支持加载指令、存储指令和运算指令。加载指令将数据压入通用寄存器栈。存储指令将栈中数据弹出并存储到主存单元。运算指令实现浮点数据的加减乘除和求相反数等操作。

&emsp;&emsp;x87 FPU指令的命令规则是前缀 `f` + 前缀 `i` (可选) + 操作 + 后缀 `p`/`s`/`l`/`t`(可选) 。其中，前缀 `i`、后缀 `s`/`l`/`t` 分别表示指令的操作数是整数、单精度浮点数、双精度浮点数和双扩展精度浮点数；后缀 `p` 表示执行该指令时需要在运算完成后从通用寄存器栈中弹出一个数据。

!!! example "x87指令示例"
    - 例1：`fistpl (%rax)` 表示将 `st(0)` 中的双精度浮点数转换成整数后存储到 `%rax` 寄存器指向的存储单元，并从FPU栈中弹出一个数据。

    - 例2：`faddp %st(3), %st(0)` 表示将 `st(3) + st(0)` 的计算结果保存在 `st(3)`，然后把 `st(0)` 弹栈。

<embed src = "../assets/x87-inst.pdf" width="100%" height="910">
