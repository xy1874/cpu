&emsp;&emsp;对于miniLA指令集，汇编实验需使用LoongSonAssembler工具。

&emsp;&emsp;LoongSonAssembler是一个支持LA32R指令集的，具有编辑器、调试器和模拟器等功能的集成开发工具，其基本界面如图4-1所示。

<center><img src = "../assets/4-1.png" width = 500></center>
<center>图4-1 LoongSonAssembler基本界面</center>

&emsp;&emsp;左侧功能区主要包含文件操作、结果生成以及调试三部分。LoongSonAssembler支持`.asm`和`.txt`两种格式的汇编源文件。

&emsp;&emsp;编写汇编代码时，LoongSonAssembler具备高亮显示和语法错误提示等功能，如图4-2所示。

<center><img src = "../assets/4-2.png" width = 100%></center>
<center>图4-2 语法高亮及错误提示示例</center>

&emsp;&emsp;代码编写完毕后，按下快捷键 ++ctrl+s++ 保存文件，再点击左侧功能区的蓝色齿轮按钮（或按下快捷键 ++f5++ ）即可进行汇编。

&emsp;&emsp;汇编后，可在界面上方的Result标签页查看汇编后的机器码及内存布局，如图4-3所示。

<center><img src = "../assets/4-3.png" width = 100%></center>
<center>图4-3 在Result标签页下查看汇编结果</center>

&emsp;&emsp;在Debugger标签页下，可对程序进行调试，如图4-4所示。调试时，点击左侧功能区的蓝色箭头（或使用快捷键 ++f11++ ）可实现单步调试；点击绿色箭头（或使用快捷键 ++f12++ ）可运行程序。

<center><img src = "../assets/4-4.png" width = 100%></center>
<center>图4-4 在Result标签页下调试程序</center>

&emsp;&emsp;在调试时，LoongSonAssembler工具还支持设置断点、运行时查看寄存器和内存数据的变化、死循环检测等功能。
