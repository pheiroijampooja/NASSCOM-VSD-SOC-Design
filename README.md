# NASSCOM-VSD-SOC-Design
OpenLANE is an open-source, end-to-end VLSI (Very Large Scale Integration) physical design flow tool. It is intended to help with every step of the integrated circuit (IC) design process, from register-transfer level (RTL) to general layout specifications (GDSII), which are needed to manufacture the chip. OpenLANE offers a complete and configurable design flow by integrating a number of open-source tools and components.     

The RTL2GDS flow is classified as follows:
1. Synthesis
The purpose is to transform the RTL code into a gate-level netlist using RTL code (Verilog or VHDL) as input. The high-level constructs are mapped to a technology-specific library of standard cells. The tools used in OpenLANE is Yosys.
2. Floorplanning and Placement
The physical structure and arrangement of cells on the chip is defined.
In Floorplanning, the input is gate-level netlist and output is the floorplan with block placements. For the process, the size and shape of the chip is defined. Areas for various functional blocks is defined and the placement of main components and power or ground networks is planned. OpenROAD is used for the floorplanning and placement.
3. Routing
The purpose of routing is to connection of placed cells according to the netlist.
Input is the placed netlist and output is the routed design. A routing plan is created to interconnect all the cells by minimizing the wire length, delay, and congestion. Power and clock distribution networks is implemented. OpenROAD is used for global and detailed routing.
4. Design Rule Check (DRC) & Layout Versus Schematic (LVS)
The main purpose is to verify and check if the design meets the necessary requirements and ready for manufacturing. The input is the routed design and output is verified design ready for GDSII export. Design Rule Check (DRC) ensures that the layout complies with the manufacturing rules using Magic. Layout Versus Schematic (LVS) verifies that the layout matches the original netlist using Netgen. Timing analysis is used to ensure the design meets timing constraints using OpenSTA.


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

### Assignment-3

Characterization of cell-

Find 4 values

(a) Rise transition:  2.24696 ns – 2.18265 ns = 0.06431 ns.

(b) Fall transition:  4.09551 ns – 4.05316 ns = 0.04235 ns.

(c) Cell rise delay:  2.21155 ns – 2.15009 ns = 0.06146 ns.  

(d) Cell fall delay:  4.7835 ns – 4.05 ns = 0.02835 ns.
