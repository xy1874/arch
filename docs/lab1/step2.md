# 练习2：实现矩阵乘法

- **任务**：使用提供的汇编代码实现矩阵乘法，并调整代码以处理不同大小的矩阵。

- **目标**：
    - 演示寄存器间接寻址和基址+变址寻址方式；  
    - 初始化矩阵数据，并在汇编中使用指针。

#### 1. 为`src/lab1/gemm_kernel.S`中的`GEMM_INIT`过程（第56行）添加合适的矩阵B地址保存指令
 
<center><img src="../assets/3-2-1.png" width = 600></center>

#### 2. 为`src/lab1/gemm_kernel.S`中的`DO_GEMM`过程（第70行）添加加载`A[m][k]`到FPU寄存器栈的逻辑

<center><img src="../assets/3-2-2.png" width = 600></center>

#### 3. 为`src/lab1/gemm_kernel.S`中的`DO_GEMM`过程（第77行）添加加载`B[k][n]`到FPU寄存器栈的逻辑

<center><img src="../assets/3-2-3.png" width = 600></center>

#### 4. 为`src/lab1/gemm_kernel.S`中的`DO_GEMM`过程（第85行）添加加载`C[m][n]`到FPU寄存器栈的逻辑

<center><img src="../assets/3-2-4.png" width = 600></center>

#### 5. 验证矩阵乘法内核的正确性

``` bash linenums="1"
mkdir -p build && cd build
cmake -B . -S ../ && cmake --build ./ --target lab1_test_gemm_kernel.unittest
./dist/bins/lab1_test_gemm_kernel.unittest --gtest_filter=gemm_kernel.test0
```

<center><img src="../assets/3-2-5.png" width = 100%></center>

#### 6. 编译上层代码形成二进制程序

``` bash linenums="1"
mkdir -p build && cd build
cmake -B . -S ../ && cmake --build ./ --target lab1_gemm
```

#### 7. 运行代码得到输出

``` bash linenums="1"
./dist/bins/lab1_gemm 256 256 256
```

<center><img src="../assets/3-2-6.png" width = 100%></center>
