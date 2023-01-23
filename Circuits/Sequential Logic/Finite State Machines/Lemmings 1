### Description
设计一个小人的运动状态

如果向右走，碰到障碍物向左走；如果向左走，碰到障碍物向右走

### Code

``` verilog

module top_module(
    input clk,
    input areset,    // Freshly brainwashed Lemmings walk left.
    input bump_left,
    input bump_right,
    output walk_left,
    output walk_right); //  

   	parameter left = 0, right = 1; 
    reg state, next_state;

    always @(posedge clk, posedge areset) begin
        if(areset) state <= left;
        else state <= next_state; 
    end
    
    always @(*) begin
        case (state)
            left: next_state = bump_left? right: left;
            right: next_state = bump_right? left: right;
        endcase
    end
    
    assign walk_left = (state == left);
    assign walk_right = !walk_left;

endmodule


```
