### Description
独热码模板

同步复位，复位至状态 $A$

### Code

``` verilog

module top_module(
    input clk,
    input in,
    input reset,
    output out); //

    parameter A = 4'b0001, B = 4'b0010, C = 4'b0100, D = 4'b1000;
    reg [3:0] state, next_state;
    
    always @(posedge clk) begin
        if(reset) state <= A;
        else state <= next_state;
    end
    
    always @(*) begin
        case (state)
            A: next_state = in? B: A;
            B: next_state = in? B: C;
            C: next_state = in? D: A;
            D: next_state = in? B: C;
        endcase
    end
    
    assign out = (state == D);
    
endmodule

```
