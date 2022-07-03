## 1. 实验目的

&emsp;&emsp;（1）理解流水线CPU工作过程；

&emsp;&emsp;（2）理解流水线冲突产生的原因及其解决方法；

&emsp;&emsp;（3）掌握流水线CPU设计与实现方法。



## 2. 实验内容

&emsp;&emsp;本实验要求同学们在Minisys开发板 (芯片型号：<font color = blue>**XC7A100TFGG484-1**</font>) 上，改造单周期CPU并实现支持miniRV-1指令集的流水线CPU。

&emsp;&emsp;具体要求如下：

&emsp;&emsp;（1）<u>至少</u>实现 **五级** 流水线；

&emsp;&emsp;（2）支持必做的24条指令，可添加选做指令；

&emsp;&emsp;（3）改造单周期CPU数据通路，设计理想流水线CPU；

&emsp;&emsp;（4）实现流水线暂停机制，解决数据冒险；

&emsp;&emsp;（5）实现数据前递机制，解决数据冒险；

&emsp;&emsp;（6）实现分支预测（选做），解决控制冒险；

&emsp;&emsp;（7）所实现的流水线CPU主频<u>不低于50MHz</u>。
