Day 3 – Sequential Circuit Optimization

 Table of Contents

1. [Introduction](#1-introduction)
2. [Basic Sequential Circuit](#2-basic-sequential-circuit)
3. [Optimization Techniques](#3-optimization-techniques)

   1. [Combinational Logic Optimization](#31-combinational-logic-optimization)
   2. [Sequential Logic Optimization](#32-sequential-logic-optimization)

      * [Sequential Constant Propagation](#a-sequential-constant-propagation)
      * [State Optimization](#b-state-optimization)
      * [Cloning](#c-cloning-physical-aware-optimization)
      * [Retiming](#d-retiming)
4. [HDL Simulation & Synthesis Commands](#4-hdl-simulation--synthesis-commands)
5. [Summary](#5-summary)

---

## 1. Introduction

Sequential circuits are digital logic circuits that use memory elements (like flip-flops) to store past information and influence current output. Unlike combinational circuits, the output depends on both current inputs and previous states.

Optimizing sequential circuits improves **speed**, **area**, and **power efficiency**.

---

## 2. Basic Sequential Circuit

A simple D flip-flop with asynchronous reset:

| Signal | Function                                    |
| ------ | ------------------------------------------- |
| RST    | Clears the output to a known state (0 or 1) |
| CLK    | Controls timing of state changes            |

**D Flip-Flop behavior:**

```
Q_next = D
If reset = 1 → Q = 0
Else → Q follows D at rising clock edge
```

---

## 3. Optimization Techniques

### 3.1 Combinational Logic Optimization

* **Constant propagation:** Replace signals with constants when possible.
* **Boolean logic simplification:** Minimize logic using K-map or algebraic methods.

**Examples:**

| Original Expression     | Simplified Expression |   |
| ----------------------- | --------------------- | - |
| y = a ? b : 0           | y = a & b             |   |
| y = a ? 1 : b           | y = a                 | b |
| y = a ? (c ? b : 0) : 0 | y = a & b & c         |   |

---

### 3.2 Sequential Logic Optimization

#### a) Sequential Constant Propagation

* Flip-flops with grounded or tied inputs can sometimes be simplified.
* **Limitation:** Cannot apply to flip-flops with asynchronous set/reset signals.

**Example:**

```verilog
always @(posedge clk, posedge reset)
begin
  if(reset)
    q <= 1'b0;
  else
    q <= 1'b1;
end
```

Optimization cannot be applied due to asynchronous reset.

---

#### b) State Optimization

* Remove unused or redundant states in FSMs or counters.
* Example: 3-bit up-counter using only `count[0]` → 1 flip-flop needed after optimization.

---

#### c) Cloning (Physical-aware Optimization)

* Duplicate flip-flops near combinational logic to reduce routing delays.
* Improves timing and overall performance.

---

#### d) Retiming

* Redistribute flip-flops without changing logic.
* Balances delays across stages to improve clock frequency.

**Example:**

| Path Delay                            | Frequency                          |
| ------------------------------------- | ---------------------------------- |
| Original: 5 ns                        | 200 MHz                            |
| Retimed: Stage1 = 3 ns, Stage2 = 2 ns | Still 200 MHz, but timing balanced |

---

## 4. HDL Simulation & Synthesis Commands

### Simulation

```bash
iverilog dff_const3.v tb_dff_const3.v
./a.out
gtkwave tb_dff_const3.vcd
```

### Synthesis

```bash
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_const3.v
synth -top dff_const3
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```

---

## 5. Summary

| Optimization Technique       | Purpose                            | Key Benefit                                      |
| ---------------------------- | ---------------------------------- | ------------------------------------------------ |
| State Optimization           | Remove unused/redundant states     | Reduces flip-flops & logic, saves area and power |
| Cloning                      | Duplicate registers near logic     | Reduces routing delay, improves timing           |
| Retiming                     | Redistribute flip-flops            | Balances delays, improves clock frequency        |
| Combinational Logic          | Simplify logic expressions         | Reduces gates and delays                         |
| Sequential Const Propagation | Simplify flip-flops with constants | Saves resources, reduces complexity              |


