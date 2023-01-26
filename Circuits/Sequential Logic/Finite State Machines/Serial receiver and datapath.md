### Description
有一种传输协议，在等待状态时，当 $in = 0$ 时，开始传输 $8bit$ 直到 $in=1$，才进入等待状态。

存储每一字节的数据
### Code

``` verilog
module top_module(
    input clk,
    input in,
    input reset,    // Synchronous reset
    output [7:0] out_byte,
    output done
); 

    parameter idle = 0, start = 1, work = 2, stop = 3, error = 4;
    reg[3:1] state, next_state;
    reg[3:0] cnt = 0;
    
    always @(posedge clk) begin
        if(reset) begin
            state <= idle;
        end
        else begin
            state <= next_state;
        end
    end
    
    always @(posedge clk) begin
        if(reset) cnt <= 0;
        else begin
            if(next_state == work) begin
                out_byte[cnt] <= in;
                cnt <= cnt + 1;    
            end
            else cnt <= 0;
        end
    end
    
    always @(*) begin
        case (state) 
            idle: next_state = in? idle: start;
            start: begin
                next_state = work;
            end
            work: begin
                if(cnt < 8)
                    next_state = work;
                else if(cnt == 8)
                    next_state = in? stop: error;
            end
            stop:
                next_state = in? idle: start;
            error:
                next_state = in? idle: error; //
        endcase
    end
    
    assign done = (stop == state);
    
endmodule
```
