## 1. RISC-V程序汇编与RARS模拟器

### 1.1 RARS简介

&emsp;&emsp;RARS (RISC-V Assembler and Runtime Simulator) 是一个轻量级的交互式集成开发环境，用于使用RISC-V汇编语言进行编程，具有代码提示，模拟运行，调试，统计等功能，其基本界面如图2-1所示。

<center><img src = "../assets/2-1.png" width = 500></center>
<center>图2-1 RARS基本界面</center>


### 1.2 运行方法

&emsp;&emsp;汇编代码编辑完成后，点击菜单栏的Run，选择Assemble即可进行汇编。也可以点击下图中用红色框框的图标进行汇编，如图2-2所示。

<center><img src = "../assets/2-2.png"></center>
<center>图2-2 RARS使用方法</center>


### 1.3 导出机器码

&emsp;&emsp;程序汇编后可以利用File菜单中的Dump Memory功能将代码段和数据段导出，采用十六进制文本（Hexadecimal Text）的方式导出到 “**.hex”，即可在LOGISIM中加载到RAM或ROM中，如图2-3所示。

<center><img src = "../assets/2-3a.png"></center>
<center>a) 从菜单导出机器码</center>

<center><img src = "../assets/2-3b.png" width = 450></center>
<center>b) 选择导出格式</center>
<center>图2-3 导出机器码</center>

!!! warning "注意 :gun:"
    &emsp;&emsp;.text是存储在指令存储器中，.data是存储在数据存储器中，需要分别存放到两个“**.hex”文件中。（注：如果汇编代码中没有定义.data，则不会生成.data段）



## 2. 在Logisim上运行程序

### 2.1 RISC-V SOC电路

&emsp;&emsp;在Logisim电路仿真软件File菜单栏中，点击“Open”，选择“RISC-V_SoC.circ”，如图2-4所示。RISC-V SoC是RISC-V单周期CPU、指令存储器、数据存储器及外设构成的SoC电路。

<center><img src = "../assets/2-4.png"></center>
<center>图2-4 RISC-V SoC顶层电路图</center>

&emsp;&emsp;RISC-V SoC电路中的层次关系如图2-5所示。

<center><img src = "../assets/2-5.png" width = 320></center>
<center>图2-5 RISC-V SoC层次关系图</center>

&emsp;&emsp;本实验使用的RISC-V SoC支持如表2-1所示的24条RISC-V指令。

<center>表2-1 RISC-V SoC支持指令</center>
<center><img src = "../assets/t2-1.png"></center>

### 2.2 外设编址方式

&emsp;&emsp;在本课程中，存储器和外设统一编址，即存储器和外设共用整个32位地址空间，其中最高的4KB (`0xFFFFF000` ~ `0xFFFFFFFF`) 用作I/O地址空间。

### 2.3 程序测试

&emsp;&emsp;在Logisim中双击打开RISC-V SoC的`IMEM`子电路，在指令存储器上点击右键选择`Clear Contents`，然后再次右键选择`Edit Contents`，如图2-6所示。

<center><img src = "../assets/2-6.png" width = 280></center>
<center>图2-6 清空指令存储器</center>

&emsp;&emsp;然后，将刚才在RARS汇编软件汇编出来的`xxx.hex`（`.text`）文件的内容粘贴到编辑器中，即将测试程序的汇编完成的指令存放到指令存储器中（注：从0地址开始）。

&emsp;&emsp;如果汇编程序中有存放在数据段（`.data`）的数据，也需要拷贝到数据存储器中。方法与指令存储器拷贝数据是一样的。在Logisim中打开RISC-V SoC的`DMEM`子电路，根据汇编程序数据的存储位置将`xxx.hex`（`.data`）的内容拷贝到RAM/ROM中。RAM和ROM的主要区别是，当在Logisim中按 ++ctrl+r++ 复位时，RAM中的数据会被清空，而ROM中的数据不会被清空。

&emsp;&emsp;使用快捷键 ++ctrl+k++ 进入连续时钟执行模式，再次 ++ctrl+k++ 则暂停执行；使用快捷键 ++ctrl+t++ 进入单步时钟执行模式，此模式可方便地查看程序的执行过程；使用快捷键 ++ctrl+r++ 复位执行。程序执行时，电路的信号将实时发生变化，如图2-7所示。

<center><img src = "../assets/2-7.png"></center>
<center>图2-7 程序执行</center>

&emsp;&emsp;为了使CPU运行频率更快，建议在菜单栏`Simulate`->`Tick Frequency`中修改时钟频率为4.1KHz，如图2-8所示。

<center><img src = "../assets/2-8.png" width = 450></center>
<center>图2-8 修改时钟频率</center>
