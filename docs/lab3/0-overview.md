## 1. 实验目的

&emsp;&emsp;（1）理解流水线CPU工作过程；

&emsp;&emsp;（2）理解流水线冲突产生的原因及其解决方法；

&emsp;&emsp;（3）掌握流水线CPU设计与实现方法。



## 2. 实验内容

&emsp;&emsp;本实验要求同学们在Minisys开发板 (芯片型号：<font color = blue>**XC7A100TFGG484-1**</font>) 上，改造单周期CPU并实现支持miniRV-1指令集的流水线CPU。

&emsp;&emsp;具体要求如下：

&emsp;&emsp;（1）<u>至少</u>实现 **五级** 流水线；

&emsp;&emsp;（2）采用哈佛结构，即分开的指令与数据接口；

&emsp;&emsp;（3）含有32个32位整数寄存器；

&emsp;&emsp;（4）支持必做的24条指令，可添加选做指令；

&emsp;&emsp;（5）具有32位的数据、地址总线宽度；

&emsp;&emsp;（6）实现异常和外部中断的处理（选做）。
