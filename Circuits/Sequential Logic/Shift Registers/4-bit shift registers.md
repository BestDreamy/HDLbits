### Description
设计一个 $4$ 位右移寄存器

复位信号，采用异步复位，且高电平有效。

载入信号，将 $data$ 载入 $Q$ 中。

载入信号优先级大于使能信号
### Code

``` verilog
module top_module(
    input clk,
    input areset,  // async active-high reset to zero
    input load,
    input ena,
    input [3:0] data,
    output reg [3:0] q); 

    always @(posedge clk, posedge areset) begin
        if(areset) q <= 0;
        else if(load) q <= data;
        else if(ena) q <= {1'b0, q[3:1]};
        else q <= q;
    end
endmodule
```
