<!-- # 练习：实现GPU矩阵乘法 -->

- **任务**：在模板代码基础上完成 CUDA 矩阵乘法的实现。

- **子任务**：
    - 验证 GPU 矩阵乘法结果的正确性；  
    - 测试不同规模的矩阵乘法，观察实验结果。

- **目标**：
    - 熟悉 GPU 编程模型；
    - 学习 CPU 实现矩阵乘法实现思路；  
    - 熟悉 CUDA 文件编译和执行过程。

- **实验代码**：`matrix_mul.cu`

#### 0. 下载并解压实验包

&emsp;&emsp;从<a href="../../#1" target=_blank>首页链接</a>下载`lab4-5.tar.gz`，将其拷贝至用户目录，并执行命令`tar -zxvf lab4-5.tar.gz`解压实验包。

#### 1. 打开`lab4-5`目录中的模板代码`matrix_mul.cu`，在30行处的`MatrixMulKernel`函数下实现 CUDA 矩阵乘的代码。将结果矩阵的位置索引计算代码填入指定位置（第34、38行）

<center><img src="../assets/3-1-1.png" width = 100%></center>

#### 2. 将计算矩阵乘结果的`for`循环写入到指定位置（第46行），每个线程计算矩阵 C 一个元素的值

<center><img src="../assets/3-1-2.png" width = 100%></center>

#### 3. 将计算结果赋值给结果矩阵的对应位置（第51行）

<center><img src="../assets/3-1-3.png" width = 100%></center>

#### 4. 检查`main`函数是否调用`MatrixMulKernel`函数

<center><img src="../assets/3-1-4.png" width = 100%></center>

#### 5. 保存文件，执行编译命令脚本，执行可执行文件`a.out`

``` bash linenums="1"
bash compile.sh
./a.out 1 1000
```

&emsp;&emsp;在命令`./a.out 1 1000`中，“`1`”表示对矩阵乘法运算结果进行校验，“`1000`”表示测试时输入矩阵的边长。

#### 6. 查看计算GPU计算矩阵乘时间和计算结果是否正确

<center><img src="../assets/3-1-5.png" width = 100%></center>

#### 7. 更改矩阵尺寸（矩阵为方阵）、第13行的线程块大小`TILE_SIZE`（线程块也是方阵），对比不同参数下的计算结果

<center><img src="../assets/3-1-6.png" width = 100%></center>

<center><img src="../assets/3-1-7.png" width = 100%></center>


