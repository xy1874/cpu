## 1. 实验报告要求

&emsp;&emsp;务必按照<a href="https://gitee.com/hitsz-cslab/cpu/blob/master/stupkt/%E8%AE%A1%E7%AE%97%E6%9C%BA%E8%AE%BE%E8%AE%A1%E4%B8%8E%E5%AE%9E%E8%B7%B5-%E6%8A%A5%E5%91%8A%E6%A8%A1%E6%9D%BF-20220715.docx" target="_blank">实验报告模板</a>认真撰写！

!!! info "PS :mega:"
    &emsp;&emsp;实验报告要求将 **单周期** 和 **流水线** CPU的<u>资源使用情况</u>、<u>功耗</u>等数据截图。截图前，先让工程运行完综合（Synthesis）与实现（Implementation）。
    
    &emsp;&emsp;点击Vivado菜单栏的`Project Summary`按钮，如图1所示。

    <center><img src = "../assets/j-1.png" width = 500></center>
    <center>图1 查看工程的简要报告</center>

    &emsp;&emsp;然后，找到页面下方的`Timing`、`Utilization`以及`Power`项，点击`Post-Implementation`后截图保存，如图2所示。

    <center><img src = "../assets/j-2.png"></center>
    <center>图2 将资源使用情况和功耗数据截图</center>

## 2. 提交要求

&emsp;&emsp;提交时，首先新建"<font color=green>**学号_姓名_CPU设计与实践**</font>"文件夹，并在其中放入下列文件/文件夹：

``` C
- 学号_姓名_CPU设计与实践
    |- 学号_姓名.doc/pdf /* 0. 实验报告 */
    |- single_cycle     /* 1. 单周期CPU源码文件夹 */
    |   |- xxx.v            /* 单周期CPU的Verilog源文件 */
    |   |- ...
    |- pipeline         /* 2. 流水线CPU源码文件夹 */
        |- xxx.v            /* 流水线CPU的Verilog源文件 */
        |- ...
```

!!! Warning
    - 报告请 **以PDF格式提交**；
    - 不论是单周期还是流水线，只需 **提交.v格式的源文件** ，**不需提交Vivado工程**；
    - 不论是单周期还是流水线，都要求 **提交能够下板运行汇编实验计算器的版本** 的.v源代码；
    - 作业系统 **只支持提交.zip格式** 的压缩包；  
    - 作业系统 **不支持提交大于100MB** 的文件。

&emsp;&emsp;然后，将"<font color=green>**学号_姓名_CPU设计与实践**</font>"文件夹压缩<font color=orange>**打包成.zip**</font>，提交到作业系统。

&emsp;&emsp;提交<font color=red>**Deadline**</font>：请自行登录作业系统查看截止时间。
