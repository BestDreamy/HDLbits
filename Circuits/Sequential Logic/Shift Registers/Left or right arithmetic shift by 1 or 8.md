### Description
设计一个 $64$ 位的算数移位寄存器

即左移补 $0$，右移补最高符号位

在位拼接时，注意指明字宽

### Code

``` verilog
module top_module(
    input clk,
    input load,
    input ena,
    input [1:0] amount,
    input [63:0] data,
    output reg [63:0] q); 
    
    always @(posedge clk) begin
        if(load) q <= data;
        else if(ena) begin
            case (amount)
                2'b00: q <= {q[62:0], 1'b0}; //q << 1
                2'b01: q <= {q[55:0], {8{1'b0}}}; //q << 8
                2'b10: q <= {q[63], q[63:1]}; //q >> 1
                2'b11: q <= {{8{q[63]}}, q[63:8]}; //q >> 8
                default: q <= q;
            endcase
        end
        else q <= q;
    end

endmodule
```
