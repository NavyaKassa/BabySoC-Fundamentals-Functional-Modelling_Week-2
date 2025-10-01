Part 2 – Labs (Hands-on Functional Modelling)

## Project Structure:

- *src/include/* : Contains all header files (*.vh) with macros, parameter definitions, and reusable constants required across the design.
- *src/module/* : Contains the Verilog source files for each individual module in the SoC, including processor, PLL, DAC, and other IP blocks.
- *output/* : Directory for storing compiled outputs, simulation results, and generated files from synthesis or verification runs.

## Requirements:
To work with this project, ensure the following tools and environment are available:
- Icarus Verilog (iverilog) – Required for compiling Verilog source files.
- GTKWave – Used to view and analyze simulation waveform files (.vcd).

## Step-by-step Procedure:
## 1. Setup and Prepare Project Directory
Clone or create the following directory structure for the VSDBabySoC project:
```
VSDBabySoC/
├── src/
│   ├── include/
│   │   ├── sandpiper.vh          # Header file containing macros and parameters
│   │   └── ...                   # Additional header files
│   ├── module/
│   │   ├── vsdbabysoc.v          # Top-level SoC module integrating all components
│   │   ├── rvmyth.v              # RISC-V core module
│   │   ├── avsdpll.v             # PLL (Phase-Locked Loop) module
│   │   ├── avsddac.v             # DAC (Digital-to-Analog Converter) module
│   │   └── testbench.v           # Testbench for simulation and verification
├── output/                       # Stores simulation results and waveform files
└── compiled_tlv/                 # Holds compiled intermediate files if needed
```

## 2. VSDBabySoC Module Descriptions

### 1. `vsdbabysoc.v` – Top-Level Module
**Purpose:**  
Integrates all sub-modules (RVMYTH processor, PLL, DAC, etc.) to form the complete BabySoC.

**Functionality:**  
- Connects the clock signals from the PLL to all modules.  
- Routes data from the RVMYTH processor to the DAC for analog conversion.  
- Instantiates and coordinates all modules, acting as the main SoC wrapper.

### 2. `rvmyth.v` – RVMYTH Processor (RISC-V Core)
**Purpose:**  
Acts as the digital brain of BabySoC.

**Functionality:**  
- Executes instructions stored in internal memory.  
- Uses the `r17` register to store and cycle through values for DAC output.  
- Handles data processing required for generating analog signals.  
- Supports basic RISC-V operations suitable for a compact SoC.

### 3. `avsdpll.v` – PLL (Phase-Locked Loop) Module
**Purpose:**  
Provides a stable and synchronized clock for the SoC.

**Functionality:**  
- Receives an initial clock or input signal.  
- Generates a clean, multiplied, or divided clock to drive RVMYTH and DAC.  
- Ensures all modules operate in harmony, preventing timing mismatches.

### 4. `avsddac.v` – DAC (Digital-to-Analog Converter) Module
**Purpose:**  
Converts digital values from RVMYTH into analog signals.

**Functionality:**  
- Receives digital input from the processor (usually from `r17`).  
- Produces a 10-bit analog output.  
- Writes the analog signal to an output file (e.g., `OUT`) for simulation or external device testing.

### 5. `testbench.v`
**Purpose:**  
Verification module to simulate and validate BabySoC functionality.

**Functionality:**  
- Instantiates the top-level `vsdbabysoc.v`.  
- Generates clock and reset signals.  
- Monitors outputs from DAC and processor.  
- Writes results to waveform files (`.vcd`) for viewing in GTKWave.


## 3. Cloning the Project:
To set up the VSDBabySoC project locally, follow these steps:
a. Navigate to your workspace (for example, ~/SoC_project):
```
cd ~/SoC_project
```

b. Clone the repository:
```
git clone https://github.com/manili/VSDBabySoC.git
```

c. Enter the project directory:
```
cd ~/VLSI/VSDBabySoC/
```

d. Verify the directory structure:
```
ls
# Output:
# images  LICENSE  Makefile  README.md  src
```

e. Check the Verilog module files:
```
ls src/module/
# Output:
# avsddac.v  avsdpll.v  clk_gate.v  pseudo_rand_gen.sv  pseudo_rand.sv
# rvmyth_gen.v  rvmyth.tlv  rvmyth.v
```
✅ This ensures that all necessary modules, including the processor, PLL, DAC, and other helper files, are present in src/module/.


The RVMYTH core of VSDBabySoC is initially provided as a **TL-Verilog (`.tlv`) file**.  
To simulate the SoC using standard Verilog simulators, you need to **convert `rvmyth.tlv` into a `.v` file**.

---

## 🔧 TLV to Verilog Conversion Steps:

### Step 1: Install required Python packages
Ensure `python3-venv` and `pip` are installed:

```
sudo apt update
sudo apt install python3-venv python3-pip
```

### Step 2: Create and activate a virtual environment
```
cd ~/SoC_project/VSDBabySoC/
python3 -m venv sp_env
source sp_env/bin/activate
```
### Step 3: Install SandPiper-SaaS inside the virtual environment
```
pip install pyyaml click sandpiper-saas
```
Step 4: Convert rvmyth.tlv to Verilog
```
sandpiper-saas -i ./src/module/*.tlv -o rvmyth.v --bestsv --noline -p verilog --outdir ./src/module/
```
✅ After this, rvmyth.v will appear in src/module/, ready for simulation with Icarus Verilog or other Verilog tools.

- We can confirm this by listing the module files:

```
cd ~/SoC_project/VSDBabySoC/
ls src/module/
# Output should include:
# avsddac.v  avsdpll.v  clk_gate.v  pseudo_rand_gen.sv  pseudo_rand.sv
# rvmyth_gen.v  rvmyth.tlv  rvmyth.v  testbench.rvmyth.post-routing.v
# testbench.v  vsdbabysoc.v
```

## ⚠️ Notes on Virtual Environment
To use the virtual environment in future sessions, activate it first:
```
source sp_env/bin/activate
```
To exit the virtual environment:
```
deactivate
```


## Simulation Steps

### 1. Pre-Synthesis Simulation:

To perform a pre-synthesis simulation of VSDBabySoC using the following steps:

### Step 1: Create Output Directory
```
cd ~/SoC_project/
mkdir -p output/pre_synth_sim
```

### Step 2: Compile with Icarus Verilog
```
iverilog -o ~/SoC_project/output/pre_synth_sim/pre_synth_sim.out -DPRE_SYNTH_SIM -I ~/SoC_project/src/include -I ~/SoC_project/src/module ~/SoC_project/src/module/testbench.v
```
**Explanation:**
- -DPRE_SYNTH_SIM defines a macro for conditional compilation in the testbench.
- -I includes directories for header files and module sources.
- The output executable will be saved as pre_synth_sim.out in output/pre_synth_sim/.

### Step 3: Run the Simulation
```
cd output/pre_synth_sim
./pre_synth_sim.out
```
- The simulation generates a VCD file (pre_synth_sim.vcd) that can be viewed in GTKWave.

### 2. Viewing Waveforms in GTKWave
Open the VCD file to analyze signals:
```
cd ~/VLSI/VSDBabySoC/
gtkwave output/pre_synth_sim/pre_synth_sim.vcd
```

Recommended signals to monitor:
- CLK – Input clock to the RVMYTH core, sourced from the PLL.
- reset – Input reset signal for RVMYTH, typically external.
- OUT – Output signal from the DAC module (simulated as digital; can be changed to analog).
- RV_TO_DAC[9:0] – 10-bit output from RVMYTH register r17 feeding the DAC.
- OUT (real datatype) – Simulated analog output from the DAC module. Set Data Format → Analog → Step to view analog behavior.

### 3. Troubleshooting Tips
- Module Redefinition:
If you encounter redefinition errors, make sure modules are included only once—either via the testbench or command line, not both.
- Path Issues:
Verify all -I paths are correct. Use full paths if relative paths cause compilation errors.
