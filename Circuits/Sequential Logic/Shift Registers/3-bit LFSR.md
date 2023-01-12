### Description
按照图示设计时序电路

其中 $R = SW,\ clk = KEY_0,\ L = KEY_1, \ Q = LEDR$

也可以通过实例化$D$触发器解决问题

注意时序逻辑中使用非阻塞赋值

### Code

``` verilog
module top_module (
	input [2:0] SW,      // R
	input [1:0] KEY,     // L and clk
	output reg [2:0] LEDR);  // Q

    wire clk = KEY[0];
    wire L = KEY[1];
    wire [2:0] R = SW;
    always @(posedge clk) begin
        if(L == 0) begin
            LEDR[0] <= LEDR[2];
            LEDR[1] <= LEDR[0];
            LEDR[2] <= LEDR[2] ^ LEDR[1];
        end
        else begin
            LEDR[0] <= R[0];
            LEDR[1] <= R[1];
            LEDR[2] <= R[2];
        end
    end

endmodule
```
