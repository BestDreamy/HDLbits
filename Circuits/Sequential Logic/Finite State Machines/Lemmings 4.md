### Description
设计一个小人的运动状态

如果向右走，碰到障碍物向左走；如果向左走，碰到障碍物向右走

当没有路面时，小人会处于下降状态，此时不会受障碍物的影响

当收到挖洞信号时，会一直保持挖洞状态，直到被打断

优先级：$ground > dig > walk$

当小人下落时间超过 $20$ 时，将等待复活，所有输出全部置零

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

    parameter go_left = 4'd0, go_right = 4'd1, down_left = 4'd2, down_right = 4'd3, dig_left = 4'd4, dig_right = 4'd5, fail = 4'd6;
    reg [4:1] state, next_state;
    int count = 0;
    
    always @(posedge clk, posedge areset) begin
        if(areset) begin
            state <= go_left;
            count <= 0;
        end
        else begin
            state <= next_state; 
            if(state == down_left || state == down_right)
                count <= count + 1;
            else count <= 0;
        end
    end
    
    always @(*) begin
        case (state)
            go_left:
                if (ground) begin
                    if (dig) next_state = dig_left;
                    else next_state = bump_left? go_right: go_left;
                end
            	else
                    next_state = down_left;
            
            go_right:
                if (ground) begin
                    if (dig) next_state = dig_right;
                    else next_state = bump_right? go_left: go_right;
                end
            	else
                    next_state = down_right;
            
            dig_left:
                if (ground) next_state = dig_left;
            	else next_state = down_left;
            
            dig_right:
                if (ground) next_state = dig_right;
            	else next_state = down_right;
            
            down_left:
                if (ground) begin
                    if (count < 20) next_state = go_left;
            		else next_state = fail;
             	end 
            	else 
                    next_state = down_left;
            
            down_right:
                if (ground) begin
                    if (count < 20) next_state = go_right;
            		else next_state = fail;
                end
            	else
                    next_state = down_right;
        endcase
    end
            
    always @(*) begin
    	if (state == fail)
        	{walk_left, walk_right, aaah, digging} = 4'b0000;
        else begin
     		walk_left = (state == go_left);
            walk_right = (state == go_right);
            aaah = (state == down_left) || (state == down_right);
            digging = (state == dig_left) || (state == dig_right);        
		end
	end        
    
endmodule
```
