
# Create a one-bit wide, 2-to-1 multiplexer. When sel=0, choose a. When sel=1, choose b.

<img width="392" height="320" alt="image" src="https://github.com/user-attachments/assets/9046ae91-1427-4e94-aada-7ca10a45b44d" />


# Create a 100-bit wide, 2-to-1 multiplexer. When sel=0, choose a. When sel=1, choose b.

<img width="340" height="226" alt="image" src="https://github.com/user-attachments/assets/fa31522b-3b5a-4774-89c4-b6dd76a6d2ac" />


#
<img width="1411" height="483" alt="image" src="https://github.com/user-attachments/assets/76f20242-4e55-4460-adcd-356a049ed5b4" />

# Create a 16-bit wide, 9-to-1 multiplexer. sel=0 chooses a, sel=1 chooses b, etc. For the unused cases (sel=9 to 15), set all output bits to '1'

#

<img width="607" height="695" alt="image" src="https://github.com/user-attachments/assets/2c69c5c6-c960-4f1a-a143-734889bc80d9" />

#

<img width="1309" height="1061" alt="image" src="https://github.com/user-attachments/assets/eadf4405-fff3-493b-beb5-107fe45d566c" />

#
<img width="727" height="792" alt="image" src="https://github.com/user-attachments/assets/8b15e58b-e7c8-460a-8673-d4a8a59e9a92" />




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

<img width="978" height="686" alt="image" src="https://github.com/user-attachments/assets/a8c8c4b7-c0ad-4ade-961a-18f9ebb6a6d8" />

✅ Conclusion

Procedural blocks are powerful when used correctly.
Use them for behavioral, multi-step, conditional, or sequential logic.
Use assign for simple, direct wiring.<img width="1001" height="451" alt="image" src="https://github.com/user-attachments/assets/28822f50-97ad-47d5-8cef-1ab6d23bc684" />

# CASE staement

No — a case statement does NOT always need to be inside an always block.

It depends on what you are trying to build.

✅ 1. Case inside always @(*) (most common)

Used for combinational logic when assigning to reg signals.

Example:
always @(*) begin
    case(sel)
        0: out = a;
        1: out = b;
        default: out = 0;
    endcase
end

This is the standard way because case must drive a reg, and regs can only be written in procedural blocks (always, initial).

✅ 2. Case inside always @(posedge clk)

Used for sequential logic, like state machines.

Example:
always @(posedge clk) begin
    case(state)
        S0: state <= S1;
        S1: state <= S2;
    endcase
end
✅ 3. Case inside a function (allowed)
function [3:0] priority(input [3:0] x);
    case(x)
        4'b1000: priority = 0;
        4'b0100: priority = 1;
    endcase
endfunction


Functions behave like combinational logic.

❌ Case outside any procedural block is NOT allowed

You CANNOT use:

case(sel)
    0: out = a;
endcase


This is illegal because Verilog only allows case statements inside procedural regions (always, initial, functions, tasks).
#


<img width="840" height="334" alt="image" src="https://github.com/user-attachments/assets/b72d6623-a85e-4762-b04c-142e6d24bc93" />

#

<img width="764" height="482" alt="image" src="https://github.com/user-attachments/assets/e957a711-3dd6-4815-ad23-dc276385d414" />

#

<img width="1385" height="494" alt="image" src="https://github.com/user-attachments/assets/614f073b-f0d3-43dc-858a-6b728891060d" />


# Concept of vectors

<img width="1195" height="242" alt="image" src="https://github.com/user-attachments/assets/c5341af4-eaa8-42ce-8db0-e67c48c5f06a" />

# Packed vs Unpacked Arrays + SRAM Modeling (Quick Notes)

## 1) Packed Arrays
**Packed ranges are written to the *left* of the variable name.**  
They define the **bit-width of each element** and behave like a single vector/bus.

Example:
```verilog
reg [7:0] data;   // packed [7:0] => one 8-bit register











