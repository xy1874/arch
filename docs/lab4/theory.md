## 矩阵乘法及CUDA优化

&emsp;&emsp;在本实验中，我们将学习如何使用 CUDA（Compute Unified Device Architecture）进行并行编程，以加速矩阵乘法计算。CUDA 是由 NVIDIA 推出的并行计算平台和编程模型，它允许开发者利用 GPU 的计算能力来处理大规模并行计算任务。利用 CUDA 编写的程序可以显著提升矩阵乘法的计算性能，使得处理大规模矩阵变得更加高效。

&emsp;&emsp;在矩阵乘法中，对于两个矩阵 $A$ 和 $B$，它们的乘积矩阵 $C$ 的每个元素可以通过以下公式计算得到

$$
C_{ij} = \sum_{k=1}^n A_{ik} \cdot B_{kj}
$$

&emsp;&emsp;其中，$C_{ij}$ 表示矩阵 $C$ 的第 $i$ 行第 $j$ 列元素。计算矩阵 $C$ 的每个元素需要遍历矩阵 $A$ 的行和矩阵 $B$ 的列，因而时间复杂度为 $O(m×n×p)$，对于大规模矩阵而言计算量非常大。

&emsp;&emsp;CUDA 用于加速矩阵乘法计算的原理是基于 GPU 的大规模并行计算架构，通过让多个线程协作以实现高效的数据处理。以下是 CUDA 加速矩阵乘法的关键原理：
	
- 多线程并行计算：CUDA 通过 GPU 上成千上万的并行线程来加速计算。在矩阵乘法中，每个元素的计算是相互独立的，CUDA 可以将每个计算任务分配给一个线程，多个线程同时执行，从而极大地提高计算效率。
	
- 块和网格结构：在 CUDA 中，线程被组织成线程块和网格。每个线程块负责计算矩阵的一部分，并在共享内存中存储中间计算结果。这种分层结构允许更好的内存管理和并行处理。例如，可以将一个线程块的每个线程分配给结果矩阵中的一个元素计算任务。
	
- 隐藏内存延迟：GPU 拥有极高的线程数量，这些线程通过切换掩盖内存延迟。当某些线程因内存读取而被阻塞时，其他线程可以继续执行，从而能够有效地隐藏延迟，使计算单元保持高利用率。
	
- 并行计算能力与指令级并行度（ILP）：CUDA 架构支持在每个时钟周期内执行多个指令。通过将指令流水线优化为并行执行路径，CUDA 能够加速矩阵乘法等计算任务，并使得在同一时间内完成更多操作，提高整体计算效率。
	
- 计算密集型与内存密集型平衡：在矩阵乘法中，CUDA 可以通过调节线程数、块大小和内存分配策略，使得计算和内存访问得到优化平衡，从而更好地利用 GPU 资源。
 
<center><img src="../assets/2-1.png" width = 400></center>
<center>图2-1 CUDA计算矩阵乘</center> 

&emsp;&emsp;如图2-1所示，每个线程负责计算结果矩阵 $P$ 中的一个元素。矩阵 $M$ 和矩阵 $N$ 都存储在全局内存中。每个线程从全局内存中读取 $M$ 的一行和 $N$ 的一列，然后执行点积运算得到 $P$ 中的一个元素。

&emsp;&emsp;通过这些并行处理技术，CUDA 在计算密集型任务（如矩阵乘法）中能够显著加速计算过程，从而在科学计算、图像处理等领域中得到广泛应用。

### 1. GPU 结构简介

&emsp;&emsp;GPU 主要由流处理器阵列和存储系统组成，其结构如图2-2所示。

<center><img src="../assets/2-2.png" width = 580></center>
<center>图2-2 GPU 结构示意图</center> 

&emsp;&emsp;单个流处理器称为 GPU 的一个线程（Thread）。所有 Thread 被组织成 “线程格（Grid） - 线程块（Block） - 线程” 的层次结构。GPU 内包含多个 Grid，一个 Grid 包含多个 Block，一个 Block 包含多个 Thread。根据不同的应用场景，这些结构可以在逻辑上视为二维、三维甚至更高维度的组织方式。

&emsp;&emsp;每个 Thread 具有私有的寄存器和局部存储器（Local Memory）。同属一个 Block 的所有 Thread 共享线程块中的共享存储器（Shared Memory）。所有 Block 共享全局存储器（Global Memory）、常量存储器（Constant Memory）和针对数据局部性进行优化的高性能存储器（Texture Memory），且 Host 与 GPU 也共享这三个存储器。在计算时，Host 需要先将待处理数据批量传输到这些存储器，由 GPU 处理完毕后再从这些存储器读回运算结果。

### 2. CUDA 程序基本组成

&emsp;&emsp;CUDA 程序主要由 Host 代码与核函数组成。Host 代码通常是一段运行在 Host（即CPU）上的 C/C++ 程序，其作用是申请存储空间、预处理数据、拷贝数据、分配计算所需的 CUDA 线程数、调用核函数等。核函数则是运行在 Device（即GPU）的硬件线程（或核心）上的代码，故被称为核函数。

&emsp;&emsp;下面是一个矩阵加法的 CUDA 示例程序：

``` c linenums="1"
// Device核函数（运行在GPU的硬件线程上）
__global__ void matrixAdd(float A[Ny][Nx], float B[Ny][Nx], float C[Ny][Nx]) 
{
    // 获取矩阵元素的行号、列号
    int row = blockIdx.y * blockDim.y + threadIdx.y;
    int col = blockIdx.x * blockDim.x + threadIdx.x; 

    C[row][col] = A[row][col] + B[row][col];
}

// Host代码（运行在CPU上）
int main()
{
    ......

    const int Nx = 12; 
    const int Ny = 6;

    // 定义一个Block包含4*3个Thread
    dim3 threadsPerBlock(4, 3); 
    // 定义一个Grid包含3*2个Block
    dim3 numBlocks(Nx/threadsPerBlock.x, Ny/threadsPerBlock.y); 
    // 使用72个线程并行执行核函数（6个线程块，每个12线程）
    matrixAdd<<<numBlocks, threadsPerBlock>>>(A, B, C);
    
    ......
}
```

- Line 14 ~ 18：定义变量、分配空间、预处理数据、拷贝数据等。

- Line 20 ~ 22：定义 Grid 和 Block 的大小。

- Line 24：使用定义好的 Grid 和 Block 参数调用核函数执行矩阵加法，所有线程将并行执行核函数中的代码。

- Line 2：使用`__global__`关键字声明核函数。

- Line 5 ~ 6：使用 Block 和 Thread 的尺寸属性计算矩阵元素的行号和列号。`blockIdx`表示当前正在执行核函数的 Thread 所在的 Block 索引，`blockIdx.x`和`blockIdx.y`分别表示 Block 在 x 轴（横轴）和 y 轴（纵轴）方向的坐标。`blockDim`表示当前 Block 的尺寸，`blockDim.x`和`BlockDim.y`分别表示当前 Block 的列数和行数。`threadIdx`同理。


