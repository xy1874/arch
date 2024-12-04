# 练习2：基于AVX指令设计并实现高性能矩阵乘法计算内核

- **任务**：利用AVX指令实现性能更好的矩阵乘法计算内核。

- **子任务**：
    - 计算内核 $C$ 矩阵（$C_r$）的元素个数不少于64，即 ${m_r}{n_r} ≥ 64$；  
    - 计算内核各维度必须满足如下要求: $m_r mod 2=0$，$k_r mod 8=0$，$n_r mod 8=0$；  
    - 计算内核采用流水线设计读取、计算、写回等过程。

- **目标**：学会利用向量指令实现计算性能更高的矩阵乘法计算单元。

#### 0. 模板代码`src/lab3/gemm_kernel_opt_avx.S`的实现思路

&emsp;&emsp;模板代码`src/lab3/gemm_kernel_opt_avx.S`采用如下图所示的矩阵分块算法：

<center><img src="../assets/3-2-0.png" width = 100%></center>

&emsp;&emsp;最内层循环每次使用 $2 \times1 $ 的`A[m:m+2][k]`分块和 $1 \times 32$ 的`B[k][n:n+32]`分块计算出 $2 \times 32$ 的`C[m:m+2][n:n+32]`分块。最内层的分块矩阵乘法采用 AVX 向量指令实现并行计算。

#### 1. 为`src/lab3/gemm_kernel_opt_avx.S`的`LOAD_MAT_C`过程（第110行、第117行）添加加载`C[m:m+2][n:n+32]`到AVX寄存器的逻辑
 
<center><img src="../assets/3-2-1.png" width = 650></center>

#### 2. 为`src/lab3/gemm_kernel_opt_avx.S`的`LOAD_MAT_A`过程（第90行）添加加载`A[m+1][k]`到AVX寄存器的逻辑

<center><img src="../assets/3-2-2.png" width = 650></center>

#### 3. 为`src/lab3/gemm_kernel_opt_avx.S`的`LOAD_MAT_B`过程（第96行）添加加载`B[k][n:n+32]`到AVX寄存器的逻辑

<center><img src="../assets/3-2-3.png" width = 650></center>

#### 4. 为`src/lab3/gemm_kernel_opt_avx.S`的`DO_COMPUTE`过程（第141行）添加计算`C[m:m+2][n:n+32]+=A[m:m+2][k]*B[k:k+8][n:n+32]`的逻辑

<center><img src="../assets/3-2-4.png" width = 650></center>

#### 5. 为`src/lab3/gemm_kernel_opt_avx.S`的`STORE_MAT_C`过程（第132行、第135行）添加保存`C[m][n:n+32]`和`C[m+1][n:n+32]`的逻辑

<center><img src="../assets/3-2-5.png" width = 650></center>

#### 6. 验证新内核的正确性

``` bash linenums="1"
# 项目根目录下执行
mkdir -p build && cd build
cmake -B . -S ../ && cmake --build ./ --target lab3_gemm_opt_avx.unittest
./dist/bins/lab3_gemm_opt_avx.unittest
```

<center><img src="../assets/3-2-6.png" width = 600></center>

#### 7. 对比优化后的算法与基线的性能（测试用例根据自己的优化选择），根据算法和结果分析优化方案的优缺点。

``` bash linenums="1"
mkdir -p build && cd build
cmake -B . -S ../ && cmake --build ./ --target lab3_gemm_opt_avx
./dist/bins/lab3_gemm_opt_avx 256 256 256
```

<center><img src="../assets/3-2-7.png" width = 620></center>
