# 🖥️ VSDBabySoC Design and Modeling - Week_2:

Welcome to the VSDBabySoC Workshop, a hands-on learning repository focused on System-on-Chip (SoC) fundamentals, functional modelling, simulation, and digital–analog integration. This repository documents my learnings while building and simulating a simplified RISC-V-based SoC called BabySoC.

## 📚 About This Repository:
This repository is intended for students, hobbyists, and engineers who want to learn about:
- SoC fundamentals and architecture (CPU, memory, peripherals, interconnect)
- Functional modelling of SoCs using Icarus Verilog and GTKWave
- Digital-to-analog interfacing with a 10-bit DAC
- RISC-V core operation (RVMYTH processor)
- Hands-on simulation and verification of a small SoC

## 🛠 Tools Used
- Icarus Verilog (iverilog) – Compile and simulate Verilog RTL
- GTKWave – Waveform visualization
- SandPiper-SaaS – Convert TL-Verilog (.tlv) to Verilog (.v)
- Git – Version control

## 📝 Prerequisites:
- Basic understanding of digital logic (gates, flip-flops, multiplexers)
- Familiarity with Linux shell commands
- Linux environment (or WSL on Windows/macOS)
- Tools: git, iverilog, gtkwave, python3, pip

## 📅 Workshop / Learning Structure:
### Part 1 – [BabySoC Fundamentals & Functional Modelling](./Part-1)
- Introduction to SoCs: architecture, types, and design challenges
- BabySoC architecture: RVMYTH CPU, PLLs, 10-bit DAC
- Functional modelling: simulation of CPU-peripheral interaction
- Observing system behavior using GTKWave

### Part 2 – Hands-on Labs (Functional Modelling)(./Part-2)
- Project setup and directory structure
- Module descriptions (RVMYTH, PLL, DAC, testbench)
- TLV → Verilog conversion for simulation
- Running pre-synthesis simulations
- Viewing DAC output in digital and analog modes
- Troubleshooting common simulation issues

## 💡 Key Concepts Learned
- Functional Modelling: Verify system behavior before RTL design
- CPU–Peripheral Interaction: Observe digital data flow and DAC conversion
- Clock Synchronization: Importance of PLL in SoC timing
- Simulation & Waveform Analysis: Use GTKWave to analyze signals and outputs
- Hands-on SoC Verification: Compile, simulate, and debug a small RISC-V SoC
