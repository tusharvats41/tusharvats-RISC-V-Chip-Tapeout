 Gate-Level Simulation (GLS) of BabySoC

 Post-Synthesis Functional Verification

**Gate-Level Simulation (GLS)** is an essential step in verifying a System-on-Chip (SoC) design after synthesis. Unlike high-level RTL simulations, which work on behavioral descriptions, GLS operates on the **gate-level netlist** — a detailed representation of logic gates and connections generated during the synthesis process.

---

Purpose of GLS

The primary goal of GLS is to validate that the **synthesized design** maintains correct functionality and timing behavior before moving into physical design or fabrication stages.

---

 Key Aspects of GLS in BabySoC

#### 1. Verification with Timing Information

* GLS uses **Standard Delay Format (SDF)** files to include **realistic timing delays** in the simulation.
* This ensures that the SoC performs correctly under actual propagation delays and setup/hold time constraints.

#### 2. Post-Synthesis Design Validation

* Confirms that the logic and dataflow are **preserved** correctly after synthesis.
* Detects potential issues like **glitches**, **metastability**, or **unintended signal delays**.
* Helps verify that synthesis optimizations haven’t altered functional intent.

#### 3. Simulation Environment

* Tools such as **Icarus Verilog (iverilog)** are used to compile and run the synthesized netlist.
* **GTKWave** is used for waveform visualization and debugging.
* Typical simulation outputs include `.out` (compiled simulation) and `.vcd` (waveform data) files.

#### 4. Importance in BabySoC

* BabySoC integrates several components — **RISC-V core (RVMYTH)**, **PLL**, and **DAC**.
* Gate-level simulation ensures:

  * The modules communicate correctly at gate-level.
  * The synthesized design meets **functional and timing requirements**.
  * The post-synthesis version behaves the same as the pre-synthesis RTL design.

---

 Outcome

Performing GLS gives confidence that:

* The synthesized BabySoC is functionally accurate.
* The design can be safely taken to the **place-and-route** stage.
* Timing and logic integrity are verified before physical realization.
![GLS Waveform](synthesislog/glswaveform)
