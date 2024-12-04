# 练习2：利用预取优化性能

- **任务**：通过为练习1代码增加数据主动预取机制提升矩阵乘法性能。

- **子任务**：
    - 利用<a href="https://sourceforge.net/p/lmbench/discussion/45564/thread/a8bb62f40d/?limit=25#6283/9602" target=_blank>lmbench3</a>获取各级缓存的访问延迟；  
    - 利用访问延迟信息为预取指令选择合适的位置。

- **目标**：
    - 学会查阅处理器生产商提供的<a href="https://www.intel.cn/content/www/cn/zh/content-details/782158/intel-64-and-ia-32-architectures-software-developer-s-manual-combined-volumes-1-2a-2b-2c-2d-3a-3b-3c-3d-and-4.html" target=_blank>软件开发手册</a>选取合适的指令（10.4.6节数据预取相关内容）；  
    - 学会在指令流中为数据预取指令选择合适的位置。

#### 1. 运行`lmbench3`工具查看目标处理器信息

&emsp;&emsp;注意：需多次运行取均值，单次运行时间约7小时，请合理安排重试次数。

``` bash linenums="1"
cd tools && tar xf lmbench.tgz && cd lmbench
make results
# 涉及的参数选择 
# MULTIPLE COPIES [default 1] 回车
# Job placement selection: 4
# MB [default 10956] 回车
# SUBSET (ALL|HARWARE|OS|DEVELOPMENT) [default all] HARWARE
# FASTMEM [default no] 回车
# SLOWFS [default no] yes
# DISKS [default none] 回车
# REMOTE [default none] 回车
# Processor mhz [default 2688 MHz, 0.3720 nanosec clock] 回车
# FSDIR [default /var/tmp] 回车
# Status output file [default /dev/tty] /tmp/lmbench.test
# Mail results [default yes] no

# 因为LMBENCH3对新系统的兼容问题，选择完后很快会失败，按下面的步骤继续操作即可
cp -a results ./bin/
make rerun

# 等待运行结束
mv results results.bak
cp -a ./bin/results ./
# 查看延迟信息（如果不够详细可以自行调整make results参数再进行测试）
cd results && make LIST=$(../scripts/os)/*
```

<center><img src="../assets/3-2-1.png" width = 100%></center>

&emsp;&emsp;在`tools目录`下可以找到“`10.251.137.194.0`”文件，这是`make results`指令的输出文件，根据上述的指令将其放入正确位置并执行`make rerun`之后的步骤即可。
 

#### 2. 根据上一步获得的存储信息，为练习1的代码增加数据预取优化（如`prefetch0`指令），形成`src/lab2/gemm_kernel_opt_prefetch.S`中`DO_GEMM`过程（第64行）中的矩阵计算逻辑

&emsp;&emsp;注意：不需自行编写下图中的`DO_GEMM`过程，只需先将`gemm_kernel_baseline.S`中第62\~105行`DO_GEMM`的代码替换掉`gemm_kernel_opt_prefetch.S`中的第63\~65行，然后在此基础上添加预取优化代码即可。

<center><img src="../assets/3-2-2.png" width = 480></center>

#### 3. 验证kernel结果的正确性

``` bash linenums="1"
# 项目根目录下执行
mkdir -p build && cd build
cmake -B . -S ../ && cmake --build ./ --target lab2_gemm_kernel_opt_prefetch.unittest

cd dist/bins && ./lab2_gemm_kernel_opt_prefetch.unittest
```

<center><img src="../assets/3-2-3.png" width = 550></center>

&emsp;&emsp;若编译时提示.cpp文件报错，则点击下载<a href="http://10.249.10.96:3011/gemm_kernel_opt_prefetch.cpp" target=_blank>gemm_kernel_opt_prefetch.cpp</a>和<a href="http://10.249.10.96:3011/gemm_kernel_opt_loop.cpp" target=_blank>gemm_kernel_opt_loop.cpp</a>，将其拷贝并替换掉`src/lab2/`目录下的原有文件，再重新编译。

#### 4. 对比优化后的算法与基线的性能（测试用例根据自己的优化选择），根据对比结果分析优化方案的局限性

``` bash linenums="1"
mkdir -p build && cd build
cmake -B . -S ../ && cmake --build ./ --target lab2_gemm_opt_prefetch
cd dist/bins && ./lab2_gemm_opt_prefetch 1024 128 4
```

<center><img src="../assets/3-2-4.png" width = 550></center>
