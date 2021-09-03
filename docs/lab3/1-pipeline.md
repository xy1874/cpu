&emsp;&emsp;流水线（pipeline）技术是指在程序执行时多条指令重叠进行操作的一种准并行处理实现技术。流水线是Intel首次在486芯片中开始使用的。在CPU中由多个不同功能的电路单元组成一条指令处理流水线，然后将一条指令分成多步后再由这些电路单元分别执行，这样就能实现在一个CPU时钟周期完成一条指令，因此提高CPU的运算速度。

&emsp;&emsp;理想条件下，流水线处理器的指令执行时间可以用以下公式计算：

&emsp;&emsp;设：

- $T_{non-Pipeline}$：非流水线指令执行时间

- $T_{Pipeline}$：流水线指令执行时间

- $N$：流水线级数

&emsp;&emsp;则有：

$$T_{Pipeline} = \frac{T_{non-Pipeline}}{N}$$

## 1. 五级流水线

&emsp;&emsp;将单周期CPU划分为如图3-1的五级流水线：

<center><img src = "../assets/3-1.png" width = 600></center>
<center>图3-1 五级流水线</center>

- 取指：（Instruction Fetch）是指将指令从存储器中读取出来的过程。

- 译码：（Instruction Decode）是指将存储器中取出的指令进行翻译的过程。经过译码之后得到指令需要的操作数寄存器索引，可以使用此索引从通用寄存器组（Register File，Regfile）中将操作数读出。

- 执行：（Instruction Execute）是指对指令进行真正运算的过程。譬如，如果指令是一条加法运算指令，则对操作数进行加法操作；如果是减法运算指令，则进行减法操作。在“执行”阶段的最常见部件为算术逻辑部件运算器（Arithmetic Logical Unit，ALU），作为实施具体运算的硬件功能单元。

- 访存：（Memory Access）是指存储器访问指令将数据从存储器中读出，或者写入存储器的过程。

- 写回：（Write-Back）是指将指令执行的结果写回通用寄存器组的过程。如果是普通运算指令，该结果值来自于“执行”阶段计算的结果；如果是存储器读指令，该结果来自于“访存”阶段从存储器中读取出来的数据。

## 2. 流水寄存器

&emsp;&emsp;如图3-2在流水级之间设置流水寄存器。

&emsp;&emsp;作用：

&emsp;&emsp;1）保存该流水级输出数据信息，交给下一级处理，并共享给其他指令。

&emsp;&emsp;2）切割组合逻辑，提升系统频率

<center><img src = "../assets/3-2.png"></center>
<center>图3-2 流水线寄存器</center>

### 2.1 IF/ID流水寄存器

&emsp;&emsp;传递当前指令以及PC值等，如图3-3所示：

<center><img src = "../assets/3-3.png" width = 110></center>
<center>图3-3 IF/ID流水寄存器</center>

- RTL实现：

<center><img src = "../assets/rtl1.png" width = 450></center>

### 2.2 ID/EX流水寄存器

&emsp;&emsp;传递当前指令操作类型，立即数，操作数等，如图3-4所示；

<center><img src = "../assets/3-4.png" width = 120></center>
<center>图3-4 ID/EX流水寄存器</center>

### 2.3 EX/MEM流水寄存器

&emsp;&emsp;传递当前指令访存操作以及计算结果等，如图3-5所示；

<center><img src = "../assets/3-5.png" width = 120></center>
<center>图3-5 EX/MEM流水寄存器</center>

### 2.4 MEM/WB流水寄存器

&emsp;&emsp;传递当前指令的写回结果，如图3-6所示；

<center><img src = "../assets/3-6.png" width = 95></center>
<center>图3-6 MEM/WB流水寄存器</center>


