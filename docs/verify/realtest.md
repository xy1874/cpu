## 1. RTL修改

### 1.1 DRAM访存地址修改

&emsp;&emsp;用于下板测试的汇编程序采用IROM和DRAM统一编址，因此需要对原DRAM的访存地址进行修改：

&emsp;&emsp;原DRAM代码：

``` Verilog
dram U_dram (
    .clk    (cpu_clk),
    .a      (waddr[15:2]),
    .spo    (rdata),
    .we     (we),
    .d      (wdata)
);
```

&emsp;&emsp;修改后的DRAM代码：

``` Verilog
wire [31:0] waddr_tmp = waddr - 16'h4000;

dram U_dram (
    .clk    (cpu_clk),
    .a      (waddr_tmp[15:2]),
    .spo    (rdata),
    .we     (we),
    .d      (wdata)
);
```

### 1.2 数码管显示修改

&emsp;&emsp;原本数码管显示PC，现修改为显示x19寄存器的值。



## 2. 存储器IP替换

&emsp;&emsp;由于下板测试程序较大，导致导入.coe后重新综合工程耗时较长，因此课程指导书网站提供了已综合好的IROM和DRAM IP核，同学们可将其直接导入CPU工程中使用。

&emsp;&emsp;下面以导入DRAM为例，介绍导入已综合IP核的方法 (IROM同理)。

&emsp;&emsp;首先，从指导书网站[下载download_test.zip压缩包](https://gitee.com/hitsz-cslab/organ/blob/2021-Summer/stupkt/download_test.zip)文件并解压到无中文路径下。

&emsp;&emsp;然后，备份原工程，按照上一节所述修改RTL代码，并删除已实例化的IROM和DRAM IP核。

&emsp;&emsp;接着，按下图所示点击添加IP核。

<center><img src = "../assets/1.png" width = 450></center>

<center><img src = "../assets/2.png" width></center>

<center><img src = "../assets/3.png" width = 600></center>
<center>从解压的目录中添加已综合的DRAM IP核</center>



## 3. 顶层模块端口定义

``` Verilog
module top (
    input  wire clk_i,
    input  wire rst_i,
    output wire led0_en_o,
    output wire led1_en_o,
    output wire led2_en_o,
    output wire led3_en_o,
    output wire led4_en_o,
    output wire led5_en_o,
    output wire led6_en_o,
    output wire led7_en_o,
    output wire led_ca_o,
    output wire led_cb_o,
    output wire led_cc_o,
    output wire led_cd_o,
    output wire led_ce_o,
    output wire led_cf_o,
    output wire led_cg_o,
    output wire led_dp_o
);

wire rst_n = !rst_i;
```

!!! warning "注意 :gun:"
    &emsp;&emsp;板上的复位信号（按键开关`S6`，即`P20`引脚）默认输出低电平，按下时输出高电平，因此`rst_i`是高电平复位。


## 4. 下板结果说明

&emsp;&emsp;每通过一个测试点，数码管显示的数值会加1，直至测试完毕。测试完毕时，数码管将以16进制形式显示通过测试的指令数 (例如，如果实现了单周期的35条指令并通过了测试，则数码管将显示0x23)。

&emsp;&emsp;需要注意的是，对于测试不通过的情形，如果数码管显示的值停在n，则表示第n+1条指令的测试失败。测试失败时，可查看start.dump的测试代码以定位出错指令，并进行相应的调试。
