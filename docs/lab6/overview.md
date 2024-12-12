# 实验6：Llama模型推理性能评测

## 实验目的

&emsp;&emsp;1. 学习综合运用所学的矩阵乘法优化手段，优化大模型推理性能；

&emsp;&emsp;2. 理解计算机体系结构相关原理和技术在实际应用中的作用。


## 实验内容

&emsp;&emsp;<a href="https://github.com/karpathy/llama2.c" target=_blank>Llama2</a> 是基于 Transformer 架构的轻量级开源自然语言处理模型，模型的推理和量化均使用单文件的 C 程序实现，且代码中含有丰富的注释，便于开发者进行移植、部署和优化。

&emsp;&emsp;本实验是开放性实验，同学们可在前面实验涉及到的矩阵乘法优化方法中自由选择和组合，将优化后的矩阵乘法算法集成到 Llama2 大模型中，实现 Llama2 推理性能的加速。具体内容包括：

&emsp;&emsp;1. 阅读 Llama2 推理程序的代码，了解其矩阵乘法的实现方式，并通过分析或调试得出其矩阵乘法运算的数据规模；

&emsp;&emsp;2. 选用一种优化手段或将多种优化手段相结合，提升 Llama2 的矩阵乘法运算性能；

&emsp;&emsp;3. 运行不同大小参数规模的 Llama2 模型，对比优化前后的推理性能并进行合理分析；

&emsp;&emsp;4. 若优化后的推理速度与优化前相比有较为稳定的提升效果，可获得一定加分，加分值根据优化效果判定。


## 实验步骤

#### 0. 下载并解压实验包

&emsp;&emsp;从 <a href="https://github.com/karpathy/llama2.c" target=_blank>Llama2官方仓库</a> 或 <a href="../../#1" target=_blank>首页链接</a>下载`llama2.c.tar.gz`和3个`.bin`文件。

&emsp;&emsp;将`llama2.c.tar.gz`拷贝至用户目录，执行命令`tar -zxvf llama2.c.tar.gz`解压实验包。

&emsp;&emsp;将3个`.bin`文件放置在解压后的`llama2.c/`目录下。

#### 1. 运行 Llama2 模型

&emsp;&emsp;打开终端，`cd`到`llama2.c/`目录后，执行`make run`命令编译代码，随后执行`./run stories15M.bin`以运行 Llama2 模型。

&emsp;&emsp;分别执行`./run stories42M.bin`和`./run stories110M.bin`，观察不同大小模型的运行时间差别。

#### 2. 查看矩阵乘法实现方式

&emsp;&emsp;Llama2 的模型推理实现在 `run.c` 文件中，矩阵乘法函数则在其中的 <a href="https://github.com/karpathy/llama2.c/blob/master/run.c#L217" target=_blank>第217行</a>。

<center><img src="../assets/3-1.png" width = 100%></center>

&emsp;&emsp;从注释可知，`matmul`函数实现的是 $d \times n$ 的矩阵 **$W$** 与维度为 $n$ 的列向量 **$x$** 的乘法，且模型推理的性能瓶颈就是矩阵乘法函数（感兴趣的同学们可自行验证）。

#### 3. 通过调试手段查看矩阵乘法的数据规模大小

&emsp;&emsp;通过在代码中添加`printf`语句，或使用 GDB 等调试工具，查看`matmul`函数的输入矩阵的尺寸参数。

#### 4. 自行选择恰当的优化方法优化`matmul`函数

&emsp;&emsp;修改前面实验实现的矩阵优化算法，并将其集成到`run.c`中。

&emsp;&emsp;修改代码后，应先编译代码再执行`./run xxx.bin`，编译时根据需要链接相应的库，具体可参考前面实验的代码。若使用 CUDA 优化，需执行`nvcc -O3 -o run run.c -lm`命令编译代码；若使用了 cuBLAS 库，则执行`nvcc -O3 -o run run.c -lm -L/usr/local/lib64 -lcublas`编译代码；若采用 AVX、OpenMP 等其他优化方法，则直接执行`make run`即可。

!!! tip "关于使用 CUDA 编译时可能出现的问题"
    &emsp;&emsp;若确认代码无语法错误，但执行`nvcc -O3 -o run run.c -lm`命令进行编译时，提示`dim3`、`<<<`等 CUDA 特有的语法和语句存在错误，如下图所示。

    <center><img src="../assets/3-2.png" width = 100%></center>

    &emsp;&emsp;此时，执行`cp run.c run.cu`为源文件创建后缀为`.cu`的副本，再执行`nvcc -O3 -o run run.cu -lm`命令进行编译。编译时可能出现如下图所示的错误提示。

    <center><img src="../assets/3-3.png" width = 100%></center>

    &emsp;&emsp;只需根据报错信息，找到对应语句，添加强制类型转换即可。比如将`run.cu`第80行的语句改成：

    ``` c title="llama2.c / run.cu" linenums="80"
        s->x = (float*)calloc(p->dim, sizeof(float));
    ```

    &emsp;&emsp;修改完成后，再次执行`nvcc -O3 -o run run.cu -lm`进行编译即可。

#### 5. 对比分析优化前后的推理性能

&emsp;&emsp;分别使用3个`.bin`文件对优化前后的 Llama2 模型各自运行3~5次，记录推理速度平均值，与优化前的运行效果进行对比，尝试对结果进行分析和优化。

&emsp;&emsp;下图是使用 CUDA 优化的示例。

<center><img src="../assets/3-4.png" width = 100%></center>


## 实验报告要求

&emsp;&emsp;按以下要求撰写实验报告。实验报告无模板，只需按要求写好相关内容即可。

1. 实现流程（结合图表以文字形式表述）；

2. 测试结果及原理分析（结合图表、文字、源码，对比分析优化前后的运行效果并描述所采用优化措施的原理）。

&emsp;&emsp;提交时，将 **报告转成pdf格式**，单独提交到作业系统。

&emsp;&emsp;此外，将实验过程中 **修改过的文件压缩成.zip压缩包**，单独提交到作业系统。
