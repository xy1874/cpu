## 1. 实验目的

&emsp;&emsp;（1）学习RARS、Logisim的使用；

&emsp;&emsp;（2）学习RISC-V汇编语言，熟悉并理解RISC-V指令系统；

&emsp;&emsp;（3）了解程序在单周期RISC-V CPU搭建的SoC中的运行。



## 2. 实验内容

### 题目1：基本输入输出

&emsp;&emsp;在提供的Logisim环境中，运行提供的程序<font color = blue>**Exercise1.asm**</font>，并阅读该程序源码，分析程序功能，学习汇编程序如何访问I/O接口及外设。

&emsp;&emsp;数码管、LED、拨码开关的端口地址如表1-1所示。

<center>表1-1 数码管、LED、拨码开关端口地址</center>
<center>

| 接口部件 | 基地址 |
| :-: | :-: |
| 数码管 | 0xFFFF_F000 |
| LED | 0xFFFF_F060 |
| 拨码开关 | 0xFFFF_F070 |

</center>

&emsp;&emsp;根据实验步骤运行程序，熟悉Logisim工具及操作流程。

### 题目2：简易计算器设计

&emsp;&emsp;运用miniRV-1指令集的24条必做指令，编写汇编程序，实现一个8位的简易计算器。输入的操作数A、B均为8bit有符号整数。

!!! warning "注意 :gun:"
    &emsp;&emsp;禁止使用除miniRV-1指令集的24条必做指令以外的其他指令。

&emsp;&emsp;计算器的运算符和操作数A、B均通过拨码开关输入，如表1-1所示。

<center>表1-1 8位简易计算器的输入定义</center>
<center>

| 拨码开关 | SW[23:21] | SW[20:16] | SW[15:8] | SW[7:0] |
| :-: | :-: | :-: | :-: | :-: |
| 输入功能 | 运算类型选择 | 保留 | 操作数B | 操作数A |

</center>

&emsp;&emsp;其中，`SW[23:21]`的值与计算器运算类型的对应关系如表1-2所示。

<center>表1-2 运算类型定义</center>
<center>

| SW[23:21] | 运算类型 |
| :-: | :-: |
| 3'b000 | 无 |
| 3'b001 | A + B |
| 3'b010 | A - B |
| 3'b011 | A & B |
| 3'b100 | A \| B |
| 3'b101 | A << B |
| 3'b110 | A >> B (算术右移) |
| 3'b111 | A · B |

</center>

&emsp;&emsp;计算器的运算结果显示到数码管DK7~DK0。

!!! info "提示 :bulb:"
    &emsp;&emsp;乘法可使用《计算机组成原理》课程中学过的原码一位乘或Booth算法来实现。
