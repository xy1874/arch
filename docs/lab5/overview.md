# 实验5：使用共享内存优化GPU代码

## 实验目的

&emsp;&emsp;1. 掌握使用CUDA中的共享内存优化技巧提升矩阵乘法性能的方法；

&emsp;&emsp;2. 掌握分析GPU程序性能瓶颈的方法；

&emsp;&emsp;3. 掌握利用共享内存优化数据访问路径的方法。



## 实验前准备

&emsp;&emsp;阅读以下资料，以了解如何在CUDA中使用共享内存优化矩阵乘法的基本原理。

- 《CUDA C Programming Guide》  
- 《CUDA C++ Best Practices Guide》

&emsp;&emsp;实验环境：一台安装了CUDA Toolkit、NVIDIA驱动程序和相关开发工具（如`nvcc`、`cuda-gdb`等）的计算机，操作系统为Ubuntu x64。




## 实验内容

&emsp;&emsp;1. 使用CUDA的共享内存机制优化矩阵乘法算法，并对比优化前后的计算时间和效率；

&emsp;&emsp;2. 调整`BLOCKSIZE`大小，对比不同`BLOCKSIZE`对运算效率的影响；

&emsp;&emsp;3. 对比所实现的矩阵乘法与CUDA自带的矩阵乘法算子`cublasSgemm`之间的运行结果。


