### Description
设计一个 $12$ 小时的时钟，范围为 $12:00:00-11:59:59$

每 $12$ 个小时, $AM$ 切换为 $PM$， $PM$ 切换为 $AM$

当复位信号来临时，时间变为 $12:00:00AM$

### Code

``` verilog
module top_module(
    input clk,
    input reset,
    input ena,
    output pm,
    output [7:0] hh,
    output [7:0] mm,
    output [7:0] ss); 

    wire [4:0] enable;
    assign enable[0] = ena;
    assign enable[1] = enable[0] && ss[3:0] == 4'h9;
    assign enable[2] = enable[0] && ss[7:0] == 8'h59;
    assign enable[3] = enable[0] && {mm[3:0], ss[7:0]} == 12'h959;
    assign enable[4] = enable[0] && {mm[7:0], ss[7:0]} == 16'h5959;
    // assign enable[5] = enable[0] && {hh[3:0], mm[7:0], ss[7:0]} ==  20'h95959;
        
    bcdCount #(.s(0), .e(9)) ss0(clk, reset, enable[0], ss[3:0]);
    bcdCount #(.s(0), .e(5)) ss1(clk, reset, enable[1], ss[7:4]);
    
    bcdCount #(.s(0), .e(9)) mm0(clk, reset, enable[2], mm[3:0]);
    bcdCount #(.s(0), .e(5)) mm1(clk, reset, enable[3], mm[7:4]);
    
    reg [4:0] hTemp, h;
    fiveBitCount realHour(clk, reset, enable[4], hTemp[4:0]);
    // assign hh[7:0] = 8'h12;
    
    always @(*) begin
        if(hTemp <= 11) pm = 0;
        else pm = 1;
       
        if(pm == 1) begin
            h = hTemp - 12; //控制范围在 [0, 11]
        end
        else begin
            h = hTemp;
        end
        
        if(h == 5'd0) begin
            hh[7:0] = 8'h12;
        end
        else if(h == 5'd11) begin
            hh[7:0] = 8'h11;
        end
        else if(h == 5'd10) begin
            hh[7:0] = 8'h10;
        end
        else begin
            hh[7:0] = {4'h0, h[3:0]};
        end
        
    end
    
endmodule

module bcdCount(input clk, input reset, input enable, output [3:0] q);
    parameter s, e;
    always @(posedge clk) begin
        if(reset) q <= s;
        else if(enable) begin
            if(q == e) q <= s;
            else q <= q + 1;
        end
    end
endmodule

module fiveBitCount(input clk, input reset, input enable, output reg [4:0] q);
    always @(posedge clk) begin
        if(reset) q <= '0;
        else if(enable) begin
            if(q == 5'd23) q <= 0;
            else q <= q + 1;
        end
    end
endmodule

```
