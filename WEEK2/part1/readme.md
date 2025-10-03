 Week 2 Part 1 – BabySoC Fundamentals & Functional Modelling

Objective
The aim of this week’s task is to build a strong foundation in **System-on-Chip (SoC) design** and understand how **functional modelling** plays a role in the early stages of chip development.  
By studying the simplified **BabySoC**, we focus on the core ideas behind SoC architecture without being overwhelmed by real-world complexity. This task also helps us practice simulation using **Icarus Verilog** and waveform analysis with **GTKWave**.

 What is a System-on-Chip (SoC)?
A **System-on-Chip (SoC)** is an integrated circuit that consolidates the essential components of a computer system into a single chip. Instead of having multiple discrete chips (processor, memory, peripherals), everything is embedded on the same die.  

This integration brings several advantages:
- **Performance** – Faster communication since components are on-chip.
- **Power Efficiency** – Consumes less power compared to multi-chip designs.
- **Compactness** – Reduces board space and cost.
- **Reliability** – Fewer inter-chip connections mean fewer failure points.

Modern devices like smartphones, wearables, IoT nodes, and even high-performance processors rely heavily on SoC designs.
![SoC Diagram](photos/M1IOS6-10a74205c8d8.jpg)


Components of a Typical SoC
1. **CPU (Central Processing Unit / Processor Core)**  
   - The computational engine of the SoC.  
   - Executes instructions, performs arithmetic/logical operations, and controls overall system behavior.  
   - In BabySoC, a simple **RISC-V based processor** is used for learning purposes.

2. **Memory**  
   - Stores both data and instructions.  
   - Typically includes:
     - **Volatile memory (RAM)** for temporary storage.
     - **Non-volatile memory (ROM/Flash)** for program storage.  
   - Acts as the workspace where the CPU fetches instructions and stores results.

3. **Peripherals**  
   - Allow the SoC to interact with the outside world.  
   - Examples include UART for serial communication, GPIO for input/output control, and timers for scheduling tasks.  
   - They extend the capabilities of the CPU beyond basic computation.

4. **Interconnect / Bus**  
   - The backbone that connects CPU, memory, and peripherals.  
   - Ensures smooth communication between components.  
   - Examples in real-world SoCs include **AMBA buses** like AXI, AHB, and APB.
![SoC Diagram](photos/socdesignflow)

---
 Why BabySoC?
Designing and verifying a full-fledged industrial SoC is extremely complex. BabySoC is introduced as a **minimalistic model** to make SoC concepts more approachable.  

- **Simplified Architecture**: Focuses only on the CPU, memory, and a few peripherals.  
- **Hands-on Learning**: Lets students simulate and analyze SoC behavior early.  
- **Bridging Theory and Practice**: Instead of just reading about SoCs, BabySoC helps us *experiment*.  
- **Stepping Stone**: Once comfortable with BabySoC, learners can move toward RTL coding, synthesis, and physical design stages.

---
Role of Functional Modelling
Before diving into **RTL (Register Transfer Level)** design or **physical implementation**, it is essential to verify the **functionality** of the SoC. This is where functional modelling comes in.  ![SoC Diagram](photos/babysoc)

### Why is it important?
- Detects **logical errors** early, before they propagate into later stages.  
- Saves time and reduces cost by minimizing design re-spins.  
- Helps visualize how CPU, memory, and peripherals interact.  
- Builds confidence in design decisions before committing to hardware.

### How do we do it?
- **Icarus Verilog (iverilog)** is used for compiling Verilog testbenches and designs.  
- **GTKWave** is used to analyze the generated waveform (`.vcd`) files.  
- By simulating BabySoC, we can trace signals, check data flow, and ensure expected behavior.

Phase-Locked Loop (PLL)

The Phase-Locked Loop (PLL) is one of the most important blocks in VSDBabySoC. Its job is to generate a clean, stable clock signal that keeps the CPU and DAC perfectly synchronized. Without a PLL, the entire chip would run into timing mismatches.

How it works (step by step):

Reference Input – The PLL receives an external clock (usually low-frequency and noisy).
Phase Detector (PD) – Compares the phase of the input clock with the PLL’s own oscillator output.
Loop Filter (LF) – Smooths the phase error signal into a clean control voltage.
Voltage Controlled Oscillator (VCO) – Adjusts frequency based on the control voltage.
Divider (optional) – Helps multiply or divide the frequency for specific needs.

Why PLL is needed in SoCs:

Off-chip clocks may be noisy or unstable → PLL cleans them up.
Different blocks of the chip may need different frequencies (e.g., CPU at 100 MHz, DAC at 50 MHz).
External crystals suffer from tolerance errors, temperature drift, and aging. The PLL compensates for these variations.
 In BabySoC, the PLL ensures the RVMYTH CPU and DAC always run in sync, avoiding glitches in audio/video output.
![SoC Diagram](photos/pll)



 Digital-to-Analog Converter (DAC)

The Digital-to-Analog Converter (DAC) bridges the digital and analog worlds. While the CPU and memory deal with binary 0s and 1s, the real world (sound, images, voltages) is continuous. The DAC makes this conversion possible.

Key Points about DAC:

Input: Digital binary number (e.g., 1010110010 from the CPU’s r17 register).
Output: Continuous analog voltage (proportional to the binary value).
Resolution: A 10-bit DAC (used in BabySoC) can represent 1024 voltage levels.

Types of DACs:

Weighted Resistor DAC – Simple but impractical for higher bit counts (needs precise resistors).
R-2R Ladder DAC – Most common; uses only two resistor values (R and 2R), scalable and accurate.
Role in BabySoC:
The RVMYTH CPU continuously updates register r17 with digital values.
These values are fed into the DAC, which converts them to analog signals.
The analog signal is stored in the OUT file or sent directly to external devices like:
Speakers (for sound).
Displays (for video).
Other analog hardware.
This makes BabySoC not just a “digital-only” chip, but a true mixed-signal SoC, since it interacts with real-world analog devices.
![SoC Diagram](photos/dac)


---

 Key Learnings 
- A System-on-Chip integrates **CPU, memory, peripherals, and interconnect** on a single chip to provide a complete computing system.  
- BabySoC acts as a **learning platform** that simplifies SoC design for beginners.  
- **Functional modelling** is the first checkpoint in SoC design flow, ensuring that the basic system works correctly before RTL and physical design.  
- Tools like **Icarus Verilog** and **GTKWave** provide valuable insight into simulation and debugging.

---

 Conclusion
The BabySoC is not just a toy model—it represents the essence of how modern chips are structured. By starting with this simplified version, we gain the intuition needed to tackle more complex designs later.  
Functional modelling reminds us that in chip design, **verification always comes before implementation**. This philosophy ensures robust designs and forms the backbone of the semiconductor industry workflow.
