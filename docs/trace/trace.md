&emsp;&emsp;测试环境及相关工具下载地址：

- 虚拟机镜像：[vm.ova - 飞书文档 (feishu.cn)](https://otuyernchr.feishu.cn/file/boxcnrClUmwkmCZz5JbPsf7QnCb)

- MobaXTerm和VirtualBox：[虚拟机工具.zip - 飞书文档 (feishu.cn)](https://otuyernchr.feishu.cn/file/boxcnTjEReejhlrl0DlCte1Lrrf)

&emsp;&emsp;下载后，请参照本页后的“附录”导入虚拟机。

## 1. 了解测试框架

&emsp;&emsp;请在终端下输入命令：

``` bash
cd ~
git clone https://github.com/HITSZ-CDP-SU2021/cdp-tests.git
```

&emsp;&emsp;拉取测试框架代码。

```
.
├── bin（指令测试用例，用于初始化内存）
│   ├── add.bin
│   ├── .......
│   ├── xor.bin
│   └── xori.bin
├── asm（指令测试用例的反汇编文件，用于调试）
│   ├── add.dump
│   ├── .......
│   ├── xor.dump
│   └── xori.dump
├── csrc（测试驱动框架，包括比对逻辑）
│   ├── dut.h
│   └── test.cpp
├── golden_model（参考RISC-V CPU行为模型，使用C语言编写）
│   ├── emu.c
│   ├── include/...
│   └── stage/...
├── waveform【运行测试后生成】（运行测试生成的波形文件，用于调试）
│   ├── add.vcd
│   ├── ........
│   └── xori.vcd
├── Makefile
├── mycpu（你实现的CPU的Verilog代码，放在此目录下，仅放Verilog代码，不拷贝IP核）
│   ├── cpu.v
|   ├── .....
│   └── top.v
├── vsrc（仿真需要用到的其他文件）
|    └── ram.v（distributed memory的行为仿真模型）
├── run_all_tests.py（自动化测试脚本）
└── Makefile
```

&emsp;&emsp;我们测试的原理是差分测试，即比对标准模型和待测模型之间的区别。在实验中，标准模型就是`golden_model`下使用C语言实现的一个RISC-V CPU模型，而待测模块就是你所实现的CPU。驱动测试的代码逻辑位于`csrc`文件夹中，分别让标准模型和待测模型执行同一条指令，比对他们执行的结果，来确定你的CPU是否实现正确。**你只需要关注`mycpu`目录，暂时不需要关注其他目录，你需要将自己实现的CPU的Verilog代码粘贴到这个目录下。**



## 2. 添加自己的代码

&emsp;&emsp;`mycpu`目录中包含了你实现的CPU，以及顶层模块对外的连线，你需要将自己实现的CPU代码复制到该目录下，模块的层次结构如下图所示：

<center><img src = "../assets/trace-1.png" width = 500></center>

&emsp;&emsp;需要注意的是：

- 在之前的实验中，我们是使用IP核来实现的指令和数据存储器，而在此处，我们已经为你提供了相应的IP，你无需将IP核的xci文件拷贝至目录下，只需在`top`中实例化并完成连线即可。

- **你需要保证在`mycpu`目录下的模块的层次关系中，顶层模块名叫做`top`，且`top`模块的接口信号命名满足要求。**

``` Verilog
module top(
    input clk,
    input rst_n,
    output        debug_wb_have_inst,   // WB阶段是否有指令 (对单周期CPU，此flag恒为1)
    output [31:0] debug_wb_pc,          // WB阶段的PC (若wb_have_inst=0，此项可为任意值)
    output        debug_wb_ena,         // WB阶段的寄存器写使能 (若wb_have_inst=0，此项可为任意值)
    output [4:0]  debug_wb_reg,         // WB阶段写入的寄存器号 (若wb_ena或wb_have_inst=0，此项可为任意值)
    output [31:0] debug_wb_value        // WB阶段写入寄存器的值 (若wb_ena或wb_have_inst=0，此项可为任意值)
);

mini_rv mini_rv_u (
    .clk(clk),
    .rst_n(rst_n),
    //......
);
// 下面两个模块，只需要实例化并连线，不需要添加文件
inst_mem imem(

);

data_mem dmem(

);
endmodule
```

!!! warning
    （1）对于Trace比对，需要将指令ROM和数据RAM的大小设置为 **32bit*65536**！

    （2）指令ROM的模块名必须是 **inst_mem**，数据RAM的模块名必须是 **data_mem**！
    
    （3）CPU复位后执行的首条指令的地址必须是 **0x0000_0000**！  
    
    **以上设置错误会导致Trace比对失败！**



## 3. 运行测试

&emsp;&emsp;进入文件夹，输入命令：

``` bash
make
```

&emsp;&emsp;将会编译你的Verilog代码，生成可执行的仿真模型。

![image-20210704012833242](assets/trace-2.png)

### 3.1 运行单个测试

&emsp;&emsp;如果你对你CPU没有充足的信心，你可以选择单个测试运行，所有的测试用例都位于`bin`文件夹下，输入命令

``` bash
ls bin
```

&emsp;&emsp;可以看到测试点名称。

&emsp;&emsp;例如，我们想运行`sltu`指令的测试，我们输入：

``` bash
make run TEST=sltu
```

![image-20210704013056368](assets/trace-3.png)

&emsp;&emsp;打印出Test Point Pass之后，就代表这条指令测试通过了。

&emsp;&emsp;如果发生了错误，就会打印如下所示的信息：

![image-20210704013210584](assets/trace-4.png)

&emsp;&emsp;左栏为参照的正确实现，右栏为你实现的CPU给出的信号，通过比对这两组信号，你可以很快地确定错误发生在哪一条指令执行过程中，然后通过反汇编和波形的形式进行调试。

### 3.2 打开波形

&emsp;&emsp;在执行完某个测试之后，所生成的波形会在`waveform`文件夹中，如果要查看`auipc`测试点的波形，进入`waveform`文件夹，输入命令

``` bash
gtkwave auipc.vcd > /dev/null 2>&1 &  
```

&emsp;&emsp;弹出波形窗口。（`auipc`替换成需要打开的文件名）

![image-20210704014403857](assets/trace-5.png)

### 3.3 查看反汇编

&emsp;&emsp;反汇编文件在`asm`文件夹中，在上述例子中，我们看到是在PC=8处出现了错误，写回的值不对，而观察`auipc.dump`文件，可以找到出错点。

![image-20210704013720885](assets/trace-6.png)

&emsp;&emsp;根据出错点，结合波形，我们可以进行高效的调试了。

### 3.4 批量运行测试

&emsp;&emsp;如果你对你的实现有足够的信心，直接使用命令进行自动化测试。

``` bash
python3 run_all_tests.py
```

!!! Warning
    &emsp;&emsp;执行这条命令之前，需要保证之前已经`make`过了。

&emsp;&emsp;在很长的滚屏之后，会出现一个测试报告，告诉你哪些测试通过，哪些测试不通过。所有测试点的波形文件都在`waveform`文件夹中，你可以打开对应的文件结合反汇编和在SUMMARY之前输出的报错信息进行调试。

![image-20210704014045871](assets/trace-7.png)

![image-20210704013913901](assets/trace-8.png)
