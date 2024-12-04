# 实验3：使用指令级并行、向量指令和并行优化矩阵乘法	

## 实验目的

&emsp;&emsp;1. 理解计算机系统的指令级并行性；

&emsp;&emsp;2. 通过循环展开、向量指令、多线程技术学习如何综合利用现代处理器的多种特性优化程序性能；

&emsp;&emsp;3. 掌握利用指令级并行、向量指令和多线程技术进一步发掘处理器并行性能的方法。



## 实验前准备

&emsp;&emsp;阅读下述论文材料了解设计高性能矩阵乘法计算内核的基本原则和Intel处理器的特性。

- 《LIBXSMM: Accelerating Small Matrix Multiplications by Runtime Code Generation》  
- 《<a href="https://www.intel.cn/content/www/cn/zh/content-details/782158/intel-64-and-ia-32-architectures-software-developer-s-manual-combined-volumes-1-2a-2b-2c-2d-3a-3b-3c-3d-and-4.html" target=_blank>Intel® 64 和 IA-32 架构软件开发人员手册合并卷</a>》的第一卷和第二卷（第5章FPU相关内容，14章AVX指令相关内容等）

&emsp;&emsp;实验环境：一台安装了`cmake`、`gcc`、`gdb`和`perf`环境且系统为ubuntu x64的计算机。



## 实验内容

&emsp;&emsp;1. 利用处理器的FPU，结合循环展开提升矩阵乘法性能；

&emsp;&emsp;2. 基于AVX指令设计实现高性能矩阵乘法计算内核；

&emsp;&emsp;3. 基于OpenMP库实现任意形状的矩阵乘法计算。


