# NASSCOM-VSD-SOC-Design
OpenLANE is an open-source, end-to-end VLSI (Very Large Scale Integration) physical design flow tool. It is intended to help with every step of the integrated circuit (IC) design process, from register-transfer level (RTL) to general layout specifications (GDSII), which are needed to manufacture the chip. OpenLANE offers a complete and configurable design flow by integrating several open-source tools and components.     

The RTL2GDS flow is classified as follows:
1. Synthesis
The purpose is to transform the RTL code into a gate-level netlist using RTL code (Verilog or VHDL) as input. The high-level constructs are mapped to a technology-specific library of standard cells. The tool used in OpenLANE is Yosys.
2. Floorplanning and Placement
The physical structure and arrangement of cells on the chip are defined.
In Floorplanning, the input is gate-level netlist and the output is the floorplan with block placements. For the process, the size and shape of the chip is defined. Areas for various functional blocks are defined and the placement of main components and power or ground networks is planned. OpenROAD is used for floorplanning and placement.
3. Routing
The purpose of routing is to connect placed cells according to the netlist.
Input is the placed netlist and output is the routed design. A routing plan is created to interconnect all the cells by minimizing the wire length, delay, and congestion. Power and clock distribution networks are implemented. OpenROAD is used for global and detailed routing.
4. Design Rule Check (DRC) & Layout Versus Schematic (LVS)
The main purpose is to verify and check if the design meets the requirements and is ready for manufacturing. The input is the routed design and the output is the verified design ready for GDSII export. Design Rule Check (DRC) ensures that the layout complies with the manufacturing rules using Magic. Layout Versus Schematic (LVS) verifies that the layout matches the original netlist using Netgen. Timing analysis is used to ensure the design meets timing constraints using OpenSTA.


## Day 1 - Inception of open-source EDA, OpenLANE and Sky130 PDK 

The session is focus on the synthesis process of a specific design ‘picorv32‘ using the OpenLane flow. The netlist and essential reports is generated following the synthesis process.

The following are the commands used in the terminal to execute the synthesis and produce a netlist:
 First we need to set the environment for the design to be synthesized using Openlane. We use the following commands to setup the environment:

Change directory to openlane:
cd /home/Desktop/work/tools/openlane_working_dir/openlane

Run the flow.tcl in the interactive mode:
 ./flow.tcl script -interactive

Load the require package openlane 0.9:
package require openlane 0.9

Preparation of the design files (run):
prep -design picorv32a

Synthesis is carried out using the following command:
run_synthesis

<img width="394" alt="3" src="https://github.com/pheiroijampooja/NASSCOM-VSD-SOC-Design/assets/171696481/dbd0193a-42f0-4472-b033-c0c62c405d3d">

<img width="446" alt="6" src="https://github.com/pheiroijampooja/NASSCOM-VSD-SOC-Design/assets/171696481/11cddfd3-9904-4fb7-b11e-91c2ed8ef816">

### Assignment 1: Flip-flop ratio

<img width="523" alt="number of cells" src="https://github.com/pheiroijampooja/NASSCOM-VSD-SOC-Design/assets/171696481/7109696e-71ab-40e8-81db-5025d4475d2a">
<img width="523" alt="FF_dfxtp_2" src="https://github.com/pheiroijampooja/NASSCOM-VSD-SOC-Design/assets/171696481/51ad71a0-e92e-4413-8791-42d3318ac399">

Total number of cells are 14876.
Number of D flip-flops are 1613.
D flip-flops ratio is  approximately 10.8% highlighting that D flip-flops are minority in the design.


## Day 2 - Good floorplan vs bad floorplan and introduction to library cells

In floorplanning, position of the chip's main functional blocks is determined. It has impact on the chip's manufacturability, power consumption, and performance. The parameters deciding the floorplanning are utilization factor, aspect ratio, de-coupling capacitor, and power planning.

Step to operate Floorplan with Openlane –

• Setting default core utilization ratio as 65% and the aspect ratio as 1.

• On completion of synthesis, the following command is used to generate the floorplan PDN: run_floorplan

• The command to generate layout of floorplan using magic —T:
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def

<img width="926" alt="floor_plan_top_view" src="https://github.com/pheiroijampooja/NASSCOM-VSD-SOC-Design/assets/171696481/4a1aeb09-5ee8-4eba-b495-1527d8ca10fe">
<img width="926" alt="floor_plan" src="https://github.com/pheiroijampooja/NASSCOM-VSD-SOC-Design/assets/171696481/7a66abac-b236-4ac6-86a5-15bcb43dcd39">

Step to execute Placement Using Openlane –

• Run the command for placement : run_placement

• The following command is used to generate placement layout in the magic tool:
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def

<img width="926" alt="floor_plan" src="https://github.com/pheiroijampooja/NASSCOM-VSD-SOC-Design/assets/171696481/95184df0-4bcc-43e3-80e1-f37bc7d7dc71">

### Assignment-2

<img width="741" alt="die area" src="https://github.com/pheiroijampooja/NASSCOM-VSD-SOC-Design/assets/171696481/aa4b2653-f4ea-431d-850e-829917ec9819">

Information about the die area present in DEF (Design Exchange Format) file, is specified as (0 0) to (660685 671405). Here, 1 micron is equal to 1000 database units. So, dividing by 1000, gives the chip dimensions in micrometers. Therefore, chip width  and height is 660.685 micrometer and 671.405 micrometer respectively.

## Day 3 - Design library cell using Magic Layout and ngspice characterization

The working directory is cloned in the github repo using the command: git clone https://github.com/nickson-jose/vsdstdcelldesign.git 

CMOS inverter layout is visualized with the command: magic -T sky130A.tech sky130_inv.mag & 

SPICE netlist extraction

Commands used in tkcon Magic window for spice extraction of custom inverter layout

#Checking current directory:
pwd

#Command to extract to .ext format:
extract all

#Command to enable the parasitic extraction before converting ext to spice:
ext2spice cthresh 0 rthresh 0

#Command to convert to ext to spice:
ext2spice

#Inside sky130_inv.ext,the netlist information extracted can be observed


#Modified spice netlist

`.option scale=0.01u

.include ./libs/pshort.lib

.include ./libs/nshort.lib

//.subckt sky130_inv A Y VPWR VGND

M1001 Y A VGND VGND nshort_model.0 ad=1.44n pd=0.152m as=1.37n ps=0.148m w=35 l=23

M1000 Y A VPWR VPWR pshort_model.0 ad=1.44n pd=0.152m as=1.52n ps=0.156m w=37 l=23

VDD VPWR 0 3.3V

VSS VGND 0 0V

C6 Y 0 2fF

Va A VGND PULSE(0V 3.3V 0 0.1ns 0.1ns 2ns 4ns)

C0 A VPWR 0.0774fF

C1 Y VPWR 0.117fF

C2 A Y 0.07f4fF

C3 Y VGND 0.279fF

C4 A VGND 0.45f

C5 VPWR VGND 0.781f

//.ends

.tran 1n 20n

.control

run

.endc

.end`

<img width="921" alt="spice out" src="https://github.com/pheiroijampooja/NASSCOM-VSD-SOC-Design/assets/171696481/a96c311a-69aa-4c7c-ba40-5d565e6bee36">

![Layout](https://github.com/pheiroijampooja/NASSCOM-VSD-SOC-Design/assets/171696481/3fb507c7-470b-45d3-bf30-5f3963cf5b61)

<img width="650" alt="ext2spice" src="https://github.com/pheiroijampooja/NASSCOM-VSD-SOC-Design/assets/171696481/742bc95e-a737-4eb1-9ada-860395617aac">

### Assignment-3

Characterization of cell-

Find 4 values

(a) Rise transition:  2.24696 ns – 2.18265 ns = 0.06431 ns.

(b) Fall transition:  4.09551 ns – 4.05316 ns = 0.04235 ns.

(c) Cell rise delay:  2.21155 ns – 2.15009 ns = 0.06146 ns.  

(d) Cell fall delay:  4.7835 ns – 4.05 ns = 0.02835 ns.
