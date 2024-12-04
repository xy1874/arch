# 练习3 (选做)：利用循环和分块提升计算性能

- **任务**：
    - 修改练习2代码，设计恰当循环顺序和数量进一步提升矩阵乘法性能。  
    - 本练习为选做内容，根据具体实现及加速效果加分。

- **目标**：
    - 学会通过优化算法提高缓存命中率；  
    - 学会根据缓存大小优化矩阵乘法分块大小提升性能。

#### 1. 根据练习1的分析结果，完善`src/lab2/gemm_kernel_opt_loop.S`中`DO_GEMM`过程（第55行）的矩阵计算逻辑
 
<center><img src="../assets/3-3-1.png" width = 450></center>

#### 2. 验证kernel结果的正确性

``` bash linenums="1"
# 项目根目录下执行
mkdir -p build && cd build
cmake -B . -S ../ && cmake --build ./ --target lab2_gemm_kernel_opt_loop.unittest
./dist/bins/lab2_gemm_kernel_opt_loop.unittest
```

<center><img src="../assets/3-3-2.png" width = 530></center>

&emsp;&emsp;对比优化后的算法与基线的性能（测试用例根据自己的优化选择），根据对比结果分析优化方案的局限性。

``` bash linenums="1"
mkdir -p build && cd build
cmake -B . -S ../ && cmake --build ./ --target lab2_gemm_opt_loop
./dist/bins/lab2_gemm_opt_loop 4 32768 4
```

<center><img src="../assets/3-3-3.png" width = 650></center>
