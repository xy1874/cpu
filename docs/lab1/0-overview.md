## 实验目的

&emsp;&emsp;（1）学习MARS、logisim的使用；

&emsp;&emsp;（2）学习MIPS汇编语言，熟悉并理解MIPS指令系统；

&emsp;&emsp;（3）了解程序在单周期MIPS-CPU搭建的SOC中的运行。

&emsp;&emsp;

## 实验内容

&emsp;&emsp;汇编部分包含3个题目：

### 题目1：基本输入输出

&emsp;&emsp;LED和拨码开关的端口地址如下：

<center>

| 接口部件 | 首地址 |
| :-: | :-: |
| 低16位LED | 0xFFFFFC60 |
| 高8位LED | 0xFFFFFC62 |
| 低16位拨码开关 | 0xFFFFFC70 |
| 高8位拨码开关 | 0xFFFFFC72 |

</center>

&emsp;&emsp;实现功能：不断地从拨码开关读出数据，并将数据输出到对应的LED灯上。要求拨码开关为1时LED灯亮。

&emsp;&emsp;根据实验指导书，运行程序，熟悉实验过程。程序见<font color = red>**Exercise3.1.asm**</font>。

### 题目2：乘法运算

&emsp;&emsp;以原码一位乘为基础，设计一个数的平方和立方2种运算。由拨码开关SW[3:0]输入一个数，SW22为平方，SW23为立方，均为高电平有效。结果输出到YLD15~YLD0同时保存到存储器中。

&emsp;&emsp;一次运算结束后使用SW21清零（高电平有效），等待进行下一次运算。

### 题目3：节日彩灯

&emsp;&emsp;设计一个有24个发光二极管的彩灯程序。循环执行，每隔大约半秒变换一次LED灯。可设置如下变换模式：

```angular2
模式1：LED灯从两边向中间依次点亮，再从中间向两边依次熄灭。
模式2：从左向右依次亮1、2、3…24盏LED灯，至24盏LED灯全亮后，从左向右灭1、2、3…24盏LED灯，至所有灯全灭。
模式3：由拨码开关控制LED灯点亮连续X位（X由拨码开关输入），并循环右移。
```

&emsp;&emsp;Logisim时钟频率设置为4.1KHz

![MARS界面简介](part2.assets/freq_set.png)
