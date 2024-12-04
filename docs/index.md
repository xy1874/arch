# 课程概况

&emsp;&emsp;本实验文档为哈尔滨工业大学（深圳）《计算机体系结构》课程实验指导材料。页面顶端为各个实验项目的指导书；页面左侧为指导书的各个小节目录，右侧为小节内的索引，页面右上角可详细搜索，页面下方可切换上下小节。请务必按顺序阅读指导书，有问题积极在群内提出。

!!! warning "声明 :loudspeaker:"
    &emsp;&emsp;本课程资料仅限哈尔滨工业大学（深圳）《计算机体系结构（实验）》2024秋季课程使用，严禁扩散或用作其他用途。



## 内容安排

&emsp;&emsp;本课程实验旨在使同学们掌握计算机体系结构中 **Cache优化**、**指令级优化** 和 **并行优化**。内容包括以下几个方面：

- 编写基础实验框架：包括使用汇编代码编写矩阵乘法和Llama推理框架以及Llama推理框架中矩阵乘法（或矩阵向量乘法）在推理时间的占比。
- 编写Cache优化汇编程序，首先理解处理器架构的Cache层次以及大小。其次理解矩阵乘法中数据读写对Cache的使用情况。最后根据数据与Cache特点，优化矩阵数据的读写模式，提高Cache利用率，提升矩阵乘法性能。
- 编写指令级并行CPU程序，理解指令级并行对矩阵乘法的加速。
- CUDA编程，包括基本的GPU矩阵乘法程序和共享内存的优化。
- Llama.c程序加速（开放实验）。

&emsp;&emsp;以上内容将编排为6个实验：

<center><table>
    <tr>
        <th align="center">专题</th>
        <th align="center">实验</th>
        <th align="center">学时</th>
    </tr>
    <tr>
        <td rowspan=4 style="vertical-align: middle;" align="center">CPU优化专题</td>
        <td align="left">实验1：编写矩阵乘法</td>
        <td align="center">4</td>
    </tr>
    <tr>
        <td align="left">实验2：使用Cache优化矩阵乘法</td>
        <td align="center">4</td>
    </tr>
    <tr>
        <td align="left">实验3：使用指令级并行、向量指令和并行优化矩阵乘法</td>
        <td align="center">8</td>
    </tr>
    <tr>
        <td align="left">:pencil: 撰写实验报告</td>
        <td align="center">-</td>
    </tr>
    <tr>
        <td rowspan=3 style="vertical-align: middle;" align="center">GPU优化专题</td>
        <td align="left">实验4：使用C/CUDA编写GPU代码</td>
        <td align="center">4</td>
    </tr>
    <tr>
        <td align="left">实验5：使用共享内存优化GPU代码</td>
        <td align="center">4</td>
    </tr>
    <tr>
        <td align="left">:pencil: 撰写实验报告</td>
        <td align="center">-</td>
    </tr>
    <tr>
        <td rowspan=2 style="vertical-align: middle;" align="center">Llama测试</td>
        <td align="left">实验6：Llama模型推理性能评测</td>
        <td align="center">4</td>
    </tr>
    <tr>
        <td align="left">:pencil: 撰写实验报告</td>
        <td align="center">-</td>
    </tr>
</table></center>



## 课表安排

&emsp;&emsp;本学期实验分3个班上课，<u>**上课地点均为T2210**</u>，各班课表如下表所示。<u>少数同学某一周的课表若存在课程或考试等冲突情况，可根据下表自行选择到其他班上课</u>。

<center>

| 周次 | 授课内容 | A班 | B班 | C班 |
| :-: | :-: | :-: | :-: | :-: |
| 11   | 实验1   | 周三 9-12节 | 周四 9-12节 | 周六 5-8节  |
| 12   | 实验2   | 周三 9-12节 | 周四 9-12节 | 周五 9-12节 |
| 13   | 实验3-1 | 周三 9-12节 | 周五 5-8节  | 周六 5-8节  |
| 14   | 实验3-2 | 周四 1-4节  | 周四 9-12节 | 周五 9-12节 |
| 15   | 实验4   | 周四 1-4节  | 周四 9-12节 | 周五 9-12节 |
| 16-1 | 实验5   | 周一 1-4节  | 周四 1-4节  | 周一 5-8节  |
| 16-2 | 实验6   | 周三 1-4节  | 周五 9-12节 | 周四 9-12节 |

</center>


## 其他

### 1. 实验包下载

&emsp;&emsp;<a href="http://10.249.10.96:3011/" target=_blank>校园网下载链接</a>

### 2. 作业提交方式

&emsp;&emsp;提交作业的方法和流程见页面左侧《作业提交说明》。

### 3. 关于实验室网络

&emsp;&emsp;T2210 Ubuntu双系统若不能上网，请按以下表格修改网络配置：

<center>

| IP | 10.251.137.座位号 |
| :-: | :-: |
| 掩码 | 255.255.255.0 |
| 网关 | 10.251.137.254 |
| DNS | 10.248.98.30 |

</center>


