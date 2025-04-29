# 4-bit-Ripple-Carry-Adder-using-Task-and-4-bit-Ripple-Counter-using-Function-with-Testbench
## Aim:
To design and simulate a 4-bit Ripple Carry Adder using Verilog HDL with a task to implement the full adder functionality and verify its output using a testbench.
To design and simulate a 4-bit Ripple Counter using Verilog HDL with a function to calculate the next state and verify its functionality using a testbench.

## Apparatus Required:
Computer with Vivado or any Verilog simulation software.
Verilog HDL compiler.

### Verilog Code
```verilog
module ripple_4bit_adder(
    input [3:0] A,
    input [3:0] B,
    input Cin,
    output [3:0] Sum,
    output Cout
);
    reg [3:0] sum_temp;
    reg carry;
    integer i;

    assign Sum = sum_temp;
    assign Cout = carry;

    task full_adder;
        input a, b, cin;
        output sum, cout;
        begin
            sum = a ^ b ^ cin;
            cout = (a & b) | (b & cin) | (a & cin);
        end
    endtask

    always @(*) begin
        carry = Cin;
        for (i = 0; i < 4; i = i + 1) begin
            full_adder(A[i], B[i], carry, sum_temp[i], carry);
        end
    end

endmodule

```
### Simulated Output
![Screenshot 2025-04-12 140139](https://github.com/user-attachments/assets/6e831d83-7ab9-4d5a-9cc1-82a8e2ffb313)


// Test bench for Ripple carry adder
```verilog
module ripple_4bit_adder_tb;

    // Inputs
    reg [3:0] A;
    reg [3:0] B;
    reg Cin;

    // Outputs
    wire [3:0] Sum;
    wire Cout;

    // Instantiate the Unit Under Test (UUT)
    ripple_4bit_adder uut (
        .A(A),
        .B(B),
        .Cin(Cin),
        .Sum(Sum),
        .Cout(Cout)
    );

    initial begin
        // Monitor the values
        $monitor("Time=%0t : A=%b B=%b Cin=%b | Sum=%b Cout=%b", $time, A, B, Cin, Sum, Cout);

        // Initialize Inputs
        A = 0; B = 0; Cin = 0;

        // Test cases
        #10 A = 4'b0001; B = 4'b0010; Cin = 0;  // 1 + 2 = 3
        #10 A = 4'b0101; B = 4'b0011; Cin = 0;  // 5 + 3 = 8
        #10 A = 4'b1111; B = 4'b0001; Cin = 0;  // 15 + 1 = 16
        #10 A = 4'b1001; B = 4'b0110; Cin = 0;  // 9 + 6 = 15
        #10 A = 4'b1111; B = 4'b1111; Cin = 1;  // 15 + 15 + 1 = 31
        #10 A = 4'b0000; B = 4'b0000; Cin = 1;  // 0 + 0 + 1 = 1
        #10;

        $finish;
    end

endmodule
```
### Simulated Output
![Screenshot 2025-04-12 143145](https://github.com/user-attachments/assets/c3a420f4-6222-4999-901f-6cbfff2864b0)
 Output


// Verilog Code ripple counter

```verilog
module ripple_counter_4bit (
    input clk,           // Clock signal
    input reset,         // Reset signal
    output reg [3:0] Q   // 4-bit output for the counter value
);

    // Function to calculate next state
    function [3:0] next_state;
        input [3:0] curr_state;
        begin
            next_state = curr_state + 1;
        end
    endfunction

    // Sequential logic for counter
    always @(posedge clk or posedge reset) begin
        if (reset)
            Q <= 4'b0000;       // Reset the counter to 0
        else
            Q <= next_state(Q); // Increment the counter
    end

endmodule
```
### Simulated Output
![Screenshot 2025-04-29 093154](https://github.com/user-attachments/assets/3591eb7a-88d2-4387-858c-c39371fed733)


// TestBench
```verilog
module ripple_counter_4bit_tb;

    reg clk;
    reg reset;
    wire [3:0] Q;

    // Instantiate the ripple counter
    ripple_counter_4bit uut (
        .clk(clk),
        .reset(reset),
        .Q(Q)
    );

    // Clock generation (10ns period)
    always #5 clk = ~clk;

    initial begin
        // Initialize inputs
        clk = 0;
        reset = 1;

        // Hold reset for 20ns
        #20 reset = 0;

        // Run simulation for 200ns
        #200 $stop;
    end

    initial begin
        $monitor("Time = %0t | Reset = %b | Q = %b", $time, reset, Q);
    end

endmodule
```
### Simulated Output
![Screenshot 2025-04-29 093620](https://github.com/user-attachments/assets/dd36b80f-7a7b-442c-a578-082ad975ac99)


Conclusion:
The 4-bit Ripple Carry Adder was successfully designed and implemented using Verilog HDL with the help of a task for the full adder logic. The testbench verified that the ripple carry adder correctly computes the 4-bit sum and carry-out for various input combinations. The simulation results matched the expected outputs.

The 4-bit Ripple Counter was successfully designed and implemented using Verilog HDL. A function was used to calculate the next state of the counter.

