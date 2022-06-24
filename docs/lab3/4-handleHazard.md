## 1. 流水线暂停

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

## 2. 数据前递

&emsp;&emsp;数据前递是指将执行阶段的结果、访存阶段的结果前递到译码阶段，参与译码阶段运算源操作数的选择，如图3-19所示。

<center><img src = "../assets/3-19.png" width = 500></center>
<center>图3-19 数据前递</center>

&emsp;&emsp;添加了数据前递机制的数据通路框图如图3-20所示：

<center><img src = "../assets/3-20.png"></center>
<center>图3-20 增加数据前递机制的数据通路</center>

- RTL实现（情形C）

<center><img src = "../assets/rtl4.png" width = 480></center>
