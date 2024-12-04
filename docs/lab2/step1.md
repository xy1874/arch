# 练习1：利用性能分析工具分析性能瓶颈

- **任务**：从访存层面分析示例代码的性能瓶颈。

- **目标**：学会利用perf性能分析工具分析示例程序的缓存命中率。

- **示例代码**：`lab2_gemm_baseline.S`

#### 0. 下载并解压实验包

&emsp;&emsp;从<a href="../../#1" target=_blank>首页链接</a>下载`lab2.tar.gz`，将其拷贝至用户目录，并执行命令`tar -zxvf lab2.tar.gz`解压实验包。

#### 1. 编译生成待分析的二进制程序`lab2_gemm_baseline`

``` bash linenums="1"
# 项目根目录执行
mkdir -p build && cd build
cmake -B . -S ../ && cmake --build ./ --target lab2_gemm_baseline
```

#### 2. 使用`perf stat`命令结合`-e`参数指定事件查看程序的性能事件并根据结果分析定位性能瓶颈点(指定的事件应随分析而变)

``` bash linenums="1"
perf stat -e l2_rqsts.code_rd_hit,l2_rqsts.references,l1d.replacement,l1d_pend_miss.pending,l2_rqsts.pf_hit,l2_rqsts.pf_miss,L1-dcache-loads,L1-dcache-load-misses ./dist/bins/lab2_gemm_baseline 256 1024 256
```

&emsp;&emsp;由于perf版本原因，支持的事件可能有差异，可在`perf list`得到的事件表中自行选取需要的事件。

<center><img src="../assets/3-1-1.png" width = 100%></center>


