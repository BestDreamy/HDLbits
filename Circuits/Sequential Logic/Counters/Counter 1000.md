### 题目描述
利用 $bcd$ 计数器将 $1000Hz$ 的时钟信号，变为 $1Hz$ 的时钟

可以实例化三个 $BCDcount$ 分别为 $b0, b1, b2$

$b0$: 一直处于工作状态，故使能一直为 $1$

$b1$: 当实例对象 $b0$ 输出为 $4'b1001$ 时， $b1$ 的使能信号才可为 $1$

$b2$: 当实例对象 $b0, b1$ 输出为 $8'b1001\_1001$ 时， $b2$ 的使能信号才可为 $1$


### 通过代码
```
module top_module (
    input clk,
    input reset,
    output OneHertz,
    output [2:0] c_enable
); //

    wire [11:0] Q;
    
    assign c_enable[0] = 1;
    assign c_enable[1] = (Q[3:0] == 4'h9);
    assign c_enable[2] = (Q[7:0] == 8'h99);
    assign OneHertz = (Q[11:0] == 12'h999);
    
    bcdcount inst0(clk, reset, c_enable[0], Q[3:0]);
    bcdcount inst1(clk, reset, c_enable[1], Q[7:4]);
    bcdcount inst2(clk, reset, c_enable[2], Q[11:8]);

endmodule
```
