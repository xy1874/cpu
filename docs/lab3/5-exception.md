## 1. 异常

&emsp;&emsp;异常机制，即CPU在顺序执行程序指令流的过程中突然遇到了异常的事情而中止执行当前的程序，转而去处理该异常。

&emsp;&emsp;1）同步异常（Synchronous Exception）  
&emsp;&emsp;是指由于执行程序指令流或者试图执行程序指令流而造成的异常。

&emsp;&emsp;2）异步异常（Asynchronous Exception）  
&emsp;&emsp;是指那些产生原因不能够被精确定位于某条指令的异常。

- 相关寄存器

<center>表3-1 异常相关的寄存器</center>
<center><img src = "../assets/t3-1.png" width = 500></center>

- 进入异常

&emsp;&emsp;举例：add指令 add x1, x2, x1 的EX阶段发生了硬件故障。

A.	IF阶段的指令：变为nop操作  
B.	ID阶段的指令：增加新的逻辑部件，使得译码阶段的输出为0  
C.	EX阶段的指令：需要保留x1原本的值  
D.	之前的指令，正常完成  
E.	设置CSR.MECAUSE和CSR.MSTATUS的寄存器值  
F.	转到例外处理程序CSR.MTVEC

- 退出异常

A.	执行当前程序流，转而从CSR 寄存器CSR.MEPC定义的PC地址开始执行。  
B.	更新CSR.MSTATUS。

## 2. 中断

&emsp;&emsp;中断机制，即CPU执行程序指令流的过程中突然被别的请求打断而中止执行当前的程序，转而去处理别的事情，待其处理完了别的事情，然后重新回到之前程序中断的点继续执行之前的程序指令流。

- 中断类型

外部中断（External Interrupt）  
计时器中断（Timer Interrupt）  
软件中断（Software Interrupt）  
调试中断（Debug Interrupt）

- 中断屏蔽

&emsp;&emsp;RISC-V 的异常是不可以被屏蔽的，而中断则可以被屏蔽掉，RISC-V 架构定义了CSR 寄存器机器模式中断便能寄存器MIE（Machine Interrupt Enable Registers）可以用于控制中断的屏蔽，如图3-26所示。

<center><img src = "../assets/3-26.png"></center>
<center>图3-26 中断屏蔽寄存器</center>

!!! 说明
    MEIE域控制外部中断的屏蔽。  
    MTIE域控制计时器中断的屏蔽。  
    MSIE域控制软件中断的屏蔽。

- 中断等待

&emsp;&emsp;RISC-V 架构定义了CSR 寄存器中断等待寄存器MIP（Machine Interrupt Pending Registers）可以用于查询中断的等待状态，如图3-27所示。

<center><img src = "../assets/3-27.png"></center>
<center>图3-27中断等待寄存器</center>

!!! 说明
    MEIP 域反映外部中断的等待状态。  
    MTIP 域反映计时器中断的等待状态。  
    MSIP 域反映软件中断的等待状态。

- 中断优先级与仲裁

&emsp;&emsp;对于RISC-V 架构而言，如果3 种中断同时发生，其响应的优先级顺序如下， MCAUSE寄存器中将按此优先级顺序选择更新异常编号（Exception Code）的值。

!!! 中断优先级
    外部中断优先级最高。  
    软件中断其次。  
    计时器中断再次。

- 中断处理流程

A.	外设发出中断信号。  
B.	保存此时的CSR寄存器（MSTATUS、MEPC、MCAUSE）。  
C.	跳转到中断处理程序（MTVEC）。  
D.	关闭其他中断响应使能（不支持嵌套）。  
E.	然后处理中断（过程中会清掉外设的中断）。  
F.	恢复CSR寄存器。  
G.	跳转PC跳回原来位置退出中断。

&emsp;&emsp;添加了异常与中断处理的数据通路如图3-28所示。

<center><img src = "../assets/3-28.png"></center>
<center>图3-28 增加异常与中断处理的数据通路</center>
