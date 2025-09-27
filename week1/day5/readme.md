 Day 5 - Introduction to DFT (Design for Testability)


- Issues in chip manufacturing
- 3 W's of DFT (What, Why, When/Where)
- Basic terminologies: Controllability, Observability, Fault, Error, Failure
- DFT Techniques: Scan Chains, ATPG
- Synopsys Tools commands for Design, Library, and IC Compiler

## Repository Structure
- `Notes/` - Markdown notes for DFT topics
- `Images/` - Reference images for controllability, observability, and scan chains



### **Notes/Day5_DFT_Introduction.md**

markdown
# Day 5 - Introduction to DFT

### Issues in chip manufacturing:
1. **Density Issue**: Smaller, denser designs increase probability of wire breaks or shorts.
2. **Software Issue**: Bugs in CAD tools can cause design errors.
3. **Application Issue**: Critical applications like medical devices and rockets require fault-free chips.
4. **Maintenance Issue**: Smaller PCBs and SoCs make maintenance expensive.
5. **Business Issue**: Faulty chips lead to financial losses.

### 3 W's of DFT
- **What**: Testability means a design is well-controllable and well-observable.
- **DFT**: Techniques to make a design testable post-fabrication.
  - Examples: MBist logic for macros, scan chains for flops, test patterns for combinational circuits.
- **Why**: Makes post-production testing easier and reduces business risk.
- **When & Where**: At the start of ASIC design flow, included during synthesis (front-end).

### Levels of testing:
1. Chip-level (die-level)
2. Board-level
3. System-level
```

---

### **Notes/DFT_Terminologies.md**

```markdown
# Basic Terminologies

- **Controllability**: Ability to set any node to 0 or 1 via scan chains.  
  - Example: Adding a MUX increases controllability at cost of area/power.

- **Observability**: Ability to measure the state of a logic signal.  
  - Example: Adding a flip-flop allows shifting out values through scan patterns.

- **Fault**: Physical defect in a chip.
- **Error**: System enters incorrect state due to a fault.
- **Failure**: System does not deliver expected service.
- **Fault Coverage**: % of logical faults tested by a test set.
- **Defect Level**: Fraction of shipped parts that are defective.
```

---

### **Notes/DFT_Techniques.md**

```markdown
# DFT Techniques

- **Scan Chain Techniques**
  1. Specify scan constraints
  2. Specify scan ports and enables
  3. Compile DFT
  4. Identify number of scan chains

- **Purpose of scan flops**
  - Test stuck-at faults
  - Test path delays

- **ATPG (Automatic Test Pattern Generation)**
  - Used to generate test patterns for fault detection
```

---

### **Notes/Synopsys_Tools_Commands.md**

````markdown
# Synopsys Tools Overview

## Design Compiler
```bash
dc_shell
gui_start
````

## Library Compiler

```bash
lc_shell
gui_start
```

## IC Compiler

```bash
icc2_shell
gui_start
```

````

---


You can add images for controllability, observability, and scan chains as referenced in your notes. Name them:
- `Controllability.png`
- `Observability.png`
- `ScanChain.png`

---

This is **GitHub-ready**. Once you create the folder structure and add these files, you can do:

```bash
git init
git add .
git commit -m "Add Day 5 DFT notes and Synopsys commands"
git branch -M main
git remote add origin <your_repo_url>
git push -u origin main
````



