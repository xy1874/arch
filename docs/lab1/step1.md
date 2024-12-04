# 练习1：环境搭建与初步测试

- **任务**：编写一个简单的程序打印一个整数，以验证汇编环境。

- **目标**：
    - 确保正确安装和配置工具库；  
    - 熟悉汇编语言的语法和编译过程。

!!! info "关于实验环境 :computer:"
    &emsp;&emsp;实验室的Ubuntu双系统已提前配置好环境，进行实验时可略过安装环境的步骤。

#### 0. 下载并解压实验包

&emsp;&emsp;从<a href="../../#1" target=_blank>首页链接</a>下载`lab1.tar.gz`，将其拷贝至用户目录，并执行命令`tar -zxvf lab1.tar.gz`解压实验包。

#### 1. 安装编译所需的基础环境

``` bash linenums="1"
sudo apt update
sudo apt install build-essential net-tools git vim cmake gdb make gfortran libnuma-dev libtirpc-dev
```

#### 2. 测试环境安装是否成功

``` bash linenums="1"
cmake –version
gcc –version
gdb --version
```

#### 3. 为`src/lab1/print_integer.S`源码中的`convert_loop`循环(源码第33行)添加正确的结束指令

<center><img src="../assets/3-1-1.png" width = 600></center>

#### 4. 修复`src/lab1/print_integer.S`源码中的`print_string`过程中(第48行-第55行)存在错误

<center><img src="../assets/3-1-2.png" width = 600></center>

#### 5. 进入项目根目录，编译代码

``` bash linenums="1"
cd lab1
mkdir -p build && cd build
cmake -B . -S ../ && cmake --build ./ --target lab1_print_integer
```

&emsp;&emsp;执行以上第3条命令前，请先确保计算机能正常访问互联网。

#### 6. 运行代码得到正确输出

``` bash linenums="1"
cd dist/bins/ && ./lab1_print_integer
```

<center><img src="../assets/3-1-3.png" width = 100%></center>
