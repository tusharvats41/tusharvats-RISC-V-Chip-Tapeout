
 Day 4 – GLS, Blocking vs Non-blocking, and Synthesis-Simulation Mismatch

 Table of Contents
1. [Introduction](#introduction)
2. [Why Gate Level Simulation (GLS) is Necessary](#why-gate-level-simulation-gls-is-necessary)
3. [Synthesis-Simulation Mismatches](#synthesis-simulation-mismatches)
   - [Missing Sensitivity List](#missing-sensitivity-list)
   - [Blocking vs Non-blocking Assignments](#blocking-vs-non-blocking-assignments)
   - [Non-standard Verilog Coding](#non-standard-verilog-coding)
4. [Labs and Examples](#labs-and-examples)
   - [Ternary Operator MUX](#ternary-operator-mux)
   - [Bad MUX](#bad-mux)
   - [Blocking Caveat](#blocking-caveat)
5. [Simulation, Synthesis, and GLS Commands](#simulation-synthesis-and-gls-commands)
6. [Summary](#summary)

---

## Introduction
This repository covers **Gate Level Simulation (GLS)**, the difference between **blocking (`=`) and non-blocking (`<=`) assignments**, and **synthesis-simulation mismatches** in Verilog. These concepts ensure:

- Correct functionality post-synthesis
- Timing-aware validation with delay annotation
- Prevention of subtle simulation-synthesis mismatches

---

## Why Gate Level Simulation (GLS) is Necessary
- RTL simulation does not consider gate delays
- GLS verifies functionality and timing after synthesis
- Essential for timing-critical designs

---

## Synthesis-Simulation Mismatches
Mismatches occur due to:

1. **Missing sensitivity list**
2. **Blocking vs non-blocking assignments**
3. **Non-standard Verilog coding**

### Missing Sensitivity List
**Bad Example:**
```verilog
always @(sel) begin
    if(sel) y <= i1;
    else y <= i0;
end
````

* Acts like a latch; `y` not updated when `i0`/`i1` change.

**Correct Example:**

```verilog
always @(*) begin
    if(sel) y <= i1;
    else y <= i0;
end
```

---

### Blocking vs Non-blocking Assignments

| Type         | Syntax | Behavior                                                                       |
| ------------ | ------ | ------------------------------------------------------------------------------ |
| Blocking     | `=`    | Executes sequentially in order; LHS updated immediately                        |
| Non-blocking | `<=`   | RHS evaluated first, LHS updated at end of timestep; allows parallel execution |

**Impact:** Using blocking in sequential logic can cause simulation to differ from synthesized design.

---

### Non-standard Verilog Coding

* Deviating from synthesizable Verilog causes mismatches.
* Examples: unintended latches, wrong always block sensitivity, wrong assignments.

---

## Labs and Examples

### Ternary Operator MUX

**Code:** `ternary_operator_mux.v`

```verilog
module ternary_operator_mux(
    input i0, input i1, input sel,
    output y
);
assign y = sel ? i1 : i0;
endmodule
```

### Bad MUX

**Code:** `bad_mux.v`

```verilog
module bad_mux(
    input i0, input i1, input sel,
    output reg y
);
always @(sel) begin
    if(sel) y <= i1;
    else y <= i0;
end
endmodule
```

### Blocking Caveat

**Code:** `blocking_caveat.v`

```verilog
module blocking_caveat(
    input a, input b, input c,
    output reg d
);
reg x;
always @(*) begin
    d = x & c;
    x = a | b;
end
endmodule
```

* HDL simulation gives old value of `x`, GLS shows correct current value → **synthesis-simulation mismatch**

---

## Simulation, Synthesis, and GLS Commands

### HDL Simulation

```bash
iverilog <module>.v <testbench>.v
./a.out
gtkwave <testbench>.vcd
```

### Synthesis

```bash
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog <module>.v
synth -top <module>
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog <module>_net.v
```

### Gate Level Simulation (GLS)

```bash
iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v <module>_net.v <testbench>.v
./a.out
gtkwave <testbench>.vcd
```

---

## Summary

* GLS ensures correctness post-synthesis with timing considerations
* Always use `always @(*)` for combinational logic
* Prefer non-blocking assignments (`<=`) for sequential logic
* HDL simulation may differ from GLS due to blocking statements or sensitivity issues

---

````

---

### **Optional Scripts for Automation**
**`scripts/run_hdl_sim.sh`**
```bash
#!/bin/bash
iverilog $1 $2
./a.out
gtkwave ${2%.v}.vcd
````

**`scripts/run_synthesis.sh`**

```bash
#!/bin/bash
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog $1
synth -top ${1%.v}
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog ${1%.v}_net.v
```

**`scripts/run_gls.sh`**

```bash
#!/bin/bash
iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v $1 $2
./a.out
gtkwave ${2%.v}.vcd
```

Summary
Day 4 covers **Gate Level Simulation (GLS)**, **blocking vs non-blocking assignments**, and **synthesis-simulation mismatches** in Verilog. GLS verifies post-synthesis correctness and timing. Mismatches arise from missing sensitivity lists, blocking assignments, or non-standard coding. Labs include ternary MUX, bad MUX, and blocking caveat, demonstrating differences between HDL simulation and synthesized behavior.
