### Description
设计一个小人的运动状态

如果向右走，碰到障碍物向左走；如果向左走，碰到障碍物向右走

当没有路面时，小人会处于下降状态，此时不会受障碍物的影响

### Code

``` verilog

module top_module(
    input clk,
    input areset,    // Freshly brainwashed Lemmings walk left.
    input bump_left,
    input bump_right,
    input ground,
    output walk_left,
    output walk_right,
    output aaah ); 

    parameter left = 2'b00, right = 2'b01, left_down = 2'b10, right_down = 2'b11; 
    reg [1:0] state, next_state;

    always @(posedge clk, posedge areset) begin
        if(areset) state <= left;
        else state <= next_state; 
    end
    
    always @(*) begin
        case (state)
            left: 
                if(ground) 
                    next_state = bump_left? right: left;
            	else
                    next_state = left_down;
            
            right: 
                if(ground) 
                    next_state = bump_right? left: right;
            	else 
                    next_state = right_down;
            
            
            left_down: next_state <= ground? left: left_down;
            
            right_down: next_state <= ground? right: right_down;
            
        endcase
    end
    
    assign walk_left = (state == left);
    assign walk_right = (state == right);
    assign aaah = state == left_down || state == right_down;
    
endmodule


```
