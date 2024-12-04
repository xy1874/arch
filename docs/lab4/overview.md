# 实验4：使用 C/CUDA 编写 GPU 代码

## 实验目的

&emsp;&emsp;1. 掌握使用 C/CUDA 编程实现矩阵乘法；

&emsp;&emsp;2. 学习如何利用 GPU 的多线程并行计算能力提升程序性能；

&emsp;&emsp;3. 掌握 CUDA 的基本编程模型和线程调度方式，理解如何在 GPU 上进行高效的矩阵运算，从而进一步挖掘计算设备的并行处理能力。



## 实验前准备

&emsp;&emsp;阅读以下材料，了解 CUDA 编程的基本原理及 GPU 架构的特性，以实现高效的矩阵乘法计算。

- 《CUDA C Programming Guide》  

&emsp;&emsp;实验环境：一台安装了 CUDA Toolkit、NVIDIA 驱动程序和相关开发工具（如`nvcc`、`cuda-gdb`等）的计算机，操作系统为 Ubuntu x64。




## 实验内容

&emsp;&emsp;1. 使用 C/CUDA 实现 GPU上 的矩阵乘法算法；

&emsp;&emsp;2. 观察和对比不同尺寸的输入矩阵，以及不同块大小下的矩阵乘法的计算结果。


