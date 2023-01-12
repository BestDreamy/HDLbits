### Description
使用一个 4 位的计数器实现 1-12 的计数器

4 位计数器模块为

```
module count4(
	input clk,
	input enable, // 使能
	input load, // 复位信号
	input [3:0] d, // 复位初始值
	output reg [3:0] Q
);
```

$reset$ ：同步复位，高电平有效
$load$ ：$reset$ 有效时，或计数器为 $Q == 12$ 且可以正常工作 ($enable == 1$) 

### Code

``` verilog
module top_module (
    input clk,
    input reset,
    input enable,
    output [3:0] Q,
    output c_enable,
    output c_load,
    output [3:0] c_d
); 

    assign c_enable = enable;
    assign c_load = (enable && Q == 12) || reset? 1: 0; 
    assign c_d = 1;
    count4 inst(clk, c_enable, c_load, c_d, Q);

endmodule
```
