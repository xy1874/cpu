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

&emsp;&emsp;在Logisim电路仿真软件File菜单栏中，点击“open”，选择“RISCV-SoC.circ”，如图2-4所示。RISCV-SoC是RISC-V单周期CPU、指令存储器、数据存储器及外设构成的SOC电路。

<center><img src = "../assets/2-4.png"></center>
<center>图2-4 RISCV-SoC顶层电路图</center>

&emsp;&emsp;RISCV-SoC电路中的层次关系如图2-5所示。

<center><img src = "../assets/2-5.png" width = 320></center>
<center>图2-5 RISCV-SoC层次关系图</center>

&emsp;&emsp;本实验使用的RISCV-SoC支持如表2-1所示的24条RISC-V指令。

<center>表2-1 RISCV-SoC支持指令</center>
<center><img src = "../assets/t2-1.png"></center>


### 2.2 外设地址空间

&emsp;&emsp;本实验采用I/O统一编制的方式，即将整个32位地址空间分成若干部分，其中高4KB用作I/O地址空间，如图2-6所示。

<center><img src = "../assets/2-6.png" width = 180></center>
<center>图2-6 地址空间分配</center>

### 2.3 程序测试

&emsp;&emsp;在Logisim中打开RISCV-SoC的IMEM子电路，在指令存储器上点击右键，选择“清空内容”，并再次选择“编辑内容”，如图2-7所示。

<center><img src = "../assets/2-7.png" width = 280></center>
<center>图2-7 清空指令存储器</center>

&emsp;&emsp;然后，将刚才在RARS汇编软件汇编出来的“**.hex” (.text) 文件的内容粘贴到编辑器中，即将测试程序的汇编完成的指令存放到指令存储器中。（注：从0地址开始）。

&emsp;&emsp;如果汇编程序中有存放在数据段（.data）的数据，也需要拷贝到数据存储器中。方法与指令存储器拷贝数据是一样的。在Logisim中打开RISCV-SoC的DMEM子电路，根据汇编程序数据的存储位置将“**.hex” (.data) 的内容拷贝到RAM/ROM中。RAM和ROM的主要区别是，当在Logisim中按`Ctrl + R`复位时，RAM中的数据会被清空，而ROM中的数据不会被清空。

&emsp;&emsp;使用快捷键`Ctrl + K`进入连续时钟执行模式，再次`Ctrl + K`则暂停执行；使用快捷键`Ctrl + T`进入单步时钟执行模式，此模式可方便地查看程序的执行过程；使用快捷键`Ctrl + R`复位执行。程序执行时，电路的信号将实时发生变化，如图2-8所示。

<center><img src = "../assets/2-8.png"></center>
<center>图2-8 程序执行</center>

&emsp;&emsp;为了使CPU运行频率更快，建议在菜单栏Simulate -> Tick Frequency中修改时钟频率为4.1KHz，如图2-9所示。

<center><img src = "../assets/2-9.png"></center>
<center>图2-9 修改时钟频率</center>
