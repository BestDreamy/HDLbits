### Description
设计一个小人的运动状态

如果向右走，碰到障碍物向左走；如果向左走，碰到障碍物向右走

当没有路面时，小人会处于下降状态，此时不会受障碍物的影响

当收到挖洞信号时，会一直保持挖洞状态，直到被打断

优先级：$ground > dig > walk$

### Code

``` verilog
module top_module(
    input clk,
    input areset,    // Freshly brainwashed Lemmings walk left.
    input bump_left,
    input bump_right,
    input ground,
    input dig,
    output walk_left,
    output walk_right,
    output aaah,
    output digging );

    parameter left = 6'b000001, right = 6'b000010, left_down = 6'b000100, right_down = 6'b001000, 
    		left_dig = 6'b010000, right_dig = 6'b100000; 
    reg [6:1] state, next_state;

    always @(posedge clk, posedge areset) begin
        if(areset) state <= left;
        else state <= next_state; 
    end
    
    always @(*) begin
        case (state)
            left: 
                if(ground) begin
                    if(dig) next_state = left_dig;
                    else next_state = bump_left? right: left;
                end
            	else
                    next_state = left_down;
            
            right: 
               	if(ground) begin
                    if(dig) next_state = right_dig;
                    else next_state = bump_right? left: right;
                end
            	else
                    next_state = right_down;
            
            left_down: next_state = ground? left: left_down;
            
            right_down: next_state = ground? right: right_down;
            
            left_dig: 
                if(ground)
                	next_state = left_dig;
            	else next_state = left_down;
            
            right_dig: 
                if(ground)
                    next_state = right_dig;
            	else next_state = right_down;
            
        endcase
    end
    
    assign walk_left = (state == left);
    assign walk_right = (state == right);
    assign aaah = (state == left_down) || (state == right_down);
    assign digging = (state == left_dig) || (state == right_dig);

endmodule
```
