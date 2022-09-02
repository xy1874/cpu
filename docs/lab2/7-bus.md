## 1. 总线概述

&emsp;&emsp;总线是连接多个部件或设备的用于信息传输的一组公共信号线。按照总线所连接的部件类型，可将总线分为片内总线、系统总线和通信总线3种。

&emsp;&emsp;片内总线指的是芯片内部的用于连接多个元器件的总线，功能一般较为单一，是最为微观层面的总线。<u>系统总线指的是计算机系统内部的用于连接CPU、主存、I/O设备的主要部件的总线，通常由控制总线、地址总线和数据总线组成</u>。通信总线则指的是计算机之间或计算机与其他系统之间的用于通信的总线，通常只由数据总线和控制信号线组成。



## 2. DRAM总线

&emsp;&emsp;目前为止，我们所设计的myCPU直接通过Distributed RAM IP核的固有接口连接到主存，其顶层模块结构形如图8-1所示。

<center><img src = "../assets/8-1.png" width = 230></center>
<center>图8-1 myCPU顶层模块结构图</center>

&emsp;&emsp;在大部分现实的计算机系统中，CPU通过系统总线与主存、外设等进行连接和交互。出于便捷性考虑，我们可以<u>直接选取Distributed RAM IP核的接口信号总线作为我们的系统总线</u>，我们不妨将这样的系统总线称为DRAM总线（“D”代表“Distributed”）。

&emsp;&emsp;显然，DRAM总线包含以下信号：

<center>表8-1 DRAM总线接口信号</center>

<center>

| 信号名 | 位宽 | 方向 | 释义 |
| :-: | :-: | :-: | :-: |
| clk | 1 | master->slave | 时钟信号 |
| addr | 14 | master->slave | 地址信号 |
| rdata | 32 | slave->master | 读数据信号 |
| wen | 1 | master->slave | 写使能信号 |
| wdata | 32 | master->slave | 写数据信号 |

</center>



## 3. 总线桥

&emsp;&emsp;虽然DRAM总线将myCPU与指令ROM、数据RAM连接在了一起，但显然指令ROM、数据RAM与myCPU之间仍然是一对一通信的关系。如果想要为myCPU连接更多的部件或设备，单纯增加myCPU的DRAM总线接口数量显然不是一个好方法。此时，需要为myCPU设计总线桥，并将图8-1所示的顶层模块结构改造成如图8-2所示。

<center><img src = "../assets/8-2.png" width = 500></center>
<center>图8-2 带有总线桥的myCPU顶层模块结构图</center>

&emsp;&emsp;总线桥是一个特殊的接口部件，其主要功能是作为总线操作的 **中转机构** 和 **控制机构**。当CPU访问数据RAM或其他设备时，总线桥将把接收到的访问请求转发给相应的从设备。此外，若存在多个部件或设备同时请求总线的情形，总线桥还需进行总线仲裁。

&emsp;&emsp;
