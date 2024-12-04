# 练习4：学习性能分析工具Perf的基本使用

- **任务**：通过`perf`观察程序性能。

- **子任务**：
    - 通过`perf list`命令查看支持的性能事件；  
    - 通过`perf stat`查看练习2的矩阵乘法程序的缓存使用情况。

- **目标**：学会`perf`的基本使用。

!!! info "关于实验环境 :computer:"
    &emsp;&emsp;实验室的Ubuntu双系统已提前配置好环境，进行实验时可略过安装环境的步骤。

#### 1. 命令安装`perf`

``` bash linenums="1"
sudo apt install linux-tools-5.4.0-26-generic
```

#### 2. 查看`perf`支持的性能事件

``` bash linenums="1"
perf list
```

<center><img src="../assets/3-4-1.png" width = 480></center>

#### 3. 查看`lab1_gemm`程序的缓存使用情况

<center><img src="../assets/3-4-2.png" width = 100%></center>

``` bash linenums="1"
# 查看基本性能事件
perf stat ./dist/bins/lab1_gemm 256 256 256
# 查看指定的性能事件(-e)，支持的性能事件可由perf list查看
perf stat -e L1-dcache-loads,L1-dcache-load-misses,dTLB-loads,dTLB-load-misses ./lab1_gemm 256 256 256
```

<center><img src="../assets/3-4-3.png" width = 100%></center>
