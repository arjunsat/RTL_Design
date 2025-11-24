
# Create a one-bit wide, 2-to-1 multiplexer. When sel=0, choose a. When sel=1, choose b.

<img width="392" height="320" alt="image" src="https://github.com/user-attachments/assets/9046ae91-1427-4e94-aada-7ca10a45b44d" />


# Create a 100-bit wide, 2-to-1 multiplexer. When sel=0, choose a. When sel=1, choose b.

<img width="340" height="226" alt="image" src="https://github.com/user-attachments/assets/fa31522b-3b5a-4774-89c4-b6dd76a6d2ac" />


#
<img width="1411" height="483" alt="image" src="https://github.com/user-attachments/assets/76f20242-4e55-4460-adcd-356a049ed5b4" />

# Create a 16-bit wide, 9-to-1 multiplexer. sel=0 chooses a, sel=1 chooses b, etc. For the unused cases (sel=9 to 15), set all output bits to '1'

#

<img width="607" height="695" alt="image" src="https://github.com/user-attachments/assets/2c69c5c6-c960-4f1a-a143-734889bc80d9" />


# Procedural Block
Procedural Blocks in Verilog — When to Use and When Not to Use
🔹 Introduction

Verilog provides two major ways to describe hardware:

Continuous assignments (assign) → direct combinational logic

Procedural blocks (always, initial, functions, tasks) → behavioral logic

Knowing when to use which is essential for clean, synthesizable RTL.

✅ Where to USE Procedural Blocks
1. Sequential Logic (Flip-Flops, Registers)

Use always @(posedge clk) or @(negedge clk).

always @(posedge clk) begin
    q <= d;
end


✔ Required for registers
✔ Used in FSM state registers
❌ Cannot be done using assign

2. Complex Combinational Logic (if-else, case)

Use always @(*) when logic depends on branching.

always @(*) begin
    case(sel)
        0: out = a;
        1: out = b;
        default: out = 0;
    endcase
end


✔ Good for multiplexers
✔ Good for encoders/decoders
✔ Good for next-state logic

3. Multi-line Combinational Calculations

Use an always block when more than one line is needed.

always @(*) begin
    temp = a + b;
    out  = temp ^ c;
end

4. State Machines (FSMs)

Two blocks are required:

// Sequential — registers
always @(posedge clk)
    state <= next_state;

// Combinational — next state logic
always @(*)
    case(state)


✔ Required for every FSM

❌ Where NOT to Use Procedural Blocks
1. Simple Combinational Logic

Use continuous assignment (assign), not always.

assign out = a & b;
assign sum = x + y;


❌ Do not wrap simple expressions in an always block.

2. Tri-state Buffers

Use assign, not always.

assign bus = en ? data : 1'bz;

3. Simulation-Only Code (initial)

initial blocks do not synthesize into hardware.

initial begin
    counter = 0;   // ❌ not synthesizable
end


Use only in testbenches.

4. Mixing Blocking and Non-Blocking Wrongly

Correct usage:

Sequential logic: non-blocking (<=)

Combinational logic: blocking (=)

⭐ Summary Table
Operation / Goal	Use assign	Use always @(*)	Use always @(posedge clk)
Simple combinational logic	✔	❌	❌
Case / if-else logic	❌	✔	❌
Multi-line combinational	❌	✔	❌
Registers, counters	❌	❌	✔
FSM state registers	❌	❌	✔
FSM next-state logic	❌	✔	❌
Testbench initialization	❌	❌	initial only
✅ Conclusion

Procedural blocks are powerful when used correctly.
Use them for behavioral, multi-step, conditional, or sequential logic.
Use assign for simple, direct wiring.![Uploading image.png…]()







