# 练习1：基于x87 FPU优化矩阵乘法性能

- **任务**：利用处理器中的x87 FPU（浮点计算单元）中更多的寄存器对矩阵乘法计算的循环进行展开以提升性能。

- **子任务**：
    - 使用`FLD`、`FSTP`等指令操作数据的读取与存储；  
    - 使用`FMUL`、`FADDP`等x87 FPU提供的指令进行计算；  
    - 以2为步进对 $N$ 维度的循环进行展开（可参考`src/lab3/gemm_kernel_baseline.S`中的 `DO_LOOP_N` 逻辑）

- **目标**：学会利用计算单元的特性进行循环展开。

!!! tip "关于浮点指令"
    &emsp;&emsp;本实验涉及x87 FPU的通用寄存器栈以及`FLD`、`FSTP`、`FMUL`、`FADDP`等常用指令，相关内容可自行回顾<a href="../../lab1/theory/#3-x87-fpu" target=_blank>实验1 - 实验原理 - 第3节 x87 FPU</a>。

#### 0. 下载并解压实验包

&emsp;&emsp;从<a href="../../#1" target=_blank>首页链接</a>下载`lab3.tar.gz`，将其拷贝至用户目录，并执行命令`tar -zxvf lab3.tar.gz`解压实验包。

#### 1. 在`src/lab3/gemm_kernel_opt_loop_unrolling.S`第83行添加计算`A[m][k]*B[k][n+1]->st(0)`的逻辑

<center><img src="../assets/3-1-1.png" width = 650></center>

#### 2. 在`src/lab3/gemm_kernel_opt_loop_unrolling.S`第91行添加加载`C[m][n]->st(1)`和加载 `C[m][n+1]->st(0)`的逻辑

<center><img src="../assets/3-1-2.png" width = 650></center>

#### 3. 在`src/lab3/gemm_kernel_opt_loop_unrolling.S`第93行添加部分和累加逻辑: `C[m][n+1] + A[m][k] * B[k][n+1]` 和 `C[m][n] + A[m][k] * B[k][n]`

<center><img src="../assets/3-1-3.png" width = 650></center>

#### 4. 在`src/lab3/gemm_kernel_opt_loop_unrolling.S`第97行添加保存`C[m][n]`的逻辑

<center><img src="../assets/3-1-4.png" width = 650></center>

#### 5. 在`src/lab3/gemm_kernel_opt_loop_unrolling.S`第99行添加N维度的循环更新逻辑

<center><img src="../assets/3-1-5.png" width = 650></center>

#### 6. 验证循环展开方案的正确性

``` bash linenums="1"
# 项目根目录下执行
mkdir -p build && cd build
cmake -B . -S ../ && cmake --build ./ --target lab3_gemm_opt_loop_unrolling.unittest
./dist/bins/lab3_gemm_opt_loop_unrolling.unittest
```

<center><img src="../assets/3-1-6.png" width = 600></center>

#### 7. 对比优化后的算法与基线的性能（测试用例根据自己的优化选择），根据对比结果分析优化方案的局限性

``` bash linenums="1"
mkdir -p build && cd build
cmake -B . -S ../ && cmake --build ./ --target lab3_gemm_opt_loop_unrolling
./dist/bins/lab3_gemm_opt_loop_unrolling 256 256 256
```

<center><img src="../assets/3-1-7.png" width = 600></center>



