# NASSCOM_VSD_SOC_DESIGN_PROGRAM

**Documentation: RTL to GDSII Implementation using OpenLane and Sky130 PDK**

---

## Overview
This document provides a comprehensive guide for implementing RTL to GDSII flows using OpenLane and the Sky130 PDK. It encompasses the complete ASIC design flow, including tool configuration, implementation steps, and analysis methodologies, supplemented with practical laboratory exercises and visual documentation.

---

# Introduction to Open-Source EDA, OpenLANE, and Sky130 PDK

## Overview
This section presents a detailed introduction to integrated circuit design fundamentals, covering packaging methodologies, die architecture, foundry operations, and the software-to-hardware translation process. It emphasizes the critical role of RTL in hardware design and outlines the open-source ASIC design ecosystem.

## Computer Architecture Fundamentals

### Chip Packaging and Structure
* Modern embedded systems incorporate integrated circuits (ICs) housed within protective packages, facilitating board mounting and handling. The primary interconnection method employed is wire bonding.
* The chip architecture consists of:
  * I/O clearing zones for pad placement
  * Core area for main circuit implementation
  * Die area encompassing both components
  * Wire bond connections linking I/O pads to external interfaces

### Manufacturing and IP Integration
* Foundries serve as manufacturing facilities for ICs and provide Intellectual Property (IP) blocks in both soft and hard macro formats
* Instruction Set Architecture (ISA) represents the final human-readable code layer before machine code conversion
* Standard compilation flow: C code → Assembly → Machine code
* RTL serves as the hardware implementation model for ISA execution

## System-on-Chip Design and OpenLANE Framework

### Essential Components
The open-source ASIC design implementation requires three fundamental elements:
1. RTL Designs
2. EDA Tools
3. PDK (Process Design Kit) Data

### ASIC Implementation Flow
OpenLANE orchestrates the following sequential steps:
1. **Synthesis**: Converts RTL to PDK-specific physical library netlist
2. **Floorplan & Power Planning**: Establishes die dimensions, pin configuration, and power distribution
3. **Placement**: Positions standard cells within core region
4. **Clock Tree Synthesis (CTS)**: Implements clock distribution network
5. **Routing**: Establishes signal interconnections
6. **Sign-off**: Performs DRC, LVS, timing verification, and GDS generation

## Laboratory Exercise: Open-Source EDA Tools Familiarization

### Environment Setup
* Working Directory: `~/Desktop/work/tools/openlane_working_dir/openlane`
* Design Directory: `~/Desktop/work/tools/openlane_working_dir/openlane/design`
* Target Design: `picorv32a`
* Configuration Priority: Design-specific `config.tcl` overrides default tool parameters

### OpenLANE Initialization
```bash
docker                    # Initialize Docker container
./flows.tcl -interactive  # Launch OpenLANE interface
package require openlane 0.9  # Load OpenLANE 0.9
prep -design picorv32a    # Initialize picorv32a design
```

### Synthesis Execution
```bash
run_synthesis  # Execute synthesis and ABC optimization
```

### Results Analysis
* Generated files are organized in the following structure:
  * `results/`: Contains synthesized netlist
  * `reports/`: Houses synthesis reports
  * `logs/`: Stores execution logs
  * Note: Most recent reports represent the current state

### Design Analysis
#### Flip-Flop Ratio Calculation
```math
Flip-Flop\ Ratio = \frac{Number\ of\ D\ Flip-Flops}{Total\ Number\ of\ Cells}
```
```math
DFF\ Percentage = Flip-Flop\ Ratio \times 100
```

#### Example Calculation
Based on `reports/synthesis/1-yosys_4.stat.rpt`:
```math
DFF\ Percentage = \frac{1613}{14876} \times 100 = 10.84\%
```

Additional analysis can be performed using STA reports and Yosys usage summaries.

---

## Sky130 Day 1 - Introduction to Open-Source EDA, OpenLANE, and Sky130 PDK

**Summary:**
This section introduces the fundamentals of chip packaging, die structure, the role of foundries and IPs, and the flow from C code to machine code. It emphasizes the importance of RTL in hardware design and provides an overview of the open-source ASIC design ecosystem.

## 📌 SKY130_D1_SK1 - How to Talk to Computers

- Chips are packaged for easier mounting on embedded boards. Common packaging method: **Wire Bonding**.
- The chip contains:
  - **I/O Clearance Area**: Hosts I/O pads
  - **Core Area**: Contains logic and memory
  - All within the **Die Area**
- **Foundries** manufacture chips and may provide **IP blocks** (hard/soft macros).
- **ISA (Instruction Set Architecture)**: Assembly code just above machine language.
- Typical software-to-hardware flow:  
  `C code → Assembly → Machine code`
- **RTL** models hardware logic that runs the ISA-defined instructions.

---

## 🏗️ SKY130_D1_SK2 - SoC Design and OpenLANE Flow

To build open-source ASICs, we need:

1. RTL Designs (e.g., Verilog)
2. Open-source EDA tools (e.g., Yosys, OpenROAD)
3. PDK (e.g., Sky130)

### 🔄 OpenLANE ASIC Design Flow:

1. **Synthesis**  
   Converts RTL to gate-level netlist using standard cells.
2. **Floorplan & Powerplan**  
   Defines die size, pin/pad placement, and power domains.
3. **Placement**  
   Places cells within the core.
4. **CTS (Clock Tree Synthesis)**  
   Distributes the clock to sequential elements.
5. **Routing**  
   Connects all logic using metal layers.
6. **Signoff**  
   Performs DRC, LVS, STA. Generates final GDSII file.

---

## 🧪 LAB: SKY130_D1_SK3 - OpenLane Hands-On with `picorv32a`

* Enter the dir `~/Desktop/work/tools/openlane_working_dir/openlane` all openlane runs must happen from this directory.
* the design folder `~/Desktop/work/tools/openlane_working_dir/openlane/design` holds the data for RTL2GDSII implementation, we focus on `picorv32a`. the `config.tcl` inside the designs have the highest priority and the cmds mentioned in them override the default commands in the tools
* To open the openlane instance:
  ```bash
  docker
  # calls the docker instance for the openlane
  ./flows.tcl -interactive
  # opens openlane terminal
  package require openlane 0.9
  # calls openlane 0.9
  prep -design picorv32a
  # prepares Picorv32a design for implemtation
  ```
  ![Screenshot from 2025-03-27 14-08-29]([File\1.jpeg](https://github.com/Sirishaterlu/report/blob/main/File/1.jpeg))

* after this `runs` dir willbe created in the `designs/picorv32a` folder
  ![Screenshot from 2025-03-27 14-10-38]([File\2.jpeg](https://github.com/Sirishaterlu/report/blob/main/File/2.jpeg))
* inside the run directory the folders will be created as the runs are done
  ![run dir]([File\3.jpeg](https://github.com/Sirishaterlu/report/blob/main/File/3.jpeg))
* there is one `config.tcl` which is diffrent from the other config file discussed before, this one shows which default parameters are being taken bt=y the runs
* Running synthesis:
  ```bash
  run_synthesis
  # runs synthesis and abc's
  ```
* after the run we can check the run dir for the results, logs and reports. `results` folder must have the synthesized netlsit. `reports` folder will have all the reports for synthesis step and `logs` will store all the run logs for synthesis. in the `reports` chronologically latest report is most accurate 
  ![runs folder content]([File\4.jpeg](https://github.com/Sirishaterlu/report/blob/main/File/4.jpeg))

* At this step we need to do DFF ratio calculation as follows:
```math
Flip Flop\ Ratio = \frac{Number\ of\ D\ Flip\ Flops}{Total\ Number\ of\ Cells}
```
```math
Percentage\ of\ DFF's = Flip Flop\ Ratio * 100
```
* checking for latest file we have `reports/synthesis/1-yosys_4.stat.rpt` from dir image above. Therefore Flip Flop ratio is:
```math
Percentage\ of\ DFF's = \frac{1613}{14876} * 100 = 10.84 %
```
* we can also analyse the sta reports along with the yosys usage summary
  
# Sky130 Day 2 - Floorplanning, Power Delivery, and Library Cells

## 🧠 Overview

This session explores key concepts in floorplanning, power distribution strategies, pin placements, and pre-placed cells. It also introduces how standard cells from libraries are bound and placed using OpenLANE.

---

## 📐 SKY130_D2_SK1 - Chip Floorplanning Considerations

### Key Definitions:

- **Floorplanning** defines the physical layout and area of the chip.
- The **absolute minimum area** is estimated based on cell dimensions (e.g., standard cells, flip-flops).

```math
Utilization = \frac{Area\ utilized\ by\ netlist}{Total\ core\ area}
```
and 

```math
Aspect\ Ratio= \frac{Heigth of core}{Width of core}
```
* with abs min area known, we need to account for area occupied by routing metals.
## (March/28/2025)
* **🧱 Preplaced Cells** : top level logic is broken into mutiple cuts and are implemented separately
* Large reusable blocks (e.g., ALUs, comparators, muxes) can be implemented as preplaced IP blocks.
*These cells are black-boxed and fixed in the layout before regular cell placement.
*Decoupling Cells are placed around them to maintain stable voltage during power fluctuations by acting as capacitors.
  
* **decoupling  cells** help maintain stable supply voltage across the logic they are placed near , by provding capacitive charging effect in the event of supply voltage drop due to reistance on the pwr supply path
* **🔌 Power Planning**:
* A single power source leads to voltage drops in large circuits.
* Power Meshes are used to distribute power efficiently across the chip.
* **📍 Pin Placement**:
* Pin positioning should reflect signal flow and reduce routing complexity.
* A good understanding of internal logic is required for optimal pin placement.
## LAB : floorplan and placement
* `:~/Desktop/work/tools/openlane_working_dir/openlane/configuration` contains the tool default configurations and `README.md` filde inside that folder lists all the diffrent config variables avaible for the designer
* priority : `:~/Desktop/work/tools/openlane_working_dir/openlane/configuration` < `design/runs/config.tcl` < `design/PDK.tcl`
```bash
  run_floorplan
  # run floorplan
```
floorplan run in terminal
 ![runs folder content]([File\5.jpeg](https://github.com/Sirishaterlu/report/blob/main/File/5.jpeg))
The run outputs structure in runs folder

![runs folder content]([File\6.jpeg](https://github.com/Sirishaterlu/report/blob/main/File/6.jpeg))

floorplan def
![ ]([File\7.jpeg](https://github.com/Sirishaterlu/report/blob/main/File/7.jpeg))

Floorplan def gives us the actual arae of design as follows:
```math
1000\ Unit\ Distance = 1\ Micron
```
```math
Width\ in\ unit\ distance = 660685 - 0 = 660685
```
```math
Height\ in\ unit\ distance = 671405 - 0 = 671405
```
```math
Area\ of\ die\ in\ microns = \frac{660.685 * 671.405}{1000 * 1000} = 443587.21\ Microns^2
```

* we can view the floorplan generated with the help of `magic` tool
```bash
# in a seperate terminal enter the directory with def filr (results folder in recent run stage : floorplan)
magic -T ~/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &

```
magic Gui
![]([File\8.jpeg](https://github.com/Sirishaterlu/report/blob/main/File/8.jpeg))
* in `magic` gui to center the design for better viewing when in  the gui press "s" and "v", "s" is to select an object (entire design in this case)
* to zoom into a location: left click mouse for lower (x1,y1) and right mouse click for setting (x2,y2)
* keep the cursor on the object whose info is needed, then press "s" to select and then in tkconsole write cmd "what"
Pin info in the GUI
![]([File\9.jpg](https://github.com/Sirishaterlu/report/blob/main/File/9.jpg))

diffrent elements in the floorplan
![]([File\10.jpeg](https://github.com/Sirishaterlu/report/blob/main/File/10.jpeg))

### SKY130_D2_SK2 - Library Binding and Placement

* **Library**: contains diffrent flavours of cells, timing, functionality informations
* Placing the netlist elements in core area from floorplan stage is **placement**
* When the placement is done its wrt the pin placement, estimated resistance, capacitances of the interconeects . signal Transition analysis is done to see if the placed cells are recievin the signals on time, if not buffers are added to improve the transition.

### LAB 
```bash
run_placement
```
output on terminal;
![](File\10.jpeg)

files generated in `results/placement` dir. 
![]([File\11.jpeg](https://github.com/Sirishaterlu/report/blob/main/File/11.jpeg))

see the def generated in `magic`

```bash
# in a seperate terminal enter the directory with def file (results folder in recent run stage : placement)
magic -T ~/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &

```

viewing the def generated in `magic`
![]([File\12.jpeg](https://github.com/Sirishaterlu/report/blob/main/File/12.jpeg))

![]([File\13.jpeg](https://github.com/Sirishaterlu/report/blob/main/File/13.jpeg))


## Sky130 Day 3 - Design library cell using Magic Layout and ngspice characterization
### Lab (MArch/30/2025)

* Clone custom inverter standard cell design from github repository: [nickson-jose vsdstdcelldesign]([File\14.jpeg](https://github.com/Sirishaterlu/report/blob/main/File/14.jpeg)).
* Load the custom inverter layout in magic and explore.
```bash
# Enter the Openlane work dir 
# Clone the repo with custom inverter design
git clone https://github.com/nickson-jose/vsdstdcelldesign.git

# Enter repo directory
cd vsdstdcelldesign

# Copy magic tech file to the repo directory for easy access
cp ../../pdks/sky130A/libs.tech/magic/sky130A.tech .

# Command to open custom inverter layout in magic
magic -T sky130A.tech sky130_inv.mag &

# Do "s" to select the element under the cursor. multiple "s" selects all elements connected to the element under the cursor
# after selection in tkconsole  of magic enter the command "what" to get info on elements selected
```
folder created by git clone
![]([File\15.jpeg](https://github.com/Sirishaterlu/report/blob/main/File/15.jpeg))
magic gui for std cell
![]([File\16.jpeg](https://github.com/Sirishaterlu/report/blob/main/File/16.jpeg))
PMOS identification
![]([File\17.jpeg](https://github.com/Sirishaterlu/report/blob/main/File/17.jpeg))
NMOS identification
![]([File\18.jpeg](https://github.com/Sirishaterlu/report/blob/main/File/18.jpeg))
Drain to drain connection of nmos and pmos
![]([File\19.png](https://github.com/Sirishaterlu/report/blob/main/File/19.png))
Pwr to Pmos connection
![]([File\20.png](https://github.com/Sirishaterlu/report/blob/main/File/20.png))
Gnd to Nmos connection
![](https://github.com/sowrabh-adiga/NASSCOM_VSD_SOC_DESIGN_PROGRAM/blob/main/files/Screenshot%20from%202025-03-30%2017-35-01.png)

* Next extract the inv cell layout
```
#inside the magic tkconsole
extract all
# creates cell.ext file
ext2spice
# creates spice file
```
![]([File\21.jpeg](https://github.com/Sirishaterlu/report/blob/main/File/21.jpeg))

### SKY130_D3_SK3 - Sky130 Tech File Labs (March/31/2025)
#### ngspice sim for cell characterization

* After the cell layout is created we need to do characterization (timing , noise)
* timimng characterization is sone after extracting the device parasitics and R,C and running simulatioms
* here for inv cell we do transient analysis to find rise , fall transition, cell delay  

### LAB
* To perform the simulation, follow these steps to create the SPICE deck.
### Steps to Prepare the SPICE Deck:
1. Export the SPICE netlist from Magic (using the ext2spice tool).
2. Modify the SPICE netlist to prepare it for simulation:
 * Add library paths where the PMOS and NMOS models are stored.
 * Define grid dimensions for the simulation environment.
 * Verify that the correct names for PMOS and NMOS models are used (e.g., sky130_fd_pr__nfet_01v8, sky130_fd_pr__pfet_01v8).
 * Add power (VDD) and ground (VSS) connections.
 * Connect the input signal and output nodes properly.
 * Define the type of simulation (in this case, transient analysis) and set the parameters for it.
  ![](https://github.com/sowrabh-adiga/NASSCOM_VSD_SOC_DESIGN_PROGRAM/blob/main/files/Screenshot-17.png) 
Commands for ngspice simulation

```bash
# Command to directly load spice file for simulation to ngspice
ngspice sky130_inv.spice

# ngsice terminal will open, inside that make sur ethere is no errors, warnings mentioned. if errors are present, solve them first, before proceeding any further
plot y vs time a
# plotting output vs time and input
```
![](https://github.com/sowrabh-adiga/NASSCOM_VSD_SOC_DESIGN_PROGRAM/blob/main/files/Screenshot%20from%202025-03-31%2016-57-45.png)



Rise transition time calculation

```math
Rise\ transition\ time = Time\ taken\ for\ output\ to\ rise\ to\ 80\% - Time\ taken\ for\ output\ to\ rise\ to\ 20\%
```
```math
20\%\ of\ output = 0.66\ V
```
```math
80\%\ of\ output = 2.64\ V
```


```math
Rise\ transition\ time = 2.2468 - 2.1824 = 0.06396\ ns = 64.4\ ps
```
![](https://github.com/sowrabh-adiga/NASSCOM_VSD_SOC_DESIGN_PROGRAM/blob/main/files/Screenshot%20from%202025-03-31%2018-07-10.png)

Fall transition time calculation

```math
Fall\ transition\ time = Time\ taken\ for\ output\ to\ fall\ to\ 20\% - Time\ taken\ for\ output\ to\ fall\ to\ 80\%
```
```math
20\%\ of\ output = 660\ mV
```
```math
80\%\ of\ output = 2.64\ V
```
```math
Fall\ transition\ time = 4.0955 - 4.0530 = 42.5\ ps
```

![](https://github.com/sowrabh-adiga/NASSCOM_VSD_SOC_DESIGN_PROGRAM/blob/main/files/Screenshot%20from%202025-03-31%2018-11-20.png)

Rise Cell Delay Calculation

```math
Rise\ Cell\ Delay = Time\ taken\ for\ output\ to\ rise\ to\ 50\% - Time\ taken\ for\ input\ to\ fall\ to\ 50\%
```
```math
50\%\ of\ 3.3\ V = 1.65\ V
```



```math
Rise\ Cell\ Delay = 2.21 - 2.15 = 0.06\ ns = 60\ ps

```
![](https://github.com/sowrabh-adiga/NASSCOM_VSD_SOC_DESIGN_PROGRAM/blob/main/files/Screenshot%20from%202025-03-31%2018-18-42.png)

Fall Cell Delay Calculation


```math
Fall\ Cell\ Delay = Time\ taken\ for\ output\ to\ fall\ to\ 50\% - Time\ taken\ for\ input\ to\ rise\ to\ 50\%
```
```math
50\%\ of\ 3.3\ V = 1.65\ V
```



```math
Fall\ Cell\ Delay = 4.0780 - 4.0501 = 0.0279\ ns = 27.9\ ps
```
![](https://github.com/sowrabh-adiga/NASSCOM_VSD_SOC_DESIGN_PROGRAM/blob/main/files/Screenshot%20from%202025-03-31%2018-23-20.png)


### APRIL/1/2025

download the drc_test folder in home directory
![](https://github.com/sowrabh-adiga/NASSCOM_VSD_SOC_DESIGN_PROGRAM/blob/main/files/Screenshot%20from%202025-04-01%2013-21-44.png)

```bash
cd drc_tests

vi .magicrc
# to view the magic rc file

magic -d XR met3.mag&
```
TO draw a layer in magic
* create a bbox by left right click of mouse
* select the layer from the layer window by clicking it with mid mouse button
![]([File\22.png](https://github.com/Sirishaterlu/report/blob/main/File/22.png))

As example for drc rule fxing open poly.mag in magic and zoomin on poly.9 error, serch for the error defination in online pdk resource at [skywater pdk readthedocs](https://skywater-pdk.readthedocs.io/en/main/rules/periphery.html#poly)
open the tech file `sky130A.tech` in the same folder and edit the missing rule: (the redbox in images below are the edited rules)
![]([File\23.png](https://github.com/Sirishaterlu/report/blob/main/File/23.png))
![]([File\24.png](https://github.com/Sirishaterlu/report/blob/main/File/24.png))
![](https://github.com/sowrabh-adiga/NASSCOM_VSD_SOC_DESIGN_PROGRAM/blob/main/files/Screenshot%20from%202025-04-01%2020-02-47.png)


## Sky130 Day 4 - Pre-layout Timing Analysis and Importance of Good Clock Tree
# SKY130_D4_SK1 - Timing Modeling Using Delay Tables
### April/2/2025
* A Timing Table is used to model the delay of a cell relative to the input transition and output load. This helps in estimating how long a signal takes to propagate through the cell, depending on how fast the input changes and the load it drives at the output.
* To insert the custom design (e.g., sky130_vsdinv) into the OpenLane flow, we need the characterized library (which includes the timing, power, and area data) and LEF (Library Exchange Format) files. These files contain information about the cells, their layout, and other physical attributes necessary for physical design tools.
* These library files are usually obtained and generated from the Magic GUI, which can extract the required timing and parasitic data from the design layout.
  
```
# rename the cell design
save <new name>.mag

save sky130_vsdinv.mag

# open the design in new gui and write lef
write lef <optional name, default is the design name in the gui>
```
![](https://github.com/sowrabh-adiga/NASSCOM_VSD_SOC_DESIGN_PROGRAM/blob/main/files/Screenshot%20from%202025-04-02%2013-09-04.png)
* copy `lib` and `lef` files to `design/<designanme>/src` folder
![](https://github.com/sowrabh-adiga/NASSCOM_VSD_SOC_DESIGN_PROGRAM/blob/main/files/Screenshot%20from%202025-04-02%2013-24-05.png)
* edit the `design/config.tcl` file to include the paths to the new `lib`, `lef` in src folder. This ensures openlane flow picks the new lib and lef
```tcl
set ::env(LIB_SYNTH) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__typical.lib"
set ::env(LIB_FASTEST) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__fast.lib"
set ::env(LIB_SLOWEST) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__slow.lib"
set ::env(LIB_TYPICAL) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__typical.lib"

set ::env(EXTRA_LEFS) [glob $::env(OPENLANE_ROOT)/designs/$::env(DESIGN_NAME)/src/*.lef]
```
![](https://github.com/sowrabh-adiga/NASSCOM_VSD_SOC_DESIGN_PROGRAM/blob/main/files/Screenshot-29.png)

* open the Openlane terminal as before and open a design
```
# Open the openlane terminal
cd Desktop/work/tools/openlane_working_dir/openlane

docker

./flow.tcl -interactive

# Inside openlane terminal

# below cmd to be used to overwrite previous run data 
prep -design picorv32a -tag <prev run folder inside /runs> -overwrite 

# Cmds to include the new cell led in merged.lef in /tm[ folder
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs

run_synthesis
```
* observe the reports for the default run of synthesis
The chip area info
![](https://github.com/sowrabh-adiga/NASSCOM_VSD_SOC_DESIGN_PROGRAM/blob/main/files/Screenshot-33.png)
The WNS and TNS for the design
![](https://github.com/sowrabh-adiga/NASSCOM_VSD_SOC_DESIGN_PROGRAM/blob/main/files/Screenshot-37.png)
The custom `sky130_vsdinv` in `temp/the merged.lef` indicating the cell was successfully used during synthesis
![](https://github.com/sowrabh-adiga/NASSCOM_VSD_SOC_DESIGN_PROGRAM/blob/main/files/Screenshot%20from%202025-04-02%2020-15-58.png)

* To improve the timing of the design during synthesis, we can alter some of the variables (like SYNTH_SIZING, SYNTH_STRATEGY, SYNTH_BUFFERING from `configurations/README.md`) to get desired results
Variable information in `configurations/README.md`
![](https://github.com/sowrabh-adiga/NASSCOM_VSD_SOC_DESIGN_PROGRAM/blob/main/files/Screenshot%20from%202025-04-02%2019-42-55.png)
Changing the variables in openlane terminal
```tcl
# Command to display current value of variable SYNTH_STRATEGY
echo $::env(SYNTH_STRATEGY)

# Command to set new value for SYNTH_STRATEGY
set ::env(SYNTH_STRATEGY) "DELAY 3"

# Command to display current value of variable SYNTH_BUFFERING to check whether it's enabled , if enabled =1
echo $::env(SYNTH_BUFFERING)

# Command to display current value of variable SYNTH_SIZING, if enabled =1
echo $::env(SYNTH_SIZING)

# Command to set new value for SYNTH_SIZING
set ::env(SYNTH_SIZING) 1

# Command to display current value of variable SYNTH_DRIVING_CELL to check whether it's the proper cell or not
echo $::env(SYNTH_DRIVING_CELL)
```
![](https://github.com/sowrabh-adiga/NASSCOM_VSD_SOC_DESIGN_PROGRAM/blob/main/files/Screenshot-35.png)

run synthesis to take all changes
```tcl
run_synthesis
```
![](https://github.com/sowrabh-adiga/NASSCOM_VSD_SOC_DESIGN_PROGRAM/blob/main/files/Screenshot%20from%202025-04-02%2020-13-52.png)

the TNS and WNS seems has become 0.0, but the chip area has increased
![](https://github.com/sowrabh-adiga/NASSCOM_VSD_SOC_DESIGN_PROGRAM/blob/main/files/Screenshot%20from%202025-04-02%2020-17-44.png)

* Next run the floorplan
```
run_floorplan
```
This gives an error in the run and error log in openroad shows this:
![](https://github.com/sowrabh-adiga/NASSCOM_VSD_SOC_DESIGN_PROGRAM/blob/main/files/Screenshot%20from%202025-04-02%2020-54-22.png)
but there are no large macros. to circumvent this we can run each step in floorplan individually 

```
init_floorplan
place_io
tap_decap_or
```
![](https://github.com/sowrabh-adiga/NASSCOM_VSD_SOC_DESIGN_PROGRAM/blob/main/files/Screenshot%20from%202025-04-02%2020-23-54.png)

```
run_placement
```
The overflow has to reduce or be closer to 0

* open the def in `magic` in anaother terminal
```bash
# enter the dir of design
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/24-03_10-03/results/placement/

# Command to load the placement def in magic tool
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &
```
![](https://github.com/sowrabh-adiga/NASSCOM_VSD_SOC_DESIGN_PROGRAM/blob/main/files/Screenshot%20from%202025-04-02%2021-09-16.png)

locating `sky130_vsdinv` instance in the implemented design
![](https://github.com/sowrabh-adiga/NASSCOM_VSD_SOC_DESIGN_PROGRAM/blob/main/files/Screenshot%20from%202025-04-02%2021-12-52.png)

we can see the connectivity by selcting the `sky130_vsdinv` instance by keeping cursor  over it and pressing 's'. To see teh connections enter the cmd `expand` in tkconsole of `magic`

```tcl
expand
```
![](https://github.com/sowrabh-adiga/NASSCOM_VSD_SOC_DESIGN_PROGRAM/blob/main/files/Screenshot%20from%202025-04-02%2021-15-59.png)

### SKY130_D4_SK2 - Timing Analysis with Ideal Clocks Using OpenSTA
#### April/03/2025
*Setup Analysis:*
* Setup analysis determines the minimum time a data signal must be stable before the active clock edge to ensure that the flip-flop can capture the data correctly. If this setup time is not met, the data may be incorrectly latched.
*Clock Jitter:*
* Clock jitter refers to the deviation of a clock signal's edges from their ideal positions. This can be caused by noise, power supply fluctuations, or imperfections in the clock distribution network. Jitter can result in timing violations, where the data may not be captured correctly by flip-flops.
*Clock Uncertainty:*
* Clock uncertainty includes all sources of timing variation that affect the synchronization of data signals. This encompasses clock jitter as well as other factors like skew (the difference in arrival times of the clock at different points in the design), and process variations (differences in the manufacturing process that can cause timing inconsistencies).
* These uncertainty values are incorporated into the slack calculations for setup and hold checks. By adding pessimism to the design, clock uncertainty makes sure the design is robust under less-than-ideal conditions. This results in more conservative timing analysis, ensuring that even with variations, the design will work correctly.

Let me know if you need further
### Lab to config the tool to run opensta post synthesis
create `pre_sta.conf` to define the files to be loaded to the opensta for analysis in diffrent corners
![](https://github.com/sowrabh-adiga/NASSCOM_VSD_SOC_DESIGN_PROGRAM/blob/main/files/Screenshot%20from%202025-04-03%2014-46-10.png)
create `mybase.sdc` file to define timimng contraints info for the sta tool. place it in `designs/picorv32a/src`
![](https://github.com/sowrabh-adiga/NASSCOM_VSD_SOC_DESIGN_PROGRAM/blob/main/files/Screenshot%20from%202025-04-03%2014-39-17.png)
now run the opensta tool
```bash
sta pre_sta.conf
```
the analysis will be done and the results will be printed in the terminal

min (hold)
![](https://github.com/sowrabh-adiga/NASSCOM_VSD_SOC_DESIGN_PROGRAM/blob/main/files/Screenshot%20from%202025-04-03%2014-48-08.png)
max (setup)
![](https://github.com/sowrabh-adiga/NASSCOM_VSD_SOC_DESIGN_PROGRAM/blob/main/files/Screenshot%20from%202025-04-03%2014-48-45.png)

#### Lab:  Optimization
*Reopen the openlane to implemmemt any changes
```bash
docker

./flow.tcl -interactive

package require openlane 0.9

prep -design picorv32a -tag 03-04_09-49 -overwrite

# Adiitional commands to include custom lef to openlane flow
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs

# Command to set new value for SYNTH_STRATEGY
set ::env(SYNTH_STRATEGY) "DELAY 3" 

# Command to set new value for SYNTH_SIZING
set ::env(SYNTH_SIZING) 1

# Command to set new value for SYNTH_MAX_FANOUT
set ::env(SYNTH_MAX_FANOUT) 4

# Now that the design is prepped and ready, we can run synthesis using following command
run_synthesis
```
open new sta cmdline in new terminal
```bash
sta pre_sta.conf
```
screenshots from the run
![](https://github.com/sowrabh-adiga/NASSCOM_VSD_SOC_DESIGN_PROGRAM/blob/main/files/Screenshot%20from%202025-04-03%2015-51-34.png)
![](https://github.com/sowrabh-adiga/NASSCOM_VSD_SOC_DESIGN_PROGRAM/blob/main/files/Screenshot%20from%202025-04-03%2015-52-34.png)
![](https://github.com/sowrabh-adiga/NASSCOM_VSD_SOC_DESIGN_PROGRAM/blob/main/files/Screenshot%20from%202025-04-03%2015-52-52.png)

### Eco implementation

* identify problematic cells from the design 
The oai cell seems to be drving 4 cells
![](https://github.com/sowrabh-adiga/NASSCOM_VSD_SOC_DESIGN_PROGRAM/blob/main/files/Screenshot%20from%202025-04-03%2016-11-33.png)
![](https://github.com/sowrabh-adiga/NASSCOM_VSD_SOC_DESIGN_PROGRAM/blob/main/files/Screenshot%20from%202025-04-03%2016-11-45.png)
```tcl
# Reports all the connections to a net
report_net -connections _04373_

# Checking command syntax
help replace_cell

# Replacing cell
replace_cell _22324_ sky130_fd_sc_hd__o21ai_4

# Generating custom timing report
report_checks -fields {net cap slew input_pins} -digits 4
```
The slack improved from -4.62 to -4.5886
![](https://github.com/sowrabh-adiga/NASSCOM_VSD_SOC_DESIGN_PROGRAM/blob/main/files/Screenshot%20from%202025-04-03%2016-26-41.png)

Nand gate of strength 2 is driving three cells
* changing drive stenght to 3 returns the message "Warning: liberty cell 'sky130_fd_sc_hd__nand2_3' not found."
* changing the drive strenght to 4
![](https://github.com/sowrabh-adiga/NASSCOM_VSD_SOC_DESIGN_PROGRAM/blob/main/files/Screenshot%20from%202025-04-03%2016-34-05.png)
the slack reduced to -4.5555 from -4.5886
![](https://github.com/sowrabh-adiga/NASSCOM_VSD_SOC_DESIGN_PROGRAM/blob/main/files/Screenshot%20from%202025-04-03%2016-35-09.png)

OAI cell of drive strenght 2 has more delay:
* changing drive stenght to 3 returns the message "Warning: liberty cell 'sky130_fd_sc_hd__o21bai_3' not found."
* changing the drive strenght to 4
![](https://github.com/sowrabh-adiga/NASSCOM_VSD_SOC_DESIGN_PROGRAM/blob/main/files/Screenshot%20from%202025-04-03%2016-43-17.png)
The slack reduced from -4.5555 to -4.5060
![](https://github.com/sowrabh-adiga/NASSCOM_VSD_SOC_DESIGN_PROGRAM/blob/main/files/Screenshot%20from%202025-04-03%2016-43-30.png)

to check the timimg through a cell we changed
```tcl
report_checks -from _35312_ -to _35239_ -through _22380_
# report_checks -from _start_pont_net_id -to end_point_net_id -through cell_id
```

### writin the ecos to new netlist

keep the old netlist in different name
```bash
cd ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/03-04_09-49/results/synthesis/

mv picorv32a.synthesis.v picorv32a.synthesis_old.v
```
 
Now write the new netlist in the opensta terminal

```tcl
write_verilog  write_verilog ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/03-04_09-49/results/synthesis/picorv32a.synthesis.v
```
After this stage rerunning synthesis will erase all the good changes done in opensta tool. so after `write_verilog` cmd we can return to openalne cmdline and run

```tcl

#if "run_floorplan" command gives error, following cmds do the same
init_floorplan
place_io
tap_decap_or

# Now we are ready to run placement
run_placement

# With placement finished , run CTS
run_cts
```
After the cts , new .v file is creted in the synthesis results folder 
![](https://github.com/sowrabh-adiga/NASSCOM_VSD_SOC_DESIGN_PROGRAM/blob/main/files/Screenshot%20from%202025-04-03%2017-26-48.png)

### SKY130_D4_SK4 - Timing analysis with real clocks using openSTA
### April/04/2025

* openroad is is the tool around which openlane was built, openroad has inbuit sta tool which we will utilze to analyse timimg.
```bash
docker

./flow.tcl -interactive

package require openlane 0.9

#to start with previous data without overwrite tag
prep -design picorv32a -tag 03-04_09-49 
```

```tcl
# INit Openroad terminal inside openlane terminal
openroad

# Reading lef file
read_lef /openLANE_flow/designs/picorv32a/runs/03-04_09-49 /tmp/merged.lef

# Reading def file
read_def /openLANE_flow/designs/picorv32a/runs/03-04_09-49/results/cts/picorv32a.cts.def

# Creating an OpenROAD database to work with
write_db pico_cts.db

# Loading the created database in OpenROAD
read_db pico_cts.db

# Read netlist post CTS
read_verilog /openLANE_flow/designs/picorv32a/runs/03-04_09-49/results/synthesis/picorv32a.synthesis_cts.v

# Read library for design
read_liberty $::env(LIB_SYNTH_COMPLETE)

# Read in the custom sdc we created
read_sdc /openLANE_flow/designs/picorv32a/src/my_base.sdc

# Setting all cloks as propagated clocks
set_propagated_clock [all_clocks]

# Generating custom timing report
report_checks -path_delay min_max -fields {slew trans net cap input_pins} -format full_clock_expanded -digits 4

# Exit to OpenLANE flow into openRoad
exit
```
cmd runs:
![](https://github.com/sowrabh-adiga/NASSCOM_VSD_SOC_DESIGN_PROGRAM/blob/main/files/Screenshot%20from%202025-04-04%2014-09-21.png)
hold slack report
![](https://github.com/sowrabh-adiga/NASSCOM_VSD_SOC_DESIGN_PROGRAM/blob/main/files/Screenshot%20from%202025-04-04%2014-09-30.png)
setup slack report
![](https://github.com/sowrabh-adiga/NASSCOM_VSD_SOC_DESIGN_PROGRAM/blob/main/files/Screenshot%20from%202025-04-04%2014-09-41.png)

##### removing "sky130_fd_sc_hd__clkbuf_1'" from CTS_CLK_BUFFER_LIST and reruning the cts to see the results


```tcl
# Checking current value of 'CTS_CLK_BUFFER_LIST'
echo $::env(CTS_CLK_BUFFER_LIST)

# Removing 'sky130_fd_sc_hd__clkbuf_1' from the list
set ::env(CTS_CLK_BUFFER_LIST) [lreplace $::env(CTS_CLK_BUFFER_LIST) 0 0]

# Checking current value of 'CTS_CLK_BUFFER_LIST'
echo $::env(CTS_CLK_BUFFER_LIST)

# Setting def as placement def otherwise, we get clock not found error
set ::env(CURRENT_DEF) /openLANE_flow/designs/picorv32a/runs/04-04_09-02//results/placement/picorv32a.placement.def

# Run CTS again
run_cts

# Command to run OpenROAD tool
openroad

# Reading lef file
read_lef /openLANE_flow/designs/picorv32a/runs/04-04_09-02//tmp/merged.lef

# Reading def file
read_def /openLANE_flow/designs/picorv32a/runs/04-04_09-02//results/cts/picorv32a.cts.def

# Creating an OpenROAD database to work with
write_db pico_cts.db

# Loading the created database in OpenROAD
read_db pico_cts.db

# Read netlist post CTS
read_verilog /openLANE_flow/designs/picorv32a/runs/04-04_09-02//results/synthesis/picorv32a.synthesis_cts.v

# Read library for design
read_liberty $::env(LIB_SYNTH_COMPLETE)

# Read in the custom sdc we created
read_sdc /openLANE_flow/designs/picorv32a/src/my_base.sdc

# Setting all cloks as propagated clocks
set_propagated_clock [all_clocks]

# Generating custom timing report
report_checks -path_delay min_max -fields {slew trans net cap input_pins} -format full_clock_expanded -digits 4

# Report hold skew
report_clock_skew -hold

# Report setup skew
report_clock_skew -setup

# Exit to OpenLANE flow
exit


# Checking current value of 'CTS_CLK_BUFFER_LIST'
echo $::env(CTS_CLK_BUFFER_LIST)

# Inserting 'sky130_fd_sc_hd__clkbuf_1' to first index of list
set ::env(CTS_CLK_BUFFER_LIST) [linsert $::env(CTS_CLK_BUFFER_LIST) 0 sky130_fd_sc_hd__clkbuf_1]

# Checking current value of 'CTS_CLK_BUFFER_LIST'
echo $::env(CTS_CLK_BUFFER_LIST)

```
hold slack
![](https://github.com/sowrabh-adiga/NASSCOM_VSD_SOC_DESIGN_PROGRAM/blob/main/files/Screenshot%20from%202025-04-04%2015-24-35.png)
setup slack and skew values
![](https://github.com/sowrabh-adiga/NASSCOM_VSD_SOC_DESIGN_PROGRAM/blob/main/files/Screenshot%20from%202025-04-04%2016-04-37.png)

before removal of "sky130_fd_sc_hd__clkbuf_1":
|Slack|Value|
|----|-----|
|hold | 0.0637   slack (MET)|
|setup | 4.5985   slack (MET)|

After removal of "sky130_fd_sc_hd__clkbuf_1":
|Slack|Value|
|----|-----|
|hold |  0.2351   slack (MET)|
|setup | 4.6943   slack (MET)|

### Sky130 Day 5 - Final Steps for RTL2GDS Using TritonRoute and OpenSTA
# Final Steps Overview:
On Day 5, the following key steps are performed as part of the final stages of the RTL2GDS flow:
3 steps are done here:
*1. Power Routing:*
* Normally, Power Delivery Network (PDN) creation is done during the floorplan stage. However, in the OpenLane flow, power routing is done at this stage. This is where power grid and supply connections to cells are established to ensure that each component gets proper power distribution throughout the chip.
* Proper power routing is crucial for the chip's operation, as insufficient power delivery or poor routing can cause reliability issues and timing failures due to voltage drops.
*2. Signal Routing with TritonRoute:*
*Signal routing refers to the interconnection of the cells (gates, flip-flops, etc.) with the metal layers that carry the signal between them. In this stage, TritonRoute is used for physical routing of signal connections.
* This step ensures that all the signal paths are properly connected, avoiding congestion or routing violations, which could affect chip performance.
*3. RC Extraction:*
* RC Extraction involves extracting the resistance (R) and capacitance (C) values of the interconnects (wires and vias) in the layout. This information is essential for post-layout timing analysis as these parasitic values impact the signal propagation speed, timing, and overall performance of the chip.
*4. Post-Route Timing Analysis with OpenSTA:*
* OpenSTA (Static Timing Analysis) is then used to perform post-route timing analysis. This step analyzes the timing of the design after the physical routing has been completed. It checks whether the design meets the required timing constraints, such as setup and hold times for flip-flops, and if the signal transitions occur within the allowed timing windows.
* The post-route timing analysis takes into account the RC delays from the extracted interconnects, which might not have been present in pre-route timing analysis. Any violations discovered at this stage will be fixed by adjusting the design or routing to ensure that all timing requirements are met.

PWR Routing:
```tcl

# check which def file openlane is currently pointing at , it should be pointing to CTS def
echo $::env(CURRENT_DEF)

# create PDN
gen_pdn
```
pdn.def generated
![](https://github.com/sowrabh-adiga/NASSCOM_VSD_SOC_DESIGN_PROGRAM/blob/main/files/Screenshot-45.png)
view the pdn with 
```bash

cd ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/04-04_09-02/tmp/floorplan

magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../merged.lef def read 12-pdn.def &
```
![](https://github.com/sowrabh-adiga/NASSCOM_VSD_SOC_DESIGN_PROGRAM/blob/main/files/Screenshot%20from%202025-04-04%2018-33-39.png)
![](https://github.com/sowrabh-adiga/NASSCOM_VSD_SOC_DESIGN_PROGRAM/blob/main/files/Screenshot%20from%202025-04-04%2018-34-34.png)


Signal routing
ROUTING_STRATEGY variable isnt available, routing optimization iterations (ROUTING_OPT_ITERS) has a default value of 64
![](https://github.com/sowrabh-adiga/NASSCOM_VSD_SOC_DESIGN_PROGRAM/blob/main/files/Screenshot%20from%202025-04-04%2017-47-51.png)
```tcl
run_routing
```
files genrated after the routing

![](https://github.com/sowrabh-adiga/NASSCOM_VSD_SOC_DESIGN_PROGRAM/blob/main/files/Screenshot%20from%202025-04-04%2017-20-13.png)
to visualize the def with magic
```bash
cd ~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/04-04_09-02/results/routing/

magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.def &

```
![](https://github.com/sowrabh-adiga/NASSCOM_VSD_SOC_DESIGN_PROGRAM/blob/main/files/Screenshot%20from%202025-04-04%2018-49-56.png)
![](https://github.com/sowrabh-adiga/NASSCOM_VSD_SOC_DESIGN_PROGRAM/blob/main/files/Screenshot%20from%202025-04-04%2018-51-57.png)

Post route the SPEF extraction and STA happens with `run_routing` command
![](https://github.com/sowrabh-adiga/NASSCOM_VSD_SOC_DESIGN_PROGRAM/blob/main/files/Screenshot-43.png)

to verify the results open the OpenRoad tool

```tcl
openroad

# Reading lef file
read_lef /openLANE_flow/designs/picorv32a/runs/04-04_09-02/tmp/merged.lef

# Reading def file
read_def /openLANE_flow/designs/picorv32a/runs/04-04_09-02/results/routing/picorv32a.def

# Creating an OpenROAD database to work with
write_db pico_route.db

# Loading the created database in OpenROAD
read_db pico_route.db

# Read netlist post CTS
read_verilog /openLANE_flow/designs/picorv32a/runs/04-04_09-02/results/synthesis/picorv32a.synthesis_preroute.v

# Read library for design
read_liberty $::env(LIB_SYNTH_COMPLETE)

# Read in the custom sdc we created
read_sdc /openLANE_flow/designs/picorv32a/src/my_base.sdc

# Setting all cloks as propagated clocks
set_propagated_clock [all_clocks]

# Read SPEF
read_spef /openLANE_flow/designs/picorv32a/runs/04-04_09-02/results/routing/picorv32a.spef

# Generating custom timing report
report_checks -path_delay min_max -fields {slew trans net cap input_pins} -format full_clock_expanded -digits 4

# Exit to OpenLANE flow
exit
```
cmds
![](https://github.com/sowrabh-adiga/NASSCOM_VSD_SOC_DESIGN_PROGRAM/blob/main/files/Screenshot%20from%202025-04-04%2017-51-50.png)
hold
![](https://github.com/sowrabh-adiga/NASSCOM_VSD_SOC_DESIGN_PROGRAM/blob/main/files/Screenshot%20from%202025-04-04%2017-52-16.png)
setup
![](https://github.com/sowrabh-adiga/NASSCOM_VSD_SOC_DESIGN_PROGRAM/blob/main/files/Screenshot%20from%202025-04-04%2017-52-25.png)

Final Slack values

|Slack|Value|
|----|-----|
|hold |  0.1079   slack (MET)|
|setup | 4.3674   slack (MET)|

# Certificate
![]()

# Acknowledgements

* [Kunal Ghosh](https://github.com/kunalg123), Co-founder, VSD Corp. Pvt. Ltd.
* [Nickson P Jose](https://github.com/nickson-jose), Physical Design Engineer, Intel Corporation.
* [R. Timothy Edwards](https://github.com/RTimothyEdwards), Senior Vice President of Analog and Design, efabless Corporation.






