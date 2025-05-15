# Sky130 Day1-Inception of open-source EDA, OpenLANE and Sky130 PDK
## How to talk to computers
### 1.Introduction to QFN-48 Package, chip, pads, core, die and IPs
#### We all might have come across a simple Arduino Board in our lives(as shown below). An Arduino board is a microcontroller-based-development platform used to build electronic projects easily. It combines a programmable chip (as shown in the encircled region) with a PCB, input/output pins, USB interface, power regulators, and other helpful components. In this project we will learn how to design this microprocessor chip.Following the steps starting from Modelling the specifications using C language, RTL(verilog/VHDL) to getting the final layout in the form of GDSII file format which is sent to the foundary.
 ![arduino image](https://github.com/user-attachments/assets/fa1decb3-2c41-45c5-ad3f-20fe69277047)

#### The above ARDUINO BOARD can also be described in the form of a block diagram. Showing the main processor(chip) along with various interfaces.
 ![block diag of arduino](https://github.com/user-attachments/assets/f038dfc4-83d8-4534-a7f1-ce3785a3e277)

#### COMPONENTS OF CHIP:
#### The chip is a QFD-48 package, QFD means 'Quad-Flat No-lead', which has terminals on four side with no pins. It includes 48 contacts which are metal pads. 
#### 1. Pads-Through whcih we can send signals inside and outside the chip.
#### 2. Core- Place where digital logic gates are fixed (eg.- MUX, AND gate, Or gate, etc.)
#### 3. Die- It is the size of the entire chip, gets manufactured on silicon wafer.
 ![components of arduino](https://github.com/user-attachments/assets/52b274be-2cee-4a92-a9d9-39a600c62068)

#### A typical chip consists of RISC-V SOC, SRAM, ADCs, DACs, PLL, and SPI. 
 ![foundary and macros](https://github.com/user-attachments/assets/613b191e-7958-43f1-8e63-6bcdde08ae54)

### 2.Introduction to RISC-V
#### RISC-V (Reduced Instruction Set Computer) is an open-source instruction set architecture (ISA) based on the RISC design principles.It was developed at the University of California, Berkeley to be simple, modular, and royalty-free— making it ideal for research, industry, and education. An ISA(Instruction Set Architecture) defines how software communicates hardware or simply, how to talk to computers. There are different basic instructions to define integer operations.
#### Given below is the layout starting from writing the C program which is then converted into Assembly Language--> It is converted into Machine Language--> Then into Binary format--> The bits are then executed in the layout.
 ![RICS-V architechture](https://github.com/user-attachments/assets/bb9be63d-7cce-4d5e-968e-4757ab9f48e2)

### 3.From Software Applications to Hardware
#### Applications that we use on a system run inside the laptop/PC which is actually the hardware, how does it happen? Here we will try to answer this question.
#### The Application software enters into block called "System Software" which converst the application into a Binary language. 
#### Now the System Software has three components: Operating System, Compiler and Assembler
#### Operating System- The OS handles all the operations, Allocate memory and the other part of OS is to take the particular app and convert it into respective Assembly Language program and finally to binary language program so that it is understood by the hardware.
#### The output of OS is given to the compiler int he form of C++/C or Java and Compiler converts it into some specific instructions(.exe file), the syntax of these instrcutiosn depends upon the hardware (e.g. the hardware belongs to RISC-V, then the instruction will be in the form of RISC-V format).
#### Once we have the respective instruction set from the compiler, the Assembler then take the instructions and convert it into the respective binary number (called as Machine Language Program).
#### The Binary data is then fed to the Hardware.
![software to harwdware data read](https://github.com/user-attachments/assets/8f54a45d-e47a-4920-a5b3-456d647900d7)
![image](https://github.com/user-attachments/assets/f78802ef-286c-49ce-aa4d-9fd667104c63)
#### The Instructions act as the abstact interface between the compiler and the hardware, this abstract interface is called the 'Instruction Set Architecture(ISA)' or 'Architecture of Computer'(PART-1).
#### For understanding instructions we need RTL(Register Transfer Level), this data is then synthesised in the form of gates(PART-2 RTL and synthesis of RISC-V based CPU core-picorv32), after this there is a physical design implementation of the data till hardware(PART-3 Physical design implementation of picorv32).
![image](https://github.com/user-attachments/assets/e1276665-d958-478c-bb0e-8c809e4e0f53)

## SOC Design and OpenLANE
### 1.Introduction to all components of open-source digital asic design
#### SoC (System on Chip) design using OpenLane refers to creating an integrated chip — including processor cores, memory blocks, I/O interfaces, etc. — using the OpenLane open-source digital ASIC design flow. OpenLane automates the RTL-to-GDSII flow to produce silicon-ready layout files.
#### For ASIC design few things which are required are: RTL IPs, EDA Tools and PDK DAta
![image](https://github.com/user-attachments/assets/a01f5257-c5f0-4042-afe8-06da63d73f5a)
#### RTL IPs(Register Transfer Level Intelectual Property) are reusable hardware design blocks described using RTL (usually in Verilog or VHDL).Example of RTL IPs is RISC-V CPU Core(like picorv32).
#### EDA (Electronic Design Automation)  is a category of software tools used to design, verify, and simulate electronic systems like ASICs, SoCs, and PCBs. EDA tools automate complex steps in the ASIC/FPGA design flow, from writing RTL to generating GDSII layout files.
#### PDK (Process Design Kit) is a technology-specific kit provided by a semiconductor foundry that contains everything needed to design chips using their manufacturing process. Example of PDK: SkyWater 130nm PDK (Sky130) – open-source PDK used with tools like OpenLane. It is an Interface between FAB and the designers. Sky130 PDK is still in use where not much advanced node is required, as the cost of advanced node for e.g-5nm node is much higher. Moreover 130nm is a fast processor for e.g.- intel:P4EE@3.46GHz(Q4'o4).
#### How they work together?  RTL IPs ───▶ EDA Tools ───▶ GDSII Layout ───▶ Foundry (using PDK) 

### 2.Simplified RTL2GDS flow
![image](https://github.com/user-attachments/assets/6570c4cc-640b-467a-bb3d-36f489f35d02)
#### Above is the PDK flow starting from RTl to GDS.
#### STEP1: SYNTHESIS- Converts RTL to a circuit out of components from the standard cell library(SCL). The resultant circuit is described in HDL(Hardware Description Language) and usually referred to as gate level netlist. A gate level netlist is functionally equivalent to RTL. The 'standard cells' have a regular layout,the cell width is variable or discrete. Each has different view/models. Examples include: Electrical- HDL, SPICE model of cells, Layout model of cells.
#### STEP2: FLOOR AND POWER PLANNING- Floorplanning and Power Planning are two critical early steps in the physical design phase of ASIC or SoC development. They define the physical structure and power integrity of the chip before placement and routing begin.It depends on whether you are implementing single component called as 'Macro', or the whole chip. The main objective is to plan the silicon Area and create the robust power distribution of the whole circuit.
#### Chip Floor planning-Partition the chip die between different system building blocks and place the I/O pads.
#### Macro Floor planning- Defines the macro dimensions, pin locations and rows.
![image](https://github.com/user-attachments/assets/5350473d-33bf-4509-be11-cd60117e1c54) ![image](https://github.com/user-attachments/assets/be0d15cd-78ef-4dda-b06d-15d584263c8f)


#### Power planning- The power netweork is constructed, typically a chip is powered by multiple VDD and GND power pins. The power pins are connected to all components through power rings and horizontal and vertical power straps. Such parallel structures are meant to reduce resistance and also addresses the problem of electromigration.
![image](https://github.com/user-attachments/assets/b09460f8-268a-44ae-8b41-46505943a939)

#### STEP3: PLACEMENT- We place the gate level netlist cells on the floorplan rows, aligned with the sites to reduce interconnected delays and enable successful routing. Done in Two steps; Global followed by Detailed Placement
![image](https://github.com/user-attachments/assets/10570d22-f6b3-4c0d-b006-e8a40edea883)
#### Global Placement- Global placement tries to find the optimum positions for all cells, not necessarily legal. The main purpose is to find the approximate locations for all cells to minimize wirelength and congestion. Cells may overlap or may go off rows.
#### Detailed Placement- Adjusts cell positions to legal locations on standard cell rows and ensures there are no overlaps. The placement obtained from global placement are altered to make it legal.
![image](https://github.com/user-attachments/assets/90aca4ef-36a5-402e-9428-274e17263d94)

 #### STEP4: CLOCK TREE SYNTHESIS- After placement comes routing, but before routing the signals we need to route the clock. It ensures that clock is delivered to all the sequential elements, e.g.- FlipFlops.Uneven clock arrival times at different registers cause clock skew, which can lead to timing violations and functional errors, therefore it ensures that there is minimum skew. Also it should be in a good shape, e.g.- H tree, X tree and so on.
#### STEP5: ROUTING- After the clock routing comes the signal routing, Routing is the step in the ASIC/SoC physical design flow where physical metal connections (wires) are created between the placed cells, macros, and IO pins. This connects the design’s logic elements (like standard cells, buffers, flip-flops) based on the netlist generated from synthesis. Routing follows placement and clock tree synthesis, and it's essential for completing the chip layout before tape-out. Given placements and fixed numbe rof metal layers is required to find the valid patterns of horizontal and vertical wires to implement the nets thst connect the cells together, the router uses the available metal layer to find the PDK. For each metal layer, the PDK defines the thickness, the pitch, the track and minimum width. Also, defines the VIAS used to connect the wires to different metal layers.
![image](https://github.com/user-attachments/assets/7b30ce12-452c-40f9-a3c9-17430a8b62b4)

#### The sky130 PDK defines 6 routing layers. The lowest layer is called the 'Local Interconnect layer' adds TiN layer and the following five layers are Aluminum layers. Most routers are Grid Routers, they construct routing grid out of the metal layer tracks. As the routing grid is huge-->'Divide and Conquer' approach is used in routing. There exist two types of routing;
Global Routing-It estimates rough routing guides, doesn't place the actual wires-just reserves the resources.
Detailed Routing-It uses the routing guides to implement the wires.It meets the DRC(Design Rule check).

![image](https://github.com/user-attachments/assets/20ec8827-6745-4883-992f-20035bf59650)

#### STEP6: SIGN OFF- Once Routing is done, we go for final checking, which includes physical and timing verification.
**Physical verification** includes Design Rule Checking(DRC) and Layout vs Schematic(LVS) ensures that the final layout matches with the gate level netlist.

**Timing Verification** includes Static Timing Analysis(STA) to make sure that all timing constraints are met.

### 3.Introduction to OpenLANE and Strive chipsets
#### For achieving the open source ASIC flow, we will be using the tool openLANE. OpenLane is an open-source automated RTL-to-GDSII flow for digital ASIC design. It is part of the OpenROAD and Skywater PDK ecosystem, enabling users to take a design from RTL (e.g., Verilog) all the way to a tapeout-ready layout (GDSII) using fully open-source tools. It has integrated tools for all the steps for ASIC design flow. OpenLANE is started as an Open source flow for a true Open -source tape-out experiment. At the Fabless, there is a family of Open everything SOCs called Strive, which includes; open RTL, Open EDA and open PDK.
![image](https://github.com/user-attachments/assets/688433de-53cd-41b0-b202-bda7de97299b)

#### The main goal of OpenLANE is to produce clean GDSII with no human intervention(no-human-in-the-loop)
#### Clean means: 
No LVS violations </br>
No DRC violations </br>
No Timing violations </br>
#### It is tuned for skyWater130 nm open PDK. Also supports XFAB180 and GF130. It can be used to harden(implement) Macrso and Chips.
#### It has two modes of operation: Autonomous and Interactive.
#### OpenLANE comes with large number of design examples , currently there are 43 designs with their best configurations.

### 4. Introduction to OpenLANE detailed ASIC design flow
![image](https://github.com/user-attachments/assets/61fbf850-0b17-4c6a-9e3c-ee36e8847401)
#### The OpenLANE ASIC flow has many steps, as explained before the flow starts with RTL design and ends with final layout in GDSII format and to function it needs PDK.OpenLANE is based on several open-source projects such as OpenRoad, yosys, ABC, Qflow and so on.
#### The flow starts with RTL synthesis, the RTL is fed to yosys with design constraints, Yosys translates RTL into logic circuits. This can be optimized usinfg library tool ABC. ABC has to be guided during optimization and this comes in the form of ABC script. Different designs can use different strategies to achieve objectives, and for that we have 'Synthesis Exploration' utility that can be used to generate reports.
#### OpenLane has 'Design Exploration' utility which  is used to sweep design configurations. It can also be used for Regression Testing(CI). We run the OpenLane on approx. 70 designs and compare the results to the best ones.
![image](https://github.com/user-attachments/assets/8f609077-39c2-42c4-8c64-ba68f478db60)

#### Next step is testing or DFT(Design For Testing) which uses the open-source tool 'Fault' to perform: Scan Insertion, Automatic test pattern Generation(ATPG), Test pattern compaction, Fault Coverage and fault Simulation.
![image](https://github.com/user-attachments/assets/801ee4ed-f17d-421e-abf7-76ba4dd6b09e)

#### Physical Implementation uses OpenROAD app and performs tasks like:
Floor/Power Planning </br>
End Decoupling Capacitors and Tap cells insertion </br>
Placement: Global and Detailed </br>
Post Placement Optimization </br>
Clock Tree synthesis(CTS) </br>
Routing: Global and detailed </br>

#### Everytime the netlist is modified, verification must be performed. CTS and post placement optimizations modifies the netlist.
#### LEC(Logic Equivalent Checking) is used to formally confirm that the function did not change after netlist.
#### During Physical Implementation, there can be 'Antenna Rule Violation', a condition where a portion of a wire (usually metal) acts like an antenna and unintentionally accumulates electrical charge during fabrication, which can damage the gate of a MOSFET connected to that wire. Therfore, the length profiles of the wire must be addressed before to avoid this issue.
![image](https://github.com/user-attachments/assets/102aa84d-7294-4f7d-b36a-367249a2eded)

#### To avoid this, there are two solutions:
Bridging: Bridging attaches a higher layer intermidiary. </br>
Add Antenna diode cell to leak away charges, antenna diodes are provides by SCL(Standard cell library). </br>
![image](https://github.com/user-attachments/assets/a9b47fe4-1eb8-4e40-872d-ade5723d7528)

#### We can also take Preventive Approach:
Add a Fake Antenna diode next to every cell input after the placement. Run the Antenna checker (Magic) on the routed layout. If the checker reports the violation on the cell input pin, replace the fake diode with a real one. </br>
![image](https://github.com/user-attachments/assets/08c70ca7-c27c-43b3-adaa-6da141eceec7)

#### Signing Off of the openLane involves STA(static timing analysis), DRC(Design Rule Checking), LVS(Layout vs Schematic):
 Static Timing analysis(STA) involves the interconnect RC Extraction(DEF2SPEF) from the routed layout, followed by STA on OpenSTA(OpenROAD) tool, resulting report will highlighting timing violations if any violations is there. </br>
 Physical Sign off involves DRC and LVS, Magic is used for Design Rule Checking and SPICE extracted from Layout. Magic and Netgen are used for LVS </br>

 ## Get familiar to open-source EDA tools
 ### 1.OpenLANE Directory structure in detail
 #### Considering some basic linux commands. We will be working in directory 'sky130_fd-sc-hd' in the 'libs.ref' file under 'pdks' folder.
 (sky130_fd_sc_hd) here sky130 is the PDK name, fd is the foundary, sc means standard cell and hd is high density variant. </br>
 ![image](https://github.com/user-attachments/assets/61896acd-acf8-429e-a639-df3b544d0388)
 
 ### 2.Design Preparation Step
 #### We will be now running OpenLane. After getting into openLane directory, type 'docker'(Docker is an open-source platform that allows you to build, run, and manage lightweight, portable containers for applications. It enables you to package an application along with all its dependencies and run it reliably across different environments).
 #### Also we will run the flow.tcl and that too with the interatcive switch so that it will run a step by srep process. If we don't use interactive switch it will run a complete flow from RTL to GDSII.
 ![image](https://github.com/user-attachments/assets/eb0031e4-4dd7-4069-a946-b029ec5e86a9)

 #### Everytime while running the openlane we need to install the package which is required, here 'package require openlane 0.9'.
 OpenLane has it's own built in designs, here we will deal with 'picorv32a' design </br>
 ![image](https://github.com/user-attachments/assets/ad460732-0a15-4825-8b77-c4d7e25f614e)
 In picorv32a we have 'src' file which has the RTL netlist</br>
 Also there is 'config.tcl' which bypasses any configuration that has been done in openlane. Many of the switches use the default that has been present in the openlane source.It overwrites the settings and become specific to the design</br>

 ![image](https://github.com/user-attachments/assets/fd6a6222-b520-401c-89ab-df33b646df34)
 ![image](https://github.com/user-attachments/assets/04a6d97f-3cff-4f10-8ca6-f99a7dd22b68)
 Here RTL file, SDC file, clock period has already been set. Also the filename has been given, but when we run our custom file 'sky130_fd_sc_hd_config.tcl' file won't be there. Openlane takes the value in the following order: First is the default value which is already set in openlane, Second is config.tcl and thirrd is sky130_fd_sc_hd_config.tcl. So the highest priority is sky130_fd_sc_hd_config.tcl, it will overwrite the default and config.tcl.This was the design part</br>
 Now we need to set up the file system specific to the flow that will be fetched from a particular location in openlane using the command 'prep -design picorv32a'.
 ![image](https://github.com/user-attachments/assets/0e2ab78f-d8ad-4f3a-b851-d716110d8931)
 #### The preparation step has been completeted



 


 


 
 

 

 

 
 
























