# 实验2：使用Cache优化矩阵乘法

## 实验目的

&emsp;&emsp;1. 理解计算机Cache原理，并掌握使用该原理优化矩阵乘法运算；

&emsp;&emsp;2. 通过合理利用Cache优化矩阵乘法计算的数据访问路径，学习和应用指令级优化的一般步骤；

&emsp;&emsp;3. 了解分析程序性能瓶颈和从指令层面对程序进行优化的基本方法。



## 实验前准备

&emsp;&emsp;阅读下述论文材料以了解设计高性能矩阵乘法计算内核的基本原则。

- 《Anatomy of High-Performance Matrix Multiplication》  
- 《Analytical modeling is enough for high-performance BLIS》

&emsp;&emsp;实验环境：一台安装了`cmake`、`gcc`、`gdb`和`perf`环境且系统为ubuntu x64的计算机。



## 实验内容

&emsp;&emsp;1. 利用性能分析工具分析性能瓶颈；

&emsp;&emsp;2. 利用数据主动预取机制优化矩阵乘法的性能；

&emsp;&emsp;3. 利用循环和分块提升矩阵乘法的计算性能。


