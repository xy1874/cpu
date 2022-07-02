&emsp;&emsp;（1）根据给出的数据通路表示例，完成数据通路表，画出数据通路图;

&emsp;&emsp;（2）根据给出的控制信号表示例，完成控制信号表，在数据通路图上添加控制信号，完成单周期CPU设计结构图;

&emsp;&emsp;（3）划分模块，新建`.v`源文件并使用Verilog HDL实现各模块 (芯片型号：<font color=blue>**XC7A100TFGG484-1**</font>);

&emsp;&emsp;（4）编写仿真程序和使用Trace验证方法，测试设计的正确性;

&emsp;&emsp;（5）设计总线桥和I/O接口（必做外设接口：数码管、拨码开关、按键开关、LED）；

&emsp;&emsp;（6）添加约束文件，下板调试及设计优化。

## **·** 添加约束文件

&emsp;&emsp;下板前，需编写引脚约束文件（.xdc），以将顶层模块的接口信号与Minisys开发板上的FPGA芯片引脚连接起来。

&emsp;&emsp;在Vivado中，右键点击`Project Manager`下面`Sources`中的`Constraints`，在弹出的菜单中选择`Add Source…`，并按照图11-1所示的设置点击`Next`。

<center><img src="../assets/11-1.png"></center>
<center>图11-1 添加或创建约束文件</center>

&emsp;&emsp;约束文件初始为空，其基本语法如图11-2所示。

```
1| # 语法格式为set_property PACKAGE_PIN (pin location) [get_ports (port name)]
2| set_property PACKAGE_PIN P5 [get_ports sw_i[0]]
3| set_property PACKAGE_PIN F6 [get_ports led_o[0]]
4|
5| # 语法格式为set_property IOSTANDARD (level:LVDS,LVCMOS18,LVCMOS33 etc.) [get_ports (port name)]
6| set_property IOSTANDARD LVCMOS33 [get_ports sw_i[0]]
7| set_property IOSTANDARD LVCMOS33 [get_ports led_o[0]]
```
<center>图11-2 约束文件基本语法</center>

&emsp;&emsp;约束文件主要包含两部分内容，第一部分是信号和FPGA芯片引脚的对应关系，比如第1行是将FPGA芯片的`P5`引脚和信号`sw_i[0]`信号绑定。第二部分是引脚的电气属性设置，`IOSTANDARD`全部设置成`LVCMOS33`，表示该IO电平标准是`CMOS 3.3v`（即高电平对应3.3V，低电平对应0.0V）。

!!! info "PS :mega:"
    &emsp;&emsp;图11-2所示的引脚约束代码也可写成：

    ```
    1| set_property -dict { IOSTANDARD LVCMOS33 PACKAGE_PIN P5 } [get_ports sw_i[0]]
    2| set_property -dict { IOSTANDARD LVCMOS33 PACKAGE_PIN F6 } [get_ports led_o[0]]
    ```

&emsp;&emsp;引脚分配既可以参考[Minisys开发板手册](https://gitee.com/hitsz-cslab/cpu/blob/master/stupkt/Minisys%E7%A1%AC%E4%BB%B6%E6%89%8B%E5%86%8C.pdf)，也可以直接在开发板的丝印上读出。例如，观察开发板最右侧的拨码开关下方丝印着“`SW0-W4`”，表示拨码开关SW0对应FPGA的`W4`引脚。

&emsp;&emsp;约束文件除了对物理引脚进行约束，还可以对时序进行约束。感兴趣的同学们可以阅读参考资料《[约束功能概述](http://xilinx.eetrend.com/blog/2019/100018252.html)》。
