# 练习3：获取目标处理器的缓存信息

- **任务**：通过操作系统提供的命令、文件和评估工具获取系统目标处理器的缓存信息。

- **子任务**：
    - 获取目标处理器缓存层级和各级缓存大小信息；  
    - 获取目标处理器各级缓存的组相联数、缓存行大小信息。

- **目标**：学会获取未知架构的处理器的缓存信息。

#### 1. 通过`lscpu`命令查看处理器型号、缓存层级信息
 
<center><img src="../assets/3-3-1.png" width = 100%></center>

#### 2. 查看cpu0的L1D缓存的组数、组相联数和缓存行大小

``` bash linenums="1"
cd /sys/devices/system/cpu/cpu0/cache
cd index0
# 查看缓存行大小
cat coherency_line_size
# 查看组数
cat number_of_sets
# 查看way数
cat ways_of_associativity
```

<center><img src="../assets/3-3-2.png" width = 100%></center>
 
#### 3. 使用相同的方法查看L2, L3缓存的组数、组相联数和缓存行大小
