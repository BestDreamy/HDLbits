### Description
独热码模板

异步复位，复位至状态 $A$

### Code

``` verilog
module top_module(
    input clk,
    input in,
    input areset,
    output out); //
    
    wire [3:0] A = 4'b0001, B = 4'b0010, C = 4'b0100, D = 4'b1000;
    reg [3:0] state, next_state;
    
    always @(posedge clk, posedge areset) begin
        if(areset) state <= A;
        else state <= next_state;
    end
    
    always @(*) begin
        case (state)
            A: next_state = !in? A: B;
            B: next_state = !in? C: B;
            C: next_state = !in? A: D;
            D: next_state = !in? C: B;
        endcase
    end
    
    assign out = state == D;
            
endmodule

```
