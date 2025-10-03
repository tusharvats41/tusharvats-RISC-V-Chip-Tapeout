 VSDBabySoC – Week 2 Part 2 Labs(Functional Modelling)

This repo documents the **Week 2 task** of the [SFAL VSD SoC Journey](https://github.com/hemanthkumardm/SFAL-VSD-SoC-Journey).
The goal is to set up tools, generate Verilog from TL-Verilog, compile the BabySoC design, and verify functionality with **Icarus Verilog** and **GTKWave**.

---

 Setup Instructions

Follow these commands in order to install dependencies and configure your environment.

```
# 1. Update system
sudo apt update

# 2. Install Python pip
sudo apt install python3-pip

# 3. Install SandPiper-SaaS (TL-Verilog to Verilog converter)
pip3 install sandpiper-saas

# 4. Add local bin to PATH (needed for sandpiper-saas)
export PATH=$HOME/.local/bin:$PATH

# 5. Make this PATH change permanent
echo 'export PATH=$HOME/.local/bin:$PATH' >> ~/.bashrc

# 6. Verify installation
sandpiper-saas --version
```

---

 Building BabySoC

```
# 1. Go to VSDBabySoC folder
cd ~/VSDBabySoC

# 2. Run pre-synthesis simulation build
make pre_synth_sim
```

---

 Running Simulation

```
# Run compiled output
./output/pre_synth_sim/pre_synth_sim.out
```


 Viewing Waveforms

```
gtkwave output/pre_synth_sim/pre_synth_sim.vcd
```
 Reset Operation
![Reset waveform](photos/waveform.png)

This screenshot shows the BabySoC reset waveform. The CPU registers are initialized and clock starts running after reset.
