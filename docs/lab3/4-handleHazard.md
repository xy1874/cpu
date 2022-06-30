## 1. 数据冒险的检测

&emsp;&emsp;要想解决流水线处理器中的数据冒险问题，首先需要在指令流中检测出数据冒险。

&emsp;&emsp;下面以图4-1为例介绍RAW冒险三种情形的检测方法。

<center><img src = "../assets/4-1.png" width = 600></center>
<center>图4-1 RAW数据冒险的三种情形</center>

### 1.1 RAW情形A检测

&emsp;&emsp;相邻指令发生RAW冒险 —— 图4-1中，第2条指令在译码阶段访问的寄存器与第1条指令在执行阶段将要写回的寄存器冲突。

&emsp;&emsp;流水线CPU中，与情形A有关的数据通路如图4-2所示。

<center><img src = "../assets/4-2.png" width = 700></center>
<center>图4-2 RAW情形A有关的流水线数据通路</center>

&emsp;&emsp;分析图4-2所示的数据通路，不难发现RAW情形A的判断方法/检测逻辑如下：

<center>
( REG~**EX/MEM**~.RD == ID.RS1 ) || ( REG~**EX/MEM**~.RD == ID.RS2 )
</center>

&emsp;&emsp;其中，REG~EX/MEM~表示执行阶段和访存阶段之间的流水线寄存器，下同；REG~EX/MEM~.RD表示EX/MEM流水线寄存器的RD字段，观察图4-2可知该字段暂存的是从ID级传递而来的目标寄存器的寄存器号。

!!! info "补充说明 :mega:"
    &emsp;&emsp;对于图4-1中的情形A，有REG~EX/MEM~.RD == ID.RS1 == `5'd19`。

### 1.2 RAW情形B检测

&emsp;&emsp;间隔1条指令发生RAW冒险 —— 图4-1中，第3条指令在译码阶段访问的寄存器与第1条指令在访存阶段将要写回的寄存器冲突。

&emsp;&emsp;流水线CPU中，与情形B有关的数据通路如图4-3所示。

<center><img src = "../assets/4-3.png" width = 700></center>
<center>图4-3 RAW情形B有关的流水线数据通路</center>

&emsp;&emsp;分析图4-3所示的数据通路，不难发现RAW情形B的判断方法/检测逻辑如下：

<center>
( REG~**MEM/WB**~.RD == ID.RS1 ) || ( REG~**MEM/WB**~.RD == ID.RS2 )
</center>

!!! info "补充说明 :mega:"
    &emsp;&emsp;对于图4-1中的情形B，有REG~MEM/WB~.RD == ID.RS1 == `5'd19`。

### 1.3 RAW情形C检测

&emsp;&emsp;间隔2条指令发生RAW冒险 —— 图4-1中，第4条指令在译码阶段访问的寄存器与第1条指令在写回阶段将要写回的寄存器冲突。

&emsp;&emsp;流水线CPU中，与情形C有关的数据通路如图4-4所示。

<center><img src = "../assets/4-4.png" width = 700></center>
<center>图4-4 RAW情形C有关的流水线数据通路</center>

&emsp;&emsp;分析图4-4所示的数据通路，不难发现RAW情形C的判断方法/检测逻辑如下：

<center>
( WB.RD == ID.RS1 ) || ( WB.RD == ID.RS2 )
</center>

&emsp;&emsp;其中，WB.RD表示写回阶段的目标寄存器的寄存器号。

!!! info "补充说明 :mega:"
    &emsp;&emsp;对于图4-1中的情形C，有WB.RD == ID.RS1 == `5'd19`。

!!! hint "参考实现 :sparkles:"
    &emsp;&emsp;对于RAW情形C，给出检测逻辑的参考实现代码：

    ``` Verilog
    wire rs1_id_wb_hazard = (wb_rd == id_rs1) & wb_we & id_rf1;
    wire rs2_id_wb_hazard = (wb_rd == id_rs2) & wb_we & id_rf2;
    ```

    &emsp;&emsp;其中，`wb_rd`表示WB阶段的目标寄存器的寄存器号；`wb_we`表示WB阶段的写使能信号；`id_rs1`表示ID阶段的RS1寄存器号；`id_rf1`表示ID阶段的RS1寄存器的<u>读标志信号</u>，该信号用于标识ID阶段的RS1是否被读取。
    
    &emsp;&emsp;特别注意的是，此处增加`id_rf1`和`id_rf2`信号是为了避免I型、U型和J型指令执行时出现RAW冒险的误判。

&emsp;&emsp;请同学们参考上述实现代码，自行完成RAW三种情形的检测逻辑。

&emsp;&emsp;添加了数据冒险检测逻辑的流水线CPU数据通路形如图4-5所示。

<center><img src = "../assets/4-5.png"></center>
<center>图4-5 具有数据冒险检测功能的流水线CPU数据通路示例</center>

## 2. 数据冒险解决方法

### 2.1 流水线暂停

&emsp;&emsp;即在流水线中插入暂停周期，如图3-15所示。

<center><img src = "../assets/3-15.png" width = 600></center>
<center>图3-15 流水线暂停</center>

- 实现方法：  
1）保持取值地址PC的值不变；  
2）保持流水线各个阶段的寄存器（IF/ID、ID/EX、EX/MEM、MEM/WB模块的输出）不变；

- 乘除法指令

&emsp;&emsp;由于乘除法指令的实现需要多周期完成，故可以采用流水线暂停的方式予以实现；

&emsp;&emsp;1）乘法指令

&emsp;&emsp;推荐采用2位Booth编码算法，如图3-16所示，32bits的乘法需要17个周期完成；

<center><img src = "../assets/3-16.png" width = 450></center>
<center>图3-16 2位Booth编码算法乘法器</center>

&emsp;&emsp;2）除法指令

&emsp;&emsp;推荐采用加减交替算法，如图3-17所示，32bits的除法需要32个周期完成；

<center><img src = "../assets/3-17.png" width = 500></center>
<center>图3-17 加减交替法除法器</center>

&emsp;&emsp;添加了流水线暂停机制的数据通路框图如图3-18所示：

<center><img src = "../assets/3-18.png"></center>
<center>图3-18 增加暂停机制的数据通路</center>

- RTL实现（IF/ID流水寄存器）

<center><img src = "../assets/rtl3.png" width = 450></center>

### 2.2 数据前推

&emsp;&emsp;数据前递是指将执行阶段的结果、访存阶段的结果前递到译码阶段，参与译码阶段运算源操作数的选择，如图3-19所示。

<center><img src = "../assets/3-19.png" width = 500></center>
<center>图3-19 数据前递</center>

&emsp;&emsp;添加了数据前递机制的数据通路框图如图3-20所示：

<center><img src = "../assets/3-20.png"></center>
<center>图3-20 增加数据前递机制的数据通路</center>

- RTL实现（情形C）

<center><img src = "../assets/rtl4.png" width = 480></center>

### 2.3 乱序执行

&emsp;&emsp;

&emsp;&emsp;乱序执行既可通过硬件动态实现，也可以通过编译器或汇编器静态实现。

&emsp;&emsp;乱序执行的硬件实现方法较为复杂，此处不详细介绍。感兴趣的同学们可自行查阅相关资料。
