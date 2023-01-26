### Description
对于独热码 $s1 = 0001, s2 = 0010, s3 = 0100, s4 = 1000$ 来说

- 可以根据高电平所在的位置区分出 $s1, s2, s3, s4$
- 当 $in=1$ 状态机会从 $s1$ 跳到 $s2$ 时,
- 采用格雷码 $state = (state == 4'b0001)\  \\& \  in? 4'b0010: 4'b0001;$    
- 采用独热码 $state_1 = state_0\  \\& \ in;$

### Code

``` verilog
module top_module(
    input in,
    input [9:0] state,
    output [9:0] next_state,
    output out1,
    output out2); 
    
    assign next_state[0] = (state[0] & !in) | (state[1] & !in) | 
        (state[2] & !in) | (state[3] & !in) | (state[4] & !in) |
        (state[7] & !in) | (state[8] & !in) | (state[9] & !in);
    assign next_state[1] = (state[0] & in) | (state[8] & in) |
        (state[9] & in);
    assign next_state[2] = state[1] & in;
    assign next_state[3] = state[2] & in;
    assign next_state[4] = state[3] & in;
    assign next_state[5] = state[4] & in;
    assign next_state[6] = state[5] & in;
    assign next_state[7] = (state[6] & in) | (state[7] & in);
    assign next_state[8] = state[5] & !in;
    assign next_state[9] = state[6] & !in;
    
    assign out1 = (state[8]) || (state[9]);
    assign out2 = (state[7]) || (state[9]);
            
endmodule

```
