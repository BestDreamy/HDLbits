### Description
通过 $bcdcount$ 实现范围为 $0-9999$ 的十进制计数器

$ena[1]$ ：十位数进位

$ena[2]$ ：百位数进位

$ena[3]$ ：千位数进位

### Code

``` verilog
module top_module (
    input clk,
    input reset,   // Synchronous active-high reset
    output [3:1] ena,
    output [15:0] q);

    bcdCount inst0(clk, reset, 1, q[3:0]);
    bcdCount inst1(clk, reset, ena[1], q[7:4]);
    bcdCount inst2(clk, reset, ena[2], q[11:8]);
    bcdCount inst3(clk, reset, ena[3], q[15:12]);
    
    assign ena[1] = (q[3:0] == 4'h9);
    assign ena[2] = (q[7:0] == 8'h99);
    assign ena[3] = (q[11:0] == 12'h999);
    
endmodule

module bcdCount(input clk, input reset, input enable, output [3:0] q);
    always @(posedge clk) begin
        if(reset) q <= '0;
        else if(enable) begin
            if(q == 4'b1001) q = '0;
            else q = q + 1;
        end
    end
endmodule
```
