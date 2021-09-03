&emsp;&emsp;流水线中有一种情况，使得在下一个时钟周期中下一条指令无法执行，这种情况被称为冒险(hazard)，分类如图3-7所示；

<center><img src = "../assets/3-7.png" width = 350></center>
<center>图3-7 流水线冒险</center>

## 1. 结构冒险

&emsp;&emsp;因缺乏硬件支持而导致指令不能在预定的时钟周期内执行的情况。

&emsp;&emsp;流水线中只有一个存储器，数据与指令共享，如图3-8所示。

<center><img src = "../assets/3-8.png" width = 600></center>
<center>图3-8 结构冒险</center>

- 解决办法

&emsp;&emsp;采用哈佛结构，指令与数据的存储器分开。

## 2. 数据冒险

&emsp;&emsp;一条指令依赖于前面一条尚在流水线中的指令。

&emsp;&emsp;因无法提供指令执行所需数据而导致指令不能在预期的时钟周期内执行的情况，如图3-9所示。

<center><img src = "../assets/3-9.png" width = 600></center>
<center>图3-9 数据冒险</center>

### 情形A

&emsp;&emsp;第2条指令译码阶段访问的寄存器与第1条指令执行阶段将要写回的寄存器冲突，如图3-10所示。

<center><img src = "../assets/3-10.png"></center>
<center>图3-10 数据冒险情形A</center>

- 判断条件：  
ID/EX .RS1 = EX/MEM.RD = x19  
ID/EX .RS2 = EX/MEM.RD = x19

### 情形B

&emsp;&emsp;第3条指令译码阶段访问的寄存器与第1条指令访存阶段将要写回的寄存器冲突，如图3-11所示。

<center><img src = "../assets/3-11.png"></center>
<center>图3-11 数据冒险情形B</center>

- 判断条件：  
ID/EX .RS1 = MEM/WB.RD = x19  
ID/EX .RS2 = MEM/WB.RD = x19

### 情形C

&emsp;&emsp;第4条指令译码阶段访问的寄存器与第1条指令写回阶段将要写回的寄存器冲突，如图3-12所示。

<center><img src = "../assets/3-12.png"></center>
<center>图3-12 数据冒险情形C</center>

- 判断条件：  
ID/EX .RS1 = WB.RD = x19  
ID/EX .RS2 = WB.RD = x19

&emsp;&emsp;RTL实现（情形C）

<center><img src = "../assets/rtl2.png" width = 450></center>

&emsp;&emsp;添加了数据冒险的数据通路框图如图3-13所示：

<center><img src = "../assets/3-13.png"></center>
<center>图3-13 增加数据冒险检测的数据通路</center>


## 3. 控制冒险

&emsp;&emsp;由于取到的指令并不是所需要的，或者指令地址的流向不是流水线所预期的，导致正确的指令无法在正确的时钟周期内执行的情况。

&emsp;&emsp;分支指令之后的IF阶段依赖于分支的结果，流水线无法保证永远取到正确的指令，如图3-14所示。

<center><img src = "../assets/3-14.png" width = 600></center>
<center>图3-14 控制冒险</center>
