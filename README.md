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

## Good floorplan vs bad floorplan and introduction to library cells


