### Description
设计一个 $100$ 位的循环移位寄存器

当使能信号为 $01$ 时，右移

当使能信号为 $10$ 时，左移

载入信号优先级高于使能信号
### Code

``` verilog
module top_module(
    input clk,
    input load,
    input [1:0] ena,
    input [99:0] data,
    output reg [99:0] q); 
    
    always @(posedge clk) begin
        if(load) q <= data;
        else if(ena == 2'b01) q <= {q[0], q[99:1]};
        else if(ena == 2'b10) q <= {q[98:0], q[99]};
        else q <= q;
    end

endmodule

```
