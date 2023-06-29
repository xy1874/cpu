## 1. 实验目的

&emsp;&emsp;（1）进一步熟悉RARS、Logisim等汇编和模拟仿真工具的使用；

&emsp;&emsp;（2）熟练掌握RISC-V汇编语言，熟悉并理解RISC-V指令系统；

&emsp;&emsp;（3）了解程序在单周期RISC-V CPU搭建的SoC中的运行过程。



## 2. 实验内容 (for miniRV)

### 题目1：基本输入输出

&emsp;&emsp;在提供的Logisim环境中，运行提供的程序<font color = blue>**Exercise1.asm**</font>，并阅读该程序源码，分析程序功能，学习汇编程序如何访问I/O接口及外设。

&emsp;&emsp;数码管、LED、拨码开关的端口地址如表1-1所示。

<center>表1-1 数码管、LED、拨码开关端口地址</center>
<center>

| 外设 | 基址 |
| :-: | :-: |
| 数码管 | 0xFFFF_F000 |
| LED | 0xFFFF_F060 |
| 拨码开关 | 0xFFFF_F070 |

</center>

&emsp;&emsp;根据实验步骤运行程序，熟悉Logisim工具及操作流程。

### 题目2：简易计算器设计

&emsp;&emsp;运用miniRV指令集编写汇编程序，实现简易计算器。计算器是输入、输出都是8bit的数据。

&emsp;&emsp;计算器执行<u>逻辑运算</u>时，需将输入的操作数A和B视为 ==无符号数==；执行<u>其他运算</u>时，需将输入的操作数A和B视为 ==有符号数== 。

!!! danger "注意 :bomb:"
    &emsp;&emsp;计算器程序最终需要下板运行和演示，因此 **禁止使用除miniRV指令集以外的其他指令**。
    
    &emsp;&emsp;如果后续设计CPU时，只实现24条必做指令，则编写本题目时不要使用附加的13条指令。

&emsp;&emsp;计算器的运算符和操作数A、B均通过拨码开关输入，如表1-2所示。

<center>表1-2 8位简易计算器的拨码开关输入定义</center>
<center>

| 拨码开关 | SW[23:21] | SW[20:16] | SW[15:8] | SW[7:0] |
| :-: | :-: | :-: | :-: | :-: |
| 输入功能 | 运算符 | 保留 | 操作数A | 操作数B |

</center>

&emsp;&emsp;计算器的运算结果需显示到数码管DK7~DK0。具体地，计算器的功能如表1-3所示。

<center>表1-3 运算器功能定义</center>
<center>

| SW[23:21] | 运算类型 | 数码管显示内容 |
| :-: | :-: | :-: |
| 3'b000 | A & B | 运算结果以2进制显示 |
| 3'b001 | A \| B | 运算结果以2进制显示 |
| 3'b010 | A ^ B | 运算结果以2进制显示 |
| 3'b011 | A << B | 运算结果以有符号整数显示 |
| 3'b100 | A >> B (算术右移) | 运算结果以有符号整数显示 |
| 3'b101 | (A == 0) ? B : [B]~补~ | 运算结果以2进制显示 |
| 3'b110 | A $\div$ B | 除法的 **商** 以有符号整数显示 |

</center>

!!! tip "提示 :bulb:"
    &emsp;&emsp;除法可使用《计算机组成原理》中的恢复余数法或加减交替法等算法来实现。  
    &emsp;&emsp;PPT回顾：  
    <embed src = "https://docs.qq.com/pdf/DZGF2cXhWUEhPdklk#page=50" width="100%" height="500">
    
    &emsp;&emsp;以上两种算法也适用于整数除法，注意试商是从被除数最高位开始的。符号位单独处理，取除数和被除数的绝对值进行运算。  
    &emsp;&emsp;设被除数$x$ = +`1101B`，除数$y$ = -`0110B`；$x$的绝对值$x^*$ = `01101B`，$y$的绝对值$y^*$ = `00110B`，$y^*$的补码$[-y^*]_补$ = `11010B`，则有：  
    
    === "恢复余数法"
        &emsp;&emsp;令被除数减去$y^*$，即加上$[-y^*]_补$：  
        &emsp;&emsp;**·** 若得到的余数为负数，表明不够减，故商上0，并加上$y^*$以恢复余数，再令被除数左移1位；  
        &emsp;&emsp;**·** 若得到的余数为正数，表明够减，故商上1，再令被除数左移1位；  
        &emsp;&emsp;重复以上步骤N次，N为位宽。在本例中，N = 4。
        <center><img src = "../assets/div_rr.png" width = 64%></center>
    
    === "加减交替法"
        &emsp;&emsp;令被除数减去$y^*$，即加上$[-y^*]_补$：  
        &emsp;&emsp;**·** 若得到的余数为负数，表明不够减，故商上0，令被除数左移1位，再加上$[y]_补$；  
        &emsp;&emsp;**·** 若得到的余数为正数，表明够减，故商上1，令被除数左移1位，再加上$[-y^*]_补$；  
        &emsp;&emsp;重复以上步骤N次，N为位宽。在本例中，N = 4。
        <center><img src = "../assets/div_as.png" width = 64%></center>



## 2A. 实验内容 (for miniLA)

### 题目1：基本输入输出

&emsp;&emsp;在提供的LoongArchAssembler汇编及模拟工具中，运行提供的程序<font color = blue>**Exercise1A.asm**</font>，并阅读该程序源码，分析程序功能，学习汇编程序如何访问I/O接口及外设。

&emsp;&emsp;数码管、LED、拨码开关的端口地址同表1-1。

&emsp;&emsp;阅读LoongArchAssembler简介文档，熟悉汇编及模拟工具的操作流程。

### 题目2：简易计算器设计

&emsp;&emsp;题目内容与miniRV的类似，但有以下不同：

&emsp;&emsp;（1）只能使用miniLA指令集中的指令实现计算器；如果后续设计CPU时，只实现25条必做指令，则编写本题目时不要使用附加的13条指令；

&emsp;&emsp;（2）使用提供的汇编程序模板`calculator.asm`完成汇编实验。在模板程序中，输入数据提前存放在数据段中，运算结果也写入到数据段中 (具体地址自定)，如图1-1所示。^^将来下板时，将获取输入数据的地址改成拨码开关的I/O地址、将保存运算结果的地址改成数码管的I/O地址即可^^。

``` asm
 1|.data
 2|    test0: .word 0x00f624    # test0: SW[23:21]==3'b000, A==8'hf6, B==8'h24
 3|    test1: .word 0x201430    # test1: SW[23:21]==3'b001, A==8'h14, B==8'h30
 4|    test2: .word 0x401213    # test2: SW[23:21]==3'b010, A==8'h12, B==8'h13
 5|    test3: .word 0x608812    # test3: SW[23:21]==3'b011, A==-120,  B==18
 6|    test4: .word 0x8023f4    # test4: SW[23:21]==3'b100, A==35,    B==-12
 7|    test5: .word 0xa000fa    # test5: SW[23:21]==3'b101, A==0,     B==-6
 8|    test6: .word 0xa001ef    # test6: SW[23:21]==3'b101, A==1,     B==-17
 9|    test7: .word 0xc0f102    # test7: SW[23:21]==3'b110, A==-15,   B==2
10|    test8: .word 0xc0eefc    # test8: SW[23:21]==3'b110, A==-18,   B==-4
11|
12|.text
13|MAIN:
14|    # TODO
```
<center>图1-1 `calculator.asm`模板代码节选片段</center>
