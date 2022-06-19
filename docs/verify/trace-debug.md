&emsp;&emsp;在CPU下板测试中，如果数码管显示卡在某一个功能点，没有继续计数，一种方法是可以采用Trace来进一步定位错误点。

- ***Step1***：将[download_test.zip](https://gitee.com/hitsz-cslab/cpu/blob/master/stupkt/download_test.zip)中的`start.bin`上传到Trace虚拟机的`cdp-test/bin`目录中；

- ***Step2***：找到`cdp-test/golden_model/include`目录下的cpu.h文件，将`#define MEM_PADDR_BITS 10`改成`#define MEM_PADDR_BITS 16`；

- ***Step3***：找到`cdp-test/golden_model`目录下的emu.c文件，将`uint32_t memory[4096];`改成`uint32_t memory[4096 * 16];`；

- ***Step4***：将你自己的Verilog代码拷贝到`cdp-test/mycpu`目录；

!!! Ps
    **Trace测试时，DRAM地址不需要减去0x4000；上板测试时，DRAM地址才需要减去0x4000。**

    ``` Verilog
    wire [31:0] waddr_tmp = waddr;// - 16'h4000;

    data_mem dmem (
        .clk    (cpu_clk),
        .a      (waddr_tmp[15:2]),
        .spo    (rdata),
        .we     (we),
        .d      (wdata)
    );
    ```

- ***Step5***：进入`cdp-test`目录，输入`make`命令以重新编译；

- ***Step6***：输入`make run TEST=start`以运行start测试。

&emsp;&emsp;例如，某次测试时报错，`debug_wb_pc`显示了`0x000018f8`，如下图所示。

<center><img src = "../assets/t-1.png" width = 400></center>

&emsp;&emsp;打开start.dump文件，找到PC值为`0x000018f8`的指令，即可定位到具体出错的指令，如下图所示。

<center><img src = "../assets/t-2.png" width = 650></center>

&emsp;&emsp;显然，在上述例子中，mycpu执行到auipc指令时出错。
