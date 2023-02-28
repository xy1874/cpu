## 1. RTL修改

### 1.1 DRAM访存地址修改

&emsp;&emsp;本实验提供可在FPGA上运行Trace比对的汇编测试程序，该程序的源码见测试包的<a href="https://github.com/HITSZ-CDP/cdp-tests/blob/peri/asm/start.dump" target="_blank">`cdp-tests`/`asm`/`start.dump`</a>

&emsp;&emsp;对于哈佛结构的CPU，汇编程序的代码段和数据段应当分别存放在IROM和DRAM。DRAM的基地址为`0x0000_0000`，而start.dump程序的数据段基地址为`0x0000_4000`。因此，为了使得CPU能够正确运行start.dump程序，我们需要将测试程序访问DRAM的地址减去`0x0000_4000`，即：

&emsp;&emsp;原DRAM实例化代码：

``` Verilog
1|    dram U_dram (
2|        .clk    (cpu_clk),
3|        .a      (waddr[15:2]),
4|        .spo    (rdata),
5|        .we     (we),
6|        .d      (wdata)
7|    );
```

&emsp;&emsp;修改后的DRAM实例化代码：

``` Verilog
1|    wire [31:0] waddr_tmp = waddr - 32'h4000;
2|    
3|    dram U_dram (
4|        .clk    (cpu_clk),
5|        .a      (waddr_tmp[15:2]),
6|        .spo    (rdata),
7|        .we     (we),
8|        .d      (wdata)
9|    );
```


## 2. 存储器IP替换

&emsp;&emsp;由于下板测试程序较大，导致导入.coe后重新综合工程耗时较长，因此课程指导书网站提供了已综合好的IROM和DRAM IP核，同学们可将其直接导入CPU工程中使用。

&emsp;&emsp;下面以导入DRAM为例，介绍导入已综合IP核的方法 (IROM同理)。

&emsp;&emsp;首先，从指导书网站<a href="https://gitee.com/hitsz-cslab/cpu/blob/master/stupkt/download_test.zip" target="_blank">下载download_test.zip压缩包</a>文件并解压到无中文路径下。

&emsp;&emsp;然后，备份原工程，按照上一节所述修改RTL代码，并删除已实例化的IROM和DRAM IP核。

&emsp;&emsp;接着，按下图所示点击添加IP核。

<center><img src = "../assets/1.png" width = 450></center>

<center><img src = "../assets/2.png" width></center>

<center><img src = "../assets/3.png" width = 600></center>
<center>从解压的目录中添加已综合的DRAM IP核</center>



## 3. 顶层模块端口定义

``` Verilog
 1|    module top (
 2|        input  wire clk_i,
 3|        input  wire rst_i,
 4|        output wire led0_en_o,
 5|        output wire led1_en_o,
 6|        output wire led2_en_o,
 7|        output wire led3_en_o,
 8|        output wire led4_en_o,
 9|        output wire led5_en_o,
10|        output wire led6_en_o,
11|        output wire led7_en_o,
12|        output wire led_ca_o,
13|        output wire led_cb_o,
14|        output wire led_cc_o,
15|        output wire led_cd_o,
16|        output wire led_ce_o,
17|        output wire led_cf_o,
18|        output wire led_cg_o,
19|        output wire led_dp_o
20|    );
21|    
22|    wire rst_n = !rst_i;
```

!!! warning "注意 :gun:"
    &emsp;&emsp;板上的复位信号（按键开关`S6`，即`P20`引脚）默认输出低电平，按下时输出高电平，因此`rst_i`是高电平复位。


## 4. 下板结果说明

&emsp;&emsp;cdp-tests测试包给了37条指令（24条必做和13条选做）的测试程序。每通过一个测试点，数码管显示的数值会加1，直至测试完毕。测试完毕时，数码管将以16进制形式显示通过测试的指令数。例如，如果实现了24条必做指令并通过了测试，则数码管将显示0x25000018。数码管高8位显示0x25，表示共有37个测试点，数码管低8位显示0x18，表示通过了24个测试点。

&emsp;&emsp;需要注意的是，对于测试不通过的情形，如果数码管显示的值停在n，则表示第n+1条指令的测试失败。测试失败时，可查看start.dump的测试代码以定位出错指令，并进行相应的调试。
