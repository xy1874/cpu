# 实验注意事项

## 宏定义使用
类似于c等高级语言开发，应避免在代码中使用魔数，而多使用const或宏定义。Verilog同样定义了宏定义，在如下的场景中，使用宏定义可以提高代码可读性，方便维护

![宏定义场景](./asset/value_matrix.png)


宏定义使用举例：  
1.新建参数模块文件（我命名为para.v）;

2.在para.v文件中使用'define宏定义参数:
```angular2
　　　　//`define+name+参数 　
　　　　`define 　　STATE_INIT　　  3'd0
　　　　`define 　　STATE_IDLE　 　 3'd1
　　　　`define 　　STATE_WRIT　　  3'd2
　　　　`define 　　STATE_READ　　  3'd3
　　　　`define 　　STATE_WORK     3'd4
　　　　`define 　　STATE_RETU　　  3'd5      //参数后面不能有分号
```

3.在需要调用参数的文件init.v中使用`include "para.v",使用`name进行调用。 
```angular2
　　　　`include "para.v"
        ...
        state_init <= `STATE_INIT;
```

　　　　