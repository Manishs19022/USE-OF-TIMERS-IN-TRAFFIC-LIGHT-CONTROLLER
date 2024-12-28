# USE-OF-TIMERS-IN-TRAFFIC-LIGHT-CONTROLLER
Traffic light controllers play a critical role in managing the flow of vehicles and pedestrians at  intersections, ensuring safety and efficiency. Timers are integral components of these systems,  providing the necessary control and coordination to manage traffic signal transitions effectively.


Traffic congestion is a growing problem in urban areas, leading to delays, pollution, and safety 
concerns. Traffic light controllers equipped with timers are fundamental in mitigating these issues by regulating the flow of vehicles and pedestrians. Timers manage the duration of signal phases, operating based on pre-configured intervals or adaptive algorithms.


# Objectives: 
The objectives of this study are: 

1. To evaluate the limitations of existing timer systems in traffic light controllers. 

2. To develop a model integrating adaptive timers for optimized traffic flow. 

3. To assess the proposed system's performance against existing models through simulation

# Block Diagram:

![image](https://github.com/user-attachments/assets/c464f823-54fa-40af-b93a-4473b4af6367)



# Comparison Table: 

![image](https://github.com/user-attachments/assets/8fa97d4d-01de-4853-a711-e039a4e769ca)

# Verilog Code:


 `timescale 1ns / 1ps 
module traffic_light_controller( 

    output reg [2:0] n_lights, // North lights 
    output reg [2:0] s_lights, // South lights 
    output reg [2:0] e_lights, // East lights 
    output reg [2:0] w_lights, // West lights 
    input clk,                 // Clock signal 
    input rst_n                // Reset signal (active low) 
    ); 
    
    parameter [2:0] north_green = 3'b001; // North green 
    
    parameter [2:0] north_yellow = 3'b010; // North yellow 
    parameter [2:0] south_green = 3'b100; // South green 
    parameter [2:0] south_yellow = 3'b101; // South yellow 
    parameter [2:0] east_green = 3'b110; // East green 
    parameter [2:0] east_yellow = 3'b111; // East yellow 
    parameter [2:0] west_green = 3'b000; // West green 
    parameter [2:0] west_yellow = 3'b011; // West yellow 
    reg [2:0] state; // Current state 
 
    reg [3:0] timer; // Timer for light duration 
    always @(posedge clk or negedge rst_n) begin 
        if (!rst_n) begin 
            state <= north_green; // Start with north green 
            timer <= 0;           // Reset timer 
        end else begin 
            case (state) 
                north_green: begin 
                    n_lights <= north_green; 
                    s_lights <= 3'b100; // South red 
                    e_lights <= 3'b100; // East red 
                    w_lights <= 3'b100; // West red 
                    if (timer == 4'd8) begin // Green for 8 clock cycles 
                        state <= north_yellow; 
                        timer <= 0; 
                    end else begin 
                        timer <= timer + 1; 
                    end 
                end 
 
                north_yellow: begin 
                    n_lights <= north_yellow; 
                    s_lights <= 3'b100; // South red 
                    e_lights <= 3'b100; // East red 
                    w_lights <= 3'b100; // West red 
                    if (timer == 4'd4) begin // Yellow for 4 clock cycles 
                        state <= south_green; 
                        timer <= 0; 
                    end else begin 
                        timer <= timer + 1; 
                    end 
                      end 
 
                south_green: begin 
                    n_lights <= 3'b100; // North red 
                    s_lights <= south_green; 
                    e_lights <= 3'b100; // East red 
                    w_lights <= 3'b100; // West red 
                    if (timer == 4'd8) begin // Green for 8 clock cycles 
                        state <= south_yellow; 
                        timer <= 0; 
                    end else begin 
                        timer <= timer + 1; 
                    end 
                end 
 
                south_yellow: begin 
                    n_lights <= 3'b100; // North red 
                    s_lights <= south_yellow; 
                    e_lights <= 3'b100; // East red 
                    w_lights <= 3'b100; // West red 
                    if (timer == 4'd4) begin // Yellow for 4 clock cycles 
                        state <= east_green; 
                        timer <= 0; 
                    end else begin 
                        timer <= timer + 1; 
                    end 
                end 
 
                east_green: begin 
                    n_lights <= 3'b100; // North red 
                    s_lights <= 3'b100; // South red 
                    e_lights <= east_green; 
                    w_lights <= 3'b100; // West red 
                    if (timer == 4'd8) begin // Green for 8 clock cycles 
                        state <= east_yellow; 
                        timer <= 0; 
                    end else begin 
                        timer <= timer + 1; 
                    end 
                end 
 
                east_yellow: begin 
                    n_lights <= 3'b100; // North red 
                    s_lights <= 3'b100; // South red 
                    e_lights <= east_yellow; 
                    w_lights <= 3'b100; // West red 
                    if (timer == 4'd4) begin // Yellow for 4 clock cycles 
                        state <= west_green; 
                        timer <= 0; 
                    end else begin 
                        timer <= timer + 1; 
                    end 
                end 
                 
                west_green: begin 
                    n_lights <= 3'b100; // North red 
                    s_lights <= 3'b100; // South red 
                    e_lights <= 3'b100; 
                    w_lights <= west_green;  
                    if (timer == 4'd8) begin // Green for 8 clock cycles 
                        state <= west_yellow; 
                        timer <= 0; 
                    end else begin 
                        timer <= timer + 1; 
                         end 
                end 
 
                east_yellow: begin 
                    n_lights <= 3'b100; // North red 
                    s_lights <= 3'b100; // South red 
                    e_lights <=  
                    w_lights <= 3'b100; // West red 
                    if (timer == 4'd4) begin // Yellow for 4 clock cycles 
                        state <= north_green; 
                        timer <= 0; 
                    end else begin 
                        timer <= timer + 1; 
                    end 
                end 
            endcase 
        end 
    end 
    endmodule 

# Test Bench:


`timescale 1ns / 1ps 

module tb_traffic_light_controller; 

    reg clk;                 // Clock signal 
    reg rst_n;              // Reset signal (active low) 
    wire [2:0] n_lights;    // North lights output 
    wire [2:0] s_lights;    // South lights output 
    wire [2:0] e_lights;    // East lights output 
    wire [2:0] w_lights;    // West lights output 
    traffic_light_controller uut ( 
        .n_lights(n_lights), 
        .s_lights(s_lights), 
         .e_lights(e_lights), 
        .w_lights(w_lights), 
        .clk(clk), 
        .rst_n(rst_n) 
    ); 
    initial begin 
        clk = 0; 
        forever #5 clk = ~clk; // Toggle clock every 5 ns 
    end 
    initial begin 
        rst_n = 0; // Assert reset 
        #10;       // Wait for 10 ns 
        rst_n = 1; // Deassert reset 
    end 
    initial begin 
        $monitor("Time: %0t | n_lights: %b | s_lights: %b | e_lights: %b | w_lights: %b",  
                 $time, n_lights, s_lights, e_lights, w_lights); 
    end 
    initial begin 
        #1000; // Run for 1000 ns 
        $finish; // End the simulation 
    end 
    endmodule



# Result:

![image](https://github.com/user-attachments/assets/7df7e745-a4c4-412c-9161-d7625426af7c)

# Conclusion: 
Timers are indispensable in traffic light controllers, offering control and adaptability to manage intersections efficiently. 

The proposed adaptive timer system addresses the limitations of 
traditional methods, providing significant improvements in traffic flow, safety, and sustainability.

Future work includes integrating AI-driven predictive algorithms to further enhance the 
capabilities of timer-based traffic systems. Detailed discussions and extended simulations 
substantiate these conclusions, aligning with the objectives of smart city initiatives. 











