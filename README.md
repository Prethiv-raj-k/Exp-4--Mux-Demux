# Exp-4-Mux-Demux
# Design and Verification of 2:1 Multiplexer and 1:2 Demultiplexer
## Aim
To design and verify a 2:1 Multiplexer and a 1:2 Demultiplexer using Verilog HDL.
## Theory / Synopsis
A 2:1 Multiplexer (MUX) selects one of two data inputs, A or B, and routes it to a single output Y based on the value of the select line S. A 1:2 Demultiplexer (DEMUX) performs the reverse operation: it routes a single data input D to one of two outputs, Y0 or Y1, based on the select line S.
Boolean Functions
### 2:1 Multiplexer:
•	Y = S'.A + S.B
### 1:2 Demultiplexer:
•	Y0 = S'.D
•	Y1 = S.D
## Files to be Created

<img width="575" height="65" alt="image" src="https://github.com/user-attachments/assets/04c2d90c-0b6b-4b35-b603-d0e10e1e4f00" />

## Design / RTL Program
```
module shift_reg (
    input  wire       clk,   // clock
    input  wire       rst,   // asynchronous active-high reset
    input  wire       sin,   // serial input
    output wire       sout,  // serial output (MSB, about to shift out)
    output reg  [3:0] Q      // parallel output
);
    always @(posedge clk or posedge rst) begin
        if (rst)
            Q <= 4'b0000;
        else
            Q <= {Q[2:0], sin};   // shift left, sin enters at LSB
    end
 
    assign sout = Q[3];
endmodule
```

## **Testbench Program
module tb6;
    reg  clk, rst, sin;
    wire sout;
    wire [3:0] Q;
 
    // Instantiate the shift register
    shift_reg uut (
        .clk(clk),
        .rst(rst),
        .sin(sin),
        .sout(sout),
        .Q(Q)
    );
 
    // ---- Clock generation: 10 time-unit period ----
    initial clk = 0;
    always #5 clk = ~clk;
 
    initial begin
        // ---- VCD dump setup ----
        $dumpfile("shift_reg.vcd");   // name of the VCD file to be generated
        $dumpvars(0, tb6);              // dump all signals in this testbench hierarchy
 
        $monitor("Time=%0t rst=%b clk=%b sin=%b | Q=%b sout=%b",
                  $time, rst, clk, sin, Q, sout);
 
        // ---- Apply asynchronous reset ----
        rst = 1; sin = 0;
        #7  rst = 0;
 
        // ---- Serially shift in the bit pattern 1 0 1 1 ----
        @(negedge clk); sin = 1;
        @(negedge clk); sin = 0;
        @(negedge clk); sin = 1;
        @(negedge clk); sin = 1;
 
        // ---- Hold sin = 0 and observe the pattern shift out ----
        @(negedge clk); sin = 0;
        @(negedge clk);
        @(negedge clk);
 
        // ---- Apply reset again mid-simulation ----
        @(negedge clk); rst = 1;
        @(negedge clk); rst = 0; sin = 1;
 
        #20 $finish;
    end
endmodule


## Truth Table

### 2:1 Multiplexer

<img width="573" height="154" alt="image" src="https://github.com/user-attachments/assets/96dd3fa2-7747-402d-b88f-17c11e28fcbb" />

### 1:2 Demultiplexer

<img width="583" height="90" alt="image" src="https://github.com/user-attachments/assets/4bc21b52-1db5-4302-afab-ebb0b9d5a74b" />

## Simulation Procedure

STEP 1 – Open Terminal
Open a terminal in the experiment folder.

STEP 2 – Load Synopsys Environment
source /synopsys/start.sh

STEP 3 – Compile Using VCS
vcs mux_demux.v mux_demux_tb.v -full64
If compilation is successful, VCS generates the simulation executable:
simv

STEP 4 – Run Simulation
./simv
The terminal displays the input combinations and corresponding outputs Y, Y0, and Y1.
A VCD waveform file is also generated:
mux_demux.vcd

STEP 5 – Open DVE

dve -full64
Other option
dve -full64 &
A DVE environment will open.
DVE Waveform Verification
In DVE:
•	Open the testbench hierarchy.
•	Locate the signals:
o	A
o	B
o	S
o	Y
o	D
o	Y0
o	Y1
•	Add the signals to the waveform window.
•	Run/inspect the waveform.
•	Verify that Y = A whenever S = 0, and Y = B whenever S = 1.
•	Verify that Y0 = D and Y1 = 0 whenever S = 0, and Y1 = D and Y0 = 0 whenever S = 1.
•	Confirm that Y0 and Y1 are never both high at the same time, since only one output is active per select value.
The waveform should agree with the truth table.

## Expected Result

<img width="760" height="683" alt="Screenshot 2026-11-23 095329" src="https://github.com/user-attachments/assets/8bf9dff7-b52e-4af9-b311-ef2b874f439e" />


## Output
<img width="1915" height="935" alt="Screenshot 2026-11-23 100022" src="https://github.com/user-attachments/assets/7ee4d459-a70a-4640-97be-e11e7a856549" />


## Viva-Voce Questions


## Result

Thus, the 2:1 Multiplexer and 1:2 Demultiplexer were designed, implemented using Verilog HDL, successfully simulated using Synopsys VCS, and verified using Synopsys DVE.



