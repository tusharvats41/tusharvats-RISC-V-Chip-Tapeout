Day 2 – Timing Libraries, Hierarchical vs Flat Synthesis & Efficient Flop Coding Styles

 Table of Contents
1. [Timing Libraries (`.lib`)](#timing-libraries-lib)
2. [Hierarchical vs Flat Synthesis](#hierarchical-vs-flat-synthesis)
   - [Hierarchical Synthesis](#hierarchical-synthesis)
   - [Flat Synthesis](#flat-synthesis)
3. [Sub-Module Level Synthesis](#sub-module-level-synthesis)
4. [Flip-Flops (Flops) & Coding Styles](#flip-flops-flops--coding-styles)
5. [Multiplication by Wiring](#multiplication-by-wiring)
6. [CMOS & PVT Considerations](#cmos--pvt-considerations)
7. [Optimization Techniques](#optimization-techniques)
8. [Key Takeaways](#key-takeaways)

---

## Timing Libraries (`.lib`)

Timing libraries define standard cells characterized across **PVT (Process, Voltage, Temperature)**:

- **Process:** Fabrication variations  
- **Voltage:** Supply voltage variations  
- **Temperature:** Operating temperature changes  

**Naming conventions:**

| Suffix | Meaning                  |
|--------|--------------------------|
| `tt`   | Typical process          |
| `025C` | 25°C                     |
| `1v80` | 1.8V                     |

Each cell includes:

- Leakage power for input combinations  
- Area  
- Power ports  
- Input capacitance  
- Transition delays  

---

## Hierarchical vs Flat Synthesis

### Hierarchical Synthesis
- Preserves module hierarchy in the netlist.  
- Example:

| Module        | Gate |
|---------------|------|
| sub_module1   | AND  |
| sub_module2   | OR   |

- Sub-modules appear in the netlist rather than individual gates.  
- **CMOS Note:** OR gates may be replaced by inverter + NAND due to PMOS stacking inefficiency.

### Flat Synthesis
- Flattens the design using the `flatten` command.  
- All logic is merged into a single-level netlist.  
- Advantage: Maximum optimization for area and timing.  
- Disadvantage: Module hierarchy is lost.

---

## Sub-Module Level Synthesis

RTL designs are modular; synthesizing sub-modules individually provides:

- **Optimization & Area Reduction:** Logic optimization and technology mapping per module.  
- **Reusability:** Verified modules can be reused in larger designs.  
- **Parallel Processing:** Concurrent synthesis reduces total runtime for large designs.

**Example Command Flow:**
```bash
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog multiple_modules.v
synth -top sub_module1
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
````

* Synthesizing `sub_module1` generates only an AND gate.

---

## Flip-Flops (Flops) & Coding Styles

Flops prevent glitches in digital circuits through:

* **Synchronization:** Edge-triggered, output changes only at clock edges.
* **Timing Control:** Clock ensures all operations are synchronized.

**Types of Flops:**

* **Asynchronous Reset:** Immediately resets output.
* **Synchronous Reset:** Resets output on the next clock edge.

**Synthesis Example (Async Reset DFF):**

```bash
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_asyncres.v
synth -top dff_asyncres
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```

---

## Multiplication by Wiring

* **Multiply by 2:**

```verilog
y[3:0] = {a[2:0], 0}  // Left shift by 1
```

* **Multiply by 9 (8 + 1):**

```verilog
y[5:0] = {a[2:0], 000} + a[2:0]
```

* Wiring-only implementation requires no extra hardware.

---

## CMOS & PVT Considerations

* OR gates may be optimized into inverter + NAND due to **PMOS stacking inefficiency**.
* `.lib` cells account for **Process, Voltage, Temperature, and Area sensitivity**.

---

## Optimization Techniques

1. **Combinational Logic Optimization**

   * Algebraic factorization
   * Boolean simplification
   * Karnaugh Map (K-Map)
   * Quine-McCluskey method

2. **Constant Propagation** – Simplifies logic using known constants.

3. **Boolean Logic Optimization** – Minimizes gate count and delay.

4. **Sequential Logic Optimization**

   * Algebraic simplification
   * Retiming for speed and area reduction

5. **Bounded Sequential Circuit Optimization**

   * Sequential equivalence checking
   * Retiming


## Key Takeaways

* `.lib` files describe standard cells and PVT characteristics.
* **Hierarchical synthesis** preserves module structure; **flat synthesis** maximizes optimization.
* Sub-module synthesis allows parallel processing, reusability, and area-efficient designs.
* Flop design (async vs sync) ensures glitch-free circuits.
* Simple multipliers can often be realized with wiring alone.
* Logic and sequential optimizations are essential for area, timing, and power efficiency.

