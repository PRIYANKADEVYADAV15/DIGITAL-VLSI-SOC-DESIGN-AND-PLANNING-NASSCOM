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

 ### 3. Review files after design prep and run synthesis
 #### After preparation is done, in picorv32a folder, runs directory is being created with today's date and time.
 ![image](https://github.com/user-attachments/assets/c4f1e778-5bbf-4280-9ef1-411e0fdeec3d)
 When we enter the date created folder, we will find all the folder structures required by the openlane. Every folder except tmp will be empty. tmp is the folder where every file is being stored. In tmp--> write command less merged.lef, it is the file which was created during preparation time which includes wire. layer levels, vias, cell level information and so on.</br>
 ![image](https://github.com/user-attachments/assets/836a1bb7-9483-4909-abb4-219b6d064c3f)
 ![image](https://github.com/user-attachments/assets/c82fffe0-e8d5-42a1-a9cf-d4d33d561879)
 #### Inside the date folder we will have results and report directory, including synthesis, floor planning, routing and so on. As we have not started the synthesis these folders will be empty. Along with this we will also see the config.tcl file, it shows all the default parameters being taken by the run.
 ![image](https://github.com/user-attachments/assets/b14b7aec-996b-45d0-819d-e6dcb2560871)

 #### Now coming back to openlane prompt, after preparation we will go for synthesis by giving command: run_synthesis
 ![image](https://github.com/user-attachments/assets/0ec91685-e0cd-492c-a80e-c2fad1fc2103)
 #### Here you can see that the synthesis is completed

 ### 4.OpenLane project Git Link description
 #### On google you can search for openlane efabless-->click on the github link
 ![image](https://github.com/user-attachments/assets/5e1b913b-4c93-4dad-9c6e-904efe5f1e8f)

 ### 5. Steps to characterize synthesis results
 #### After synthesis our first step would be to calculate the Flip Ratio;
 Flip Ratio=no. of D flip flops/ No. of cells</br>
![image](https://github.com/user-attachments/assets/70d80c2a-5ee9-4e5c-9ef0-bae2ee131ddd)
![image](https://github.com/user-attachments/assets/aeb9d607-ce40-41ea-b4f6-cb1b7f1e5183)


 Here the number of D flip flop=1613</br>
 No. of cells=18036</br>
#### Therefore, Flop Ratio=1613/18036=0.0894
#### Flip RAtio%= 8.94

#### In the results file, we can see inside synthesis, if we get the picorv32a.synthesis.v that means synthesis is complete
![image](https://github.com/user-attachments/assets/8461cf9d-058c-462d-be81-7676d6289ddc)
Also by going inside the reports, we get the statistics file where we get the number of cells and D- flip flops.

# Sky130 Day 2 - Good floorplan vs bad floorplan and introduction to library cells
## Chip Floor planning consideration
### 1. Utilization factor and aspect ratio
#### In this the first step in the physical design is to DECIDE THE HEIGHT AND WIDTH OF CORE AND DIE. We will start with the basic netlist.
![image](https://github.com/user-attachments/assets/911cecbc-fad0-4f1d-bb87-c97207ba4115)
Considering the basic netlist-->consists of two FFs(launch clock and capture clock), and gate and or gate. The given image is a netlist, a 'netlist' defines the connectivity between all the components.</br>
We are dependent on the dimensions of logic gates and FFs. We will try to give proper length and breadth to this particular gate.
![image](https://github.com/user-attachments/assets/3c12d693-42fd-4202-b94d-0d05f21c6d03)
Next, we are actually interested in finding the dimensions of core and die rather than the wires as of now.So, we will find the dimensions of the standard cells first.</br>
Considering the dimensions of standard cells as 1unit X 1unit, the Area we get = 1 sq. units </br>
With the help of netlist, we will identify the Area occupied by the std. cells on the silicon wafer. Before that Let's remove the wires and place them together(as shown below).</br>
Now the area of the netlist becomes Area= 2units X 2units= 4 sq.units</br>
![image](https://github.com/user-attachments/assets/08092309-6908-4329-8dab-ab44a59872a5)

#### What is a core and a die?
On a silicon wafer, one section is die--> inside doe there is a core(a core is a section of the chip where fundamental logic of the design is placed).</br>
(a die which consists of a core, is a small semiconductor material specimen on whcih the fundamental circuit is fabricated).</br>
![image](https://github.com/user-attachments/assets/0f1c6cb4-3d52-4d6e-ba7a-64d0809d8fd5)

#### How to arrive on it's dimensions?
Place all the logic cells inside the core, if fits completely and utilizes all the space then this is called 100% Utilization of the core.</br>
After this we come on to the concept of **Utilization factor=(Area occupied by the netlist)/ (Total Area of the core)**.</br>
![image](https://github.com/user-attachments/assets/d6cb55a3-c244-4461-b552-079de4a09570)

In the above example the Utilization factor=1, but practically only 50-60% utilization is possible.</br>
#### Another important term is 'Aspect Ratio', which is Height/Width. So in this case the aspect ratio is 1 that means chip is square. If aspect ratio is not equal to 1 that means the chip is rectangle.In such case , the remaining place is optimized by using some other circuitry.
![image](https://github.com/user-attachments/assets/100ec6cb-a7ef-4567-bc71-2fb66965b17e)

### 2. Concept of pre-placed cells
#### Let's take another example where the width and height of die is 4 units by 4 units and we have the netlist of 2units by 2 units, so if we calculate the utilization factor it will come around 25%. That means 75% of the core is empty and can be used for other optimazation, routing and wires.
![image](https://github.com/user-attachments/assets/c715a5dd-1b5e-4ca7-bfdf-c7814c8f6405)

#### Next step is DEFINE THE LOCATIONS OF PRE-PLACED CELLS, but first know about preplaced cells. Let's consider a combinational circuit(which can include mux, demux, encoder or decoder) and the equivalent circuitry consists of 100k logic gates. So we can actually minimize the gates by dividing the number of gates and turning them into different blocks. Different blocks will be implemented separately.
![image](https://github.com/user-attachments/assets/633af475-a04f-430a-a877-b1e6b74be43e)

Considering the two blocks, we separate the input output pins of both the blocks. Separate the blocks and make them a black box, the I/O pins will also be used separately. The advantage of this kind of system is that we don't have to implement the circuit multiple times, the same black box can be sent to different users for separate usage. This will reduce the number of logic gates. This a concept of Reused models.</br>
![image](https://github.com/user-attachments/assets/13d2b25e-3cc5-4483-9b2d-1915f04ad354)
![image](https://github.com/user-attachments/assets/9aace51a-e6cc-46e4-a4bc-94066323b41d)

#### Therefore, Preplaced cells are specific standard cells or blocks (typically macros or hard IPs) that are manually positioned during the floorplanning stage of an ASIC or SoC design, before the automated placement of the rest of the standard cells. This is done to fix the position of high-performance IPs or memory blocks near certain logic to minimize delay.Placing large blocks carefully reduces routing congestion.
These cells are placed in such a way that, the placement and routing tool do not touch the location of the cell in the further processes.</br>
![image](https://github.com/user-attachments/assets/5752f705-7764-4cf7-a3ae-9031105ddb7a)

### 3.De-Coupling Capacitors
#### Earlier we discussed about the locations of the pre-placed cells, that is it needs to be fixed and fill not change in the further processes also.
#### After this THE PREPLACED CELLS WILL BE SURROUNDED BY DECOUPLING CAPACITORS. Now, what are decoupling capacitors and why do we need them..?
Consider a piece of circuit as shown below. Whenever this circuit switches, e.g: AND gate switches from logic 0 to Logic 1 there is a demand of current. For this a small capacitor is placed so whenever transition from 0-->1 the capacitor charges to show 1. It is the responsibility of applied voltage(Vdd) to supply current to all the logics. Similarly when logic switches from 1-->0 the capacitor discharges to 0, for this Vss is responsible. But every physical wire has some equivalent resistance, inductance and capacitance of it's own, which leads to drop in the supply voltage.</br>
![image](https://github.com/user-attachments/assets/57dabe53-1d04-4d5b-9a9b-bf56d8a97619)

So if the supply voltage is suppose 1V then due to resistances of wire due to voltage drop the voltage reached is 0.8 or 0.7V (Vdd').</br>
The capacitor will now charge till 0.7V only. Now if the 0.7V lies between the high and low margin region, then it will be a problem as it can switch to 0 or 1 irrespective of the requirement.</br>
This is the problem of having a large distance between the main power supply and the physical circuit.</br>
![image](https://github.com/user-attachments/assets/cc398306-fa7d-472f-ae8b-b4066a783118)

#### This problem can be solved using De-coupling capacitors, De-coupling capacitors are large capacitors which are charged till the applied voltage. They are placed very close to the main circuit so that there is hardly any voltage drop. The capacitor acts as a shock absorber for the chip's power supply. It smooths out sudden jolts in voltage just like a damper absorbs mechanical shocks. So, as the name suggests it decouples from the main circuit.
![image](https://github.com/user-attachments/assets/dcd4a4b0-7f8f-458b-ac65-6ff79e634034)

#### Below image shows how the main circuit blocks are surrounded by the decoupled capacitors. This ensures that there is proper switching and no cross-talk.
![image](https://github.com/user-attachments/assets/14b2d67c-4c49-4fe2-bb06-c292f41d51fc)

### 4.Power Planning
#### Now we have taken care of local communication, but what about the global communication..?
Let us suppose there are multiple macros, and we have connected the decoupling capacitor to all the macros individually. There is a driver connected to load.</br>
![image](https://github.com/user-attachments/assets/9d5eff89-8bb5-4474-88a0-74e2cc304f79)

The macros are connected to the main power supply(Vdd). As in the diagram we can see the driver and load are connected with a 'red' wire. We want the logic operation going on in driver to be transmitted to load. But here also there will be voltage drop due to wire's resistance. Furthermore, we can't connect decoupling capacitors here as it is not feasible to connect the decouple capacitors everywhere.</br>
![image](https://github.com/user-attachments/assets/ab3865cd-a52c-4bb6-80df-dfa176be1d4c)
Let the 'red' wire represents a 16 bit bus, suppose we are giving a 16 bit signal, where for 1 the capacitors have to be fully charged till V, and for 0 the capacitors have to be discharged. Now, if we connect an inverter at the load, the 1 has to turn to 0, that means Capacitors voltage needs to be discharged to ground simultaneously. Due to discharge at single 'Ground' tap point, there will be a bump at the Ground called as 'Ground Bounce'. Which will lie in between the noise margin levels causing the disrupt in the output values.</br>
![image](https://github.com/user-attachments/assets/d53b3cb2-f285-4024-900e-04c6927990a7)
![image](https://github.com/user-attachments/assets/3672ec0e-529e-4743-a6d5-4e84c2665d26)
Now, when the capacitors are charging from 0 to 1 so they are demanding current supply at the same time, this will create a 'Voltage Droop'.</br>
![image](https://github.com/user-attachments/assets/20a8438e-06b7-471a-90fd-6aa32c02e4a6)

#### These problems occur only because there is only one power supply, if there would have been multiple power supplies then this wouldn't happen. For example as shown below.Therefore, while designing chips we give multiple power supplies. So that any logic will take it's power from it's nearest power supply and dump it's current to it's nearest ground.
![image](https://github.com/user-attachments/assets/5cc3849b-58b5-4c7c-bd03-e667dafef616)
#### This is how we do Power Planning by giving horizontal and vertical lines and the interconnects are the contacts.
![image](https://github.com/user-attachments/assets/0b31b8aa-d170-4b77-9516-9ec0f66a0a62)

### 5.Pin Placement and logic cell Placement blockage
#### Let's consider the example design which needs to be implemented, with input-output terminals and individual clocks. Later connecting the pre-placed blocks to the below logic gates.
![image](https://github.com/user-attachments/assets/2854f669-61fc-4207-9dba-8f4567d4bcb0)
Now, taking one more section of the same circuitry with two different clocks for different FFs, showcasing the concept of 'Interclocks Timing Analysis'.Also, including the pre-placed cells in between.</br>
![image](https://github.com/user-attachments/assets/2ce2a04d-7c71-4fb7-880e-326c8950cfc7)

#### Showing below the complete design
![image](https://github.com/user-attachments/assets/fe2ea2a7-f81e-4f8f-bed6-3f62c61ef62f)

#### Also connecting the clocks common to the logic gates at one place. Now, the connectivity information between the gates is coded using Verilog/VHDL language and is called as 'netlist'.
![image](https://github.com/user-attachments/assets/47f63ad2-2131-4a08-80ea-d28e97f44d5e)
Now let's see how will bw the pin placement.We need to place the logic circuit between the gap of core and die. Here, the input port is on the left and output port is on the right, but it can vary. We observe few things here, **First** the ordering of input and output ports are random depending upon the requirements. As block a is connected to D1 and D4 inout so they are placed near to that and block b is at Dout1 and Dout 3. Also as the blocks are placed at certain area so we ensure that the cell placement is not in that area.This is where the hand-checking between frontend and backend teams comes into picture. Frontend team defiens the netlist connectivity and backend defines the pin placement. **Second** thing to observe is the size of clocks is much bigger than the size of inputs and output pins. This is because the clocks is the driving source of the I/O pins, FFs and the complete chip. So we need the least resistance path, therefore bigger the size lower is the resistance. Next we will ensure that where the pin placement has been done, we need to block that area for routing and placement tool ,This is done by 'Logical cell placement blockage'.</br>
![image](https://github.com/user-attachments/assets/d2e5c67a-7f65-4b14-9318-b539fca523f2)
#### After the logical cell placement blockage step our floorplan is done for placement and routing step.
![image](https://github.com/user-attachments/assets/7e8c4491-53de-44e4-b5f2-fd94726516db)

### 6. Steps to run floorplan using OpenLANE
#### We will doing the floorplanning in openlane. For Floorplanning we require some switches which we will get in 'configuration' file in openlane.
#### Inside the configuration there is a README file--> enter into that.
![image](https://github.com/user-attachments/assets/4255e319-ee14-4af3-af47-7b18f54bc2fc)
Here you will see the variables required for each stage, such as global variable with design name, synthesis variables and so on.</br>
Different switches are given under floorplanning as shown.</br>
![image](https://github.com/user-attachments/assets/3ca286f2-8b02-4126-805c-113ff8461340)

Now we need to set the switches, for that go back to the README file, in the floorplan.tcl directory we will see the default switches which are being set already,
We can see (FP_IO_MODE):1 means that the i/o pins is positioned randomly but at equidistant, for 0 means not positioned equidistant.</br>
![image](https://github.com/user-attachments/assets/18bb6493-894b-43ca-8755-585617702055)

Now we have seen earlier that in the openlane the lowest priority is given to system default(floorplane.tcl) then config.tcl and the highest priority is given to PDK variant.tcl(sky130A_sky130_fd_sc_hd_congig.tcl).</br>
#### We will now run the floorplan by giving command: run_floorplan.
![image](https://github.com/user-attachments/assets/ef771304-4605-4e73-9467-713af73c670a)

### 7.Review floorplan files and steps to view floorplan
#### As we have run the floorplan, just like we did for synthesis we will go inside picorv32a and check for the present date when the floorplan file was created. Then we will go into the floorplan, and open the directory 'ioplacer.log' and we did the placements in input output.
![image](https://github.com/user-attachments/assets/2bbd1f12-8fad-4e24-9051-9a1a648a76db)

Inside the configuration we will se the default floorplan.tcl file which will show the default settings.</br>
To see how the floorplan looks like, we will go inside the created file by running floorplan-->results-->floorplan we will se a .def(design exchange format) file, open the def file</br>
![image](https://github.com/user-attachments/assets/a364bda2-3721-42b3-853c-1f0091a3a27d)

After opening the file we will get various parameters, the diarea which is given in databased unit. We need to convert into microns.</br>
1 microns = 1000 database units, so given DIAREA (0 0) (660685 671045), converted into microns becomes,</br>
**Width= 660.685 microns, Height=671.045 microns**
![image](https://github.com/user-attachments/assets/9b852944-2da2-4ab3-ac77-d20de8ab2a79)

#### To see the actual Floorplan, let us first open Magic by writing the command `magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def`
We will see the layout in magic</br>
![image](https://github.com/user-attachments/assets/dc261fb9-d323-4d18-8995-dd1de0304790)
![image](https://github.com/user-attachments/assets/f9dbe342-18e8-4ea4-b7c7-fad7c1b2151d)

### 8.Review floorplan layout in Magic
#### In the above image we can see the layout is not at the center, so to fit in the center-->full screen the window-->Press s-->press v. Then the layout will fir in the window.
#### To zoom in first left click on mouse, then right click and press z,
#### Similarly to zoom out press shift + z.
Also as we have selected IO_mode=1 that means I/O pins are placed equidistant with each other.</br>
To select any pin just hover the mouse over that element and press s on keyboard.
![image](https://github.com/user-attachments/assets/de28d5eb-f1b2-4361-a45e-fb40fe4a619a)

After selecting any pin, there is one more window tkcon, where we can get the information of the selected pin. Just type 'what' in that window.You will see metal3 which means horizontal.</br>
![image](https://github.com/user-attachments/assets/2dbe3b58-8ef8-4eaf-810b-a0dd26f420d8)

Similarly, we check for vertical pins we will get metal2 as mentioned in below image.</br>
![image](https://github.com/user-attachments/assets/042a55bf-2bf6-46de-b559-8256ecd80f93)
#### Along with this we can also see the Decaps(decoupled capacitor) arranged along the border or side rows.Then we have tap cells, which are used to avoid latch-up conditions in CMOS devices, they connect n-well to VDD and substrate to GND.
![image](https://github.com/user-attachments/assets/d0af6c3e-8a04-42da-85c0-bb89c8370a9d)
#### We also have the standard cell at the lower left corner which represents the AND, OR,etc logic gates.
![image](https://github.com/user-attachments/assets/272caf5f-dd25-43e9-b337-9cd2eb8ae25f)

## Library building and Placement
### 1. Netlist binding and initial place design
#### In Placement and Routing , the first step to bind the physical netlist.In reality the logic gates do not actually have the shape as in which they are shown instead they are represented as a box with certain width and height which is given during designing. So now each and every component of the netlist is now given a proper dimension.
![image](https://github.com/user-attachments/assets/74c18e1c-aa12-4477-926d-1a87faae9f29)

Now Removing the wires and placing the elements together. These things are present in shell called "Library." Library has the timing information, basically there are 2 types of libraries one tells about the delay and second tells about the shapes and size of the shell.</br>
So, the library will have the delay of the particular shell, it's width and height, also it's particular information at which condition it will be operated.</br>
Library provides various options about the size.Like in the second case, the gates are bigger in size-->less resistance path-->so faster. Similarly in third case it is even faster.</br>
![image](https://github.com/user-attachments/assets/0d065fd6-ba54-4934-bd5b-910a3508a4e8)

#### Now next comes the Placement of the desired netlist on the floorplan. So we have the floorplan, the netlist and the shape of components in netlist.
![image](https://github.com/user-attachments/assets/88440514-109c-482a-8498-50fa89309b50)

#### Considering the floorplan that we have along with preplaced cells, we will start placing the FFs by looking at the netlist. As in the netlist the FFs1 is near to Din1 and FF2 is at Dout1 so we will place accordingly.They are placed closed to each other to avoid timing delay.
Also, the stage 2 of logic, you can see that all the FFs and gates are placed together.</br>
![image](https://github.com/user-attachments/assets/962c3ebe-0e3f-42c2-9c1c-4681abbc8762)
Now, in the netlist we can see that FF1 is close to Din2 and FF2 is close to Dout3, the distance is quite large.They are arranged diagonally.</br>
Similarly, in the next stage the FF1 is at Din4 and FF2 is at Dout4, but there is preplaced cells that cannot be moved, so it will be arranged as shown in the image below.</br>
![image](https://github.com/user-attachments/assets/b23a1bee-89fe-4d4a-9ed8-e25f0509ac77)
But we need to solve this problem by Optimizing the placement.

### 2. Optimize placement using estimated wire-length and capacitance
#### This is the stage where we estimate wire length and capacitances and based on that we insert repeaters. Like at Din2 to Dout3  we try to estimate the wire length and then the equivalent capacitance. As the distance is very high, the resistance and capacitance will be very high, due to which there will be loss of signals. To avoid this we place repeaters and buffers at the intermediate distances to maintain 'signal integrity'. But there will be loss of area due to so many buffers and repeaters, anyhow we need to still live with it.
In stage 1 the FFs are near so there is no need to place repeaters.(as shown below)</br>
![image](https://github.com/user-attachments/assets/31e06c8b-94e6-4e3c-8b94-a6c1c07eff63)
In stage 2 the FF1 is far from Din2 so we need buffers/repeaters in between to maintain the signal integrity. So we place 2 buffers in between.(as shown below)</br>
![image](https://github.com/user-attachments/assets/43b5d19b-9f90-4df4-aef3-5686331998e0)

### 3.Final placement optimization
#### In stage 2 you can see that there no space between the FFs and Logic gates, this is called 'Abetment in placement optimization', this is done when a particular circuit has to run very fast(high frequency application) so there should'nt be any wire placement in between to avoid delay.

Similarly in stage 3 we need to place a buffer between logic gate 2 and FF2 as the distance appeared is high.</br>
![image](https://github.com/user-attachments/assets/2d2475b3-85d0-4992-bed0-2d99ea17018d)

Coming to the last stage i.e 4th stage, it is the trickiest one, we placed 2 buffers in between, and also there is a criss-cross with other connections in between. So we need to deal with that also further.</br>
![image](https://github.com/user-attachments/assets/60e86193-d89b-4922-a405-b75401f0af34)

#### Now we will try to do the Setip Timing Ananlysis, considering the clocks to be ideal that means giving clock to all the FFs at the same time.

### 4.Need for libraries and characterization
#### We have come across the steps for designing i.e. Logic Synthesis, Floorplanning, Placement... and the last step STA(Static Timing Analysis). To reach there we need to accomplish a very important step i.e. CLOCK TREE SYNTHESIS(CTS).
For 0 skew, FFs everywhere should receive the signal at the same time. Clock Buffers will make sure that the signal received is at the same time. This is where Lbraries come into picture.</br>
Also one common thing in all the stages is Gates/Cells(AND, OR, BUFFER, INVERTERS...).Here Library Characterisation is very important where there is collection of gates/cells. We need this because tools should understand what a specific gate is-->For what we need to model the gate in specific way to make the EDA tool understand the logics of gates.

### 5.Congestion aware placement using RePlAce
#### At present we are more interested in ensuring that the congestion free placement, later we will consider the timing analysis.
We have earlier seen that placement occurs in two stages- global placement--> detailed placement</br>
In Global Placement legalization does not happens, it happens in Detailed Placement. Legalization means the standard cells are placed in standard cell rows, they have to be exactly inside and abeted(closely packed) with each other. Also no overlapping, Legalization involves timing.</br>
So while we do run_placement in openlane-->1st global placement happens-->the main objective of this is to reduce wire length and in openlane we use concept og HPWL(Half parameter wire length).Also, Our main motive is to converge the overflow, if it does then the placement is done.</br>
To physically check if placement is done, go into results folder and check fro placment, a placement.def file will be created.Open the file in magic using the same tech file as used earlier</br>
![image](https://github.com/user-attachments/assets/caeb49a9-5bd0-4b8a-b32e-bd2fad2a9d04)

We see the standard cells which were actually at the bottom left corner of the floorplan are now placed in the standard cells of rows.
![image](https://github.com/user-attachments/assets/17b85da2-d657-4498-94b8-f75a629dc0a0)

## Cell design and characterization flows
### 1. Inputs for cell design flow
#### What are standard cells in typical IC design flow?
Standard cells are pre-designed and pre-characterized logic gates and other fundamental building blocks used in the physical design of integrated circuits (ICs). They form the foundation of digital IC design</br>
Standard cells are: Logic gates (e.g., AND, OR, NOT),Flip-flops, latches,Buffers, inverters, multiplexers, and Special cells (e.g., tie-high/low, filler cells)</br>
![image](https://github.com/user-attachments/assets/ac37e946-1591-4db6-938b-8b3b52babd8b)
These standard cells are placed in Libraries. A library has got cells with different functionality, and different sizes. Also cells with different threshold voltage(Vt).
![image](https://github.com/user-attachments/assets/f74fe2f9-38c5-4a68-9f22-705bff6c2a02)
Let's take one particular inverter-->see the cell design flow, this inverter should be understood by a particular EDA tools.It has to be represented in form of shape, size and various cell design flow.</br>
**Cell design flow is divided into 3 parts:**
a)Inputs</br>
b)Design steps</br>
c)Outputs</br>
![image](https://github.com/user-attachments/assets/100d9c77-dc25-426f-8162-2d8c2c50e89f)
![image](https://github.com/user-attachments/assets/0863ccf8-c7eb-4227-8ecf-b1130242a596)
![image](https://github.com/user-attachments/assets/b905a337-2be2-4ec1-92c3-6e4292c356b7)

### 2.Circuit design steps
Consider an example of 'Library' in inputs-->the separation between power rail and ground rail decides the cell height. It is the responsibility of cell library that the cell height is maintained. If 'Drive strength' of a particular cell is high, it will be able to drive even longer wires.</br>
![image](https://github.com/user-attachments/assets/92f6205d-216d-4d71-ab47-c7329baf8924)
Let's take an example of 'User defined specifications'--> The top level of the cell decides at what level the chip will operate and the library developer has to decide the supply voltage. The library also has to decide the Metal layer and Pin Locations.</br>
 **Design steps** - Now coming on to the design steps after defining the inputs in the library, we need to design in such a way which adheres to the inputs.</br>
 Design involves three steps-</br>
 i)Circuit design</br>
 ii)Layout design</br>
 iii)Characterisation</br>

 #### In circuit design step we need to follow two steps, first is to implement the circuit and second is to model the PMOS and NMOS transistor in order to meet the library requiremenents. The output we get from circuit design is called CDL(Circuit Description Language)
 ![image](https://github.com/user-attachments/assets/d7832a90-32da-4d7d-ac39-d3610e5ed537)
Next step is the Layout design.</br>

### 4.Layout Design
#### The first we already discussed that is implementation of the given function, the second step to derive the pmos and nmos network graphs. This is done by 'Art of Layout-Euler's path and stick diagram'. It will give the best layout and best performance.
After we are done with the network graphs, we get the Euler's path. **Euler's path** is the path which is being traced only once. Based on Euler's path we draw a stick diagram out of it.</br>
![image](https://github.com/user-attachments/assets/08e57318-9171-4fa6-8576-bd6e4a15261c)
![image](https://github.com/user-attachments/assets/241f0a49-1057-4d21-b087-204858ef441d)
Next Step is to convert the stick diagram into a proper layout adhering to the DRC rules. We can implement it in magic.(as shown below)</br>
![image](https://github.com/user-attachments/assets/1e9253b3-c1b9-4e49-8bb3-5836f40f0f13)
The next step and the final step will be to extract the parasitics(resistances and capacitance) from the layout and charcaterise in terms of timing.The layout desing will be saved in the output in the form of GDSII, LEF, and extracted spice netlist</br>
Next step is very important that is Characterisation and we will get the output in the form of timing, noise and power information.</br>

### 4.Typical Characterisation flow
#### Let us try to built in characterisation flow from the inputs we have, there are certain steps we need to follow:
i)   Reading the model files</br>
ii)  Read the Extacted SPICE netlist</br>
iii) Recognizethe behaviour of buffer</br>
iv)  Read the sub-circuit of inverter</br>
v)   Attach the power sources</br>
vi)  Apply the stimulus given to the characterisation step</br>
vii) Provide the necessary output capacitors</br>
viii)Provide the necessary simulation command i.e. For transition simulation-->.tran and for DC simulation-->.dc</br>

![image](https://github.com/user-attachments/assets/2fb95e00-e0fe-469f-b47c-d5b7a5162cff)
![image](https://github.com/user-attachments/assets/0e6b80c0-8dac-4ad4-88f4-1f4617ea6611)
Next is to feed all these steps in characterisation software called GUNA.This software will generate timing,noise and power.libs outputs</br>
![image](https://github.com/user-attachments/assets/b35b945a-c82c-4882-a016-3df862030470)

## General timing characterization parameters
### 1. Timing threshold definitions
#### Here we will understand various syntex and symentix of timing.lib, power.lib and noise.lib. This is necessary to understand the GUNA software.
We will try to understand the timing threshold definitions of waveform itself.</br>
![image](https://github.com/user-attachments/assets/21857667-3857-47e6-8ae1-30d4dd9a1df7)
Waveform of output of 1st inverter is given as input to 2nd inverter.</br>
**slew_low_rise_thr** It is voltage level below which a rising signal is considered to have started it's transition. Or we can say that slew low rise threshold depicts the value close to 0.slew_low_rise_thr is typically 20% from bottom power supply.</br>
![image](https://github.com/user-attachments/assets/8e60e7e6-e03d-426d-a808-26eb67de51d6)

**slew_high_rise_thr** It is typically 20% from top power supply</br>
![image](https://github.com/user-attachments/assets/0824e11f-a960-4ff5-b7d0-e3f9fcf3d6c5)

**slew_low_fall_thr**
![image](https://github.com/user-attachments/assets/12b65a3a-2790-491b-ac3b-1182239f7580)

**slew_high_fall_thr**
![image](https://github.com/user-attachments/assets/c16f693a-5310-4c07-a5da-5bac2b3d8dfe)

Now the other definitions include input waveforms,taking the input stimulus and the output of the first buffer.

**in_rise_thr** It tell the delay from the given input, to measure the arrival time of a rising signal at the input pin of a standard cell.It is taken when the input crosses 50% of the signal.
![image](https://github.com/user-attachments/assets/59213ea0-727e-41d1-87f9-64935032e6dc)

**out_rise_thr** Just like input rise, output rise threshold is also 50% of the output waveform.</br>
![image](https://github.com/user-attachments/assets/e753cb31-3859-47de-b53f-f6c993418a1b)

**in_fall_thr**
![image](https://github.com/user-attachments/assets/8051be12-6da9-4a8f-a875-1a4835f33880)

**out_fall_thr**
![image](https://github.com/user-attachments/assets/c0cf64e8-87a0-4e0b-96e3-7f332859cd85)

### 2. Propagation delay and transition time
We have in&out_rise_thr and in&out_fall_thr. So if we want to calculate delay--> time(out_*_thr)-time(in_*_thr)</br>
![image](https://github.com/user-attachments/assets/62986c2b-2986-4336-a955-b34355feac0f)
Lte's take an example, here the Red curve is input waveform and blue curve is output waveform taken from 2nd inverter.
![image](https://github.com/user-attachments/assets/f65564f5-c994-4d34-95d3-293804641c39)

Next if we shift the threshold points above 50%, then we will see that there is a negative delay sha shown below. A negative delay means output arrived befor the input, so it is not good. Therefore choosing a proper threshold point is very very important.</br>
![image](https://github.com/user-attachments/assets/d827a8ba-d1dd-45c7-9869-3b7fcdc1d5c1)

Another example of negative delay is given below, here the input slew is too high due to long wires.</br>
![image](https://github.com/user-attachments/assets/2cccbd7c-0f40-450d-a0e3-3613206835e1)
We can see that in_rise_thr point is much higher than out_fall_thr point which results ina negative delay.</br>
![image](https://github.com/user-attachments/assets/83e2121a-cf39-4563-add6-5d415152526a)

Next we will understand the transition time which is given by--> time(slew_high_rise_thr)-time(slew_low_rise_thr), similarly for the fall-->time(slew_high_fall_thr)-time(slew_low_fall_thr)</br>
Let's consider 20% of VDD as low value and 80% of VDD as high value</br> 
So here comes the slew rate i.e. high-low for input and output characteristics.</br>
![image](https://github.com/user-attachments/assets/b93723e5-4624-4d86-9bc1-592b17628047)

# SKy130 Day 3 - Design library cell using Magic Layout and ngspice characterization
## Labs for CMOS inverter ngspice simulations
### 1.IO placer revision
#### As we have taken the example of an inverter, we will be designing the cell. We'll load the magic file into the picorv32a.Till now we have already done with the floorplan, now also we can change the core utilisation factor.
Open the floorplan that we got, we have put earlier FP_IO_MODE 1, so we got equidistant input output pins, now let's change the configuration and see what happens.</br>
Inside floorplan.tcl we have env(FP_IO_MODE) 1, now write in openlane as set 2 as shown below. **And do the run_floorplan again**.</br>
![image](https://github.com/user-attachments/assets/2439a713-a453-4883-9356-7f864ce80ce4)
We observe that now all the pins are stacked together in the lower half and no pin in the upper half.
![image](https://github.com/user-attachments/assets/9f9e2d23-0cc5-44c5-bdd2-9444d030fbb8)

### 2.SPICE deck creation for CMOS inverter
Now we will be doing some SPICE simulations and deriving the real time mosfets.</br>
1st step is **SPICE deck**, it is the *connectivity information* about the netlist. It has got the inputs provided for simulation, tap points at which we'll take the outputs and so on. So we will create the SPICE deck for complete netlist with pmos and nmos.In this case we looking at the 'static behaviour' of the cmos.Next, we will define the *component value*, where the pmos and nmos are given W/L values, and output capacitance load value.(Although we know pmos should be wider than nmos, but here we will take the same values for both). </br>
![image](https://github.com/user-attachments/assets/e382a5e4-519d-40e8-8c00-66c6354a1425)

Next step is to give the input values.Usually the voltage kept is in multiples of channel length.Also assume the draain voltage.
![image](https://github.com/user-attachments/assets/22986514-3101-4346-a8c6-63d899bb561f)

Next step is to *Identify the Nodes*, when between two points there is a component then that is specified as a node.</br>
![image](https://github.com/user-attachments/assets/3eb87fa1-cabf-44be-ab4e-2a5380514745)
Let's *name the nodes*.</br>
![image](https://github.com/user-attachments/assets/701baa5b-0b67-40f7-b9fa-987648ec00a3)

We will start writing the SPICE deck code.</br>
Stars define the ***commands***, The syntax will be Drain-Gate-Source-Substrate</br>
![image](https://github.com/user-attachments/assets/ad3a3fc4-8ad9-472b-82d6-323353796080)

### 3.SPICE simulation lab for CMOS inverter
#### Now we will into the other components, taking the output capacitance. cload is connected between out and 0 and it's value 10fF.
![image](https://github.com/user-attachments/assets/9486fce5-cf99-4cde-84e3-3eab33682d52)

Similarly the supply voltage VDD connected between Vdd and 0.<br>
![image](https://github.com/user-attachments/assets/4fab48d8-874a-496f-9b5d-4cfb037adc74)
SImilarly the Vin connected between in and 0.<br>

#### Now we will write the simulation commands. 
Given command means sweep the input voltage from 0 to 2.5 at steps of 0.05. reason of doing this , we need to calculate the voltage at the output while we sweep the input this will give the *Transfer Characteristics*.</br>
![image](https://github.com/user-attachments/assets/27679ff2-8317-41ab-b2a7-6a82e4d0f9b4)
Final step is to 'Describe the Model file', this is very important step.This has got the complete description of pmos and nmos transistors including all the technological paramters. From this file only it will take all the description.</br>
![image](https://github.com/user-attachments/assets/e39338f7-1d81-497c-8ceb-28b709b6e212)

We will now try to plot the waveform in the ngspice using the model file we have.</br>
Here Wn,p=0.375microns, Ln,p=0.25microns; Wn/Ln=Wp/Lp=1.5W</br>
We get the required Transfer Characteristics.</br>
![image](https://github.com/user-attachments/assets/5085a256-1620-41b6-93e6-33fefce46172)

Next keeping the Wn=0.375 microns, Wp=0.9375microns; Ln,p=0.25 microns; Wn/Ln=1.5, Wp/Lp=3.75.</br>
![image](https://github.com/user-attachments/assets/a6f9deae-c064-412b-9b13-b500f7c7cc73)
We can see in dc1 the waveform is a bit shifted left from it's center, whereas dc2 is accurate.

### 4. Switching Threshold Vm
#### Previously. in the first case we took Wn/Ln=Wp/Lp=1.5, whereas in second case we took Wp/Lp > Wn/Ln. Clearly we saw waveform shift in the second case. Both have different applications.
Even though we changed the width/length ratio we saw the graphs is same in both the cases, this shows that CMOS Inverter is a robust device.The behaviour of the inverter remains same despite of the changes.</br>
We will do the Static behaviour Evaluation showing the robustness of the CMOS Inverter. The parameters which define the same are;</br>
![image](https://github.com/user-attachments/assets/ad50a822-acaf-463c-ba6b-177d154b03ee)

1) **Switching Threshold, Vm**- It is a point where Vin=Vout, we will draw a tangent and see at what point Vin=Vout. This will give Vm. Also Vm is the point when both PMOS and NMOS are ON(Saturation Region). In this region there is a leakage current which flows from Vdd to ground.</br>
 Given below we got the Vm in both the cases.</br>
   ![image](https://github.com/user-attachments/assets/050271a5-e07f-40ff-bc75-07c18dbb2b13)
At this point *Vgs=Vd*s, that means *Vgs>>Vt*, also the current which flows from PMOS and NMOS are same It's just that the direction of current are different.
![image](https://github.com/user-attachments/assets/009d1106-5f6f-4500-b260-96bde5dbf8b9)

### 5.Static and dynamic simulation of CMOS inverter
Now we will try to prove the robustness of CMOS Inverter with different W/L ratios in SPICE simulator and calculating the Vm.</br>
![image](https://github.com/user-attachments/assets/9f33e5f1-4139-43a6-8f37-43e2ac39c96a)

Earlier we did the static simulation by command `.dc`, now we will be doing the dynamic simulation by writing the command `.tran` and the input provided will be a pulse.</br>
![image](https://github.com/user-attachments/assets/4eff5692-42c8-48f3-84eb-7f94f11dec80) ![image](https://github.com/user-attachments/assets/2038aebe-f299-447c-82cc-fe376da06415)
![image](https://github.com/user-attachments/assets/913620b8-1e90-4036-859e-8c95f88f06f5)
We will calculate the rise delay and fall delay from the graph we obtained.</br>
So at Wp/Lp=Wn/Ln, we got the rise and fall delay as shown below, with switching threshold Vm=0.99V</br>
![image](https://github.com/user-attachments/assets/5c04d9e5-3280-4e80-804a-901f9d18e18b)

### 6. Lab steps to git clone vsdstdcelldesign
We can the git clone the repository where there is skywater SPICE modulation files for NMOS and PMOS.</br>
To get the git clone, first go to the github page, copy the url under code and paste the url under the openlane directory using the command `git clone`.</br>
This will create a folder called **vsdstdcelldesign** inside the openlane directory.</br>
![image](https://github.com/user-attachments/assets/c23acdb2-22d9-4457-9413-d475f04a3783)

Now we will be extracting the CMOS Inverter and doing the SPICE simulations.</br>
First we will copy the sky130A.tech file inside the new folder created usinf the command `cp`.</br>
![image](https://github.com/user-attachments/assets/3592779d-9195-49aa-916b-58e45303eba1)
![image](https://github.com/user-attachments/assets/baa442ad-b1cc-4876-83f6-81cf55565b98)
You can see that is has been copied.</br>
![image](https://github.com/user-attachments/assets/671bb5bf-6d66-4cd3-96a6-9465d6b3163f)
We will now see the layout in magic. Also don't need to write the whole code as we have copied the tech file inside the present working directory</br>
Also '&' after writing the command is used to free the magic for next prompt.</br>
We have got the layout of the inverters below.</br>
![image](https://github.com/user-attachments/assets/7aa0ce8b-cce2-4e79-8ce9-67a2371f8a80)

![image](https://github.com/user-attachments/assets/86dbe57e-3b2d-4c0d-af6a-9608cb64050f)

## Inception of layout ̂ÃÂ CMOS fabrication process
### 1. Create Active regions
#### We will create a 16 mask CMOS process.
1) **Selecting a substrate**- THe complete layout is laid onto a substrate,here we will select the most commonly used substrate i.e.a ptype Si substrate.</br>
![image](https://github.com/user-attachments/assets/a0bdb010-bd73-479d-8068-641c0bae441d)
2) **Create the active regions for transistors**- Active regions are the pockets where we will dope with n type.</br>
 For this we need to create the isolation so that the pockets do not interact with each other, so we will grow a ~40nm SiO2 layer on the substrate.</br>
 Next we will deposite a ~80nm layer of Si3N4 on top of SiO2.</br>
 Now to make the active region pockets we will deposit the ~1micron layer of photoresist to create the masks.</br>
 Where we want to create the wells there will put masks.</br>
 And UV light drom the top.</br>
 ![image](https://github.com/user-attachments/assets/1fb3633b-26d0-4210-afe0-2e287854859d)
 ![image](https://github.com/user-attachments/assets/961f419e-2000-4ca4-9bb8-623eb091fe35)
 ![image](https://github.com/user-attachments/assets/09ab4cdd-730e-4beb-b012-08d6c182e81f)
 ![image](https://github.com/user-attachments/assets/38a0215d-cc95-4590-8735-33f41f55afc8)
After this the extra regions that were being exposed to the UV light are washed away.</br>
![image](https://github.com/user-attachments/assets/2a1acc7d-6a51-46ae-b48e-d3a92b81d54c)
Next step is to remove the mask and etch out the exposed area. The area which has photoresit will be saved from the etchant.</br>
![image](https://github.com/user-attachments/assets/355de28f-7ec5-49c4-9732-abc3e1461d26)
After this the resist is also removed and we place the substrate into high temperature furnace to grow the SiO2 layer on the exposed area.</br>
![image](https://github.com/user-attachments/assets/9b5b93f4-6a55-43e8-a209-9190a756c2df)

Si3N4 was able to protect the areas underneath it, but couldn't protect the edges.</br>
![image](https://github.com/user-attachments/assets/99d99c6d-b85f-49b4-931c-dd9f92875331)
Now the transistors which will be fabricated are now isolated, this process is called 'LOCOS' Which is 'local oxidation of silicon', and the area which protects transistor from communicating is called 'Bird's Beak'.</br>
![image](https://github.com/user-attachments/assets/26b3b682-8dcc-4755-9b4f-2800355b5ed5)
Also the Si3N4 will be stripped out using hot phosphoric acid, resulting in an isolation layer.</br>
![image](https://github.com/user-attachments/assets/4e44c2f3-e0b0-4fd0-9140-91e1a8d2a946)

### 2. Formation of N-well and P-well
   3)**Creation of nwell and pwell**- n well will be used for PMOS fabrication and pwell will be used for fabrication of NMOS. Both cannot be formed at the same time, so we need to protect one to make the other and vice versa.</br>
   ![image](https://github.com/user-attachments/assets/93ac4c80-2a36-4cc8-894a-4ea4d3a2d54e)
   We will expose the substrate with UV light.</br>
   ![image](https://github.com/user-attachments/assets/1792a332-706a-417d-be27-1f26264cb5c0)
   The area which does not have the mask washes away with the photoresist, and the remaining area is protected.</br>
   ![image](https://github.com/user-attachments/assets/fbddcdc5-d6c2-4a5a-ab80-f9ff117035a2)
   Next, remove the mask and create a p well using Boron with the help of ion implantation using ~200kev energy.So, the boron diffuses into the oxide layer in the p substrate.</br>
   ![image](https://github.com/user-attachments/assets/de3130a1-aa35-499f-9411-f8471f808f7e)
   Similarly we will create an active n region/ n well by ion implanting phosphorus. The energy required by photsphorus is bit high ~400kev because phosphorus is heavier than boron.</br>
   ![image](https://github.com/user-attachments/assets/2c456d28-5766-4e02-bede-b80f6a3ba9b0)
   N-well and P-well are created, but the depth is not finallised so we need to diffuse it under high temperature to create uniform depths.Pockets will be created, this is called 'twin-tub process'</br>
   ![image](https://github.com/user-attachments/assets/679dc40b-b102-49ba-8ae2-13f0e086f7ae)

   ### 3. Formation of gate terminal
   #### 4) Gate Formation
   We will now form the gate. Gate is a very important terminal as that's where we control the threshold voltage.</br>
   To control Vt, we need to control the doping concentration and Oxide capacitance 
   ![image](https://github.com/user-attachments/assets/984658aa-ea6f-4082-87e8-68944acd6b97)

   Now we will put the Mask4 above the photoresist and expose the remaining area with boron at energy ~60kev. This time the energy is low because we want the boron to be at the surface and penetrate so much.Also we want the doping concentration to be precise for gate formation.</br>
   ![image](https://github.com/user-attachments/assets/6f3e6235-dc71-4205-a460-4b602e40df5c)
Similarly we will put Mask5 on the remaining side and dope with Arsenic. Just like p type we will control the energy here also, to get the required threshold voltage.</br>
![image](https://github.com/user-attachments/assets/b89f0165-172c-4538-8ebd-131194aefda6)
Due to so many implantation steps, the oxide layer underneath gets damage, so we will remove the oxide layer using HF and again regrow the layer ~10nm.</br>
![image](https://github.com/user-attachments/assets/2a825c1c-4bb3-44de-a7f6-937d1daf234a)

We will now fabricate a thick layer ~0.4 micron of polysilicon,and expose to very light Ntype (arsenic or phosphorus) layer by ion implantation for low gate resistance.</br>
![image](https://github.com/user-attachments/assets/9e313fb4-9845-4086-8518-36c53abd8ae9)
Then we will deposit Mask6 on top.</br>
![image](https://github.com/user-attachments/assets/47d20211-309b-48e0-952d-236e5d079df0)
Expose to the UV light, which washes away the exposed area, and the remaining area that was out from the photoresist is etched away.In this way we will get the polysilicon gate.</br>
![image](https://github.com/user-attachments/assets/24981305-a913-439a-b50c-ca40c7b7983d)

The remaining photoresist is removed. We will get the substrate, the oxide layer and controlled gate kayer of polysilicon.</br>
![image](https://github.com/user-attachments/assets/8268a5fd-bbd2-45b4-9058-d82906674e67)

### 4.Lightly doped drain (LDD) formation
#### 5)LDD formation
For PMOS we are tyring to build the P+,P-,N doping profile, where source is P+ doped, drain is lightly doped and substrate is N type. SImilarly for NMOS the doping profiles are N+,N-,P.</br>
This profile is maintained due to two reasons:</br>
**Hot Electron effect**- when device size reduces-->Electric field increases(E=V/d)-->high energy carriers break Si-Si bonds--> this energy crosses 3.2eV barrier between Si conduction band and SiO2 conduction band.</br>
**Short channel effect**- due to low devices size-->gate length is changed from 1micron to 0.5 micron-->the drain area penetrates into channel area-->difficult for gate to control current between source and drain.</br>
![image](https://github.com/user-attachments/assets/6a57bead-f8e6-4605-9708-3c2ed3bc0acf)
After creating the Mask7 and creating impurity of Ntype over pwell, and due to ion implantation and by controlling the doping concentration we get the N- implants.</br>
![image](https://github.com/user-attachments/assets/56cc0be3-e01e-4542-a1d7-f56c985e4926)
Now we will create Mask8 and protect this layer and expose the other layer with Boron such a way that P- implants are created.</br>
![image](https://github.com/user-attachments/assets/f863da4f-50d4-4a45-a7f4-e5a01f6325c0)
But the actual structure will be affected by these implants, so to avoid this we will create 'side wall spacers' by depositing a thick SiO2 or Si3N4 layer on the gate terminal.Then doing the Plasma anisotropic Etching to remove the oxide layer. This etching does not remove side walls, there it will create "side wall spacers'.</br>
![image](https://github.com/user-attachments/assets/f2feb1f7-0efe-44ec-b04d-6aa5ff42eb09)

### 5. Source and drain formation
#### 6) Source and drain formation
Before the formation of Source and Drain,a thin screen layer of oxide is deposited to avoid channeling effect.</br>
![image](https://github.com/user-attachments/assets/798902cf-db37-44f1-be22-a49656eb7383)

For Source and Drain formation, we deposit make Mask9 on n substrate and exposing the p substrate to Arsenic with energy ~75eV. The side wall spacers will protect the LDD so that channeling does not happen.We will get the N+ structure required.</br>
![image](https://github.com/user-attachments/assets/7b348110-d73a-47e7-9908-38ae1dc28645)
Similarly we will mask this layer using Mask10 and expose the n substrate to Boron.</br>
![image](https://github.com/user-attachments/assets/7979e346-51e7-4a78-9c92-3e9f9ebee48f)
Now we will put the structure under high temperature for Annealing, it will push the dopants more inside the substrate and there will be uniform distribution.</br>
![image](https://github.com/user-attachments/assets/0f3dac2f-e21f-4a93-a3fc-5bb82808e83e)

### 6. Local interconnect formation
#### 7) Steps to form contacts and interconnects(local)
Contacts are really important, as these are the only users a user can connect to the circuit.For thsi first we will etch out the thin oxide layer for avoiding channeling effect using HF.</br>
![image](https://github.com/user-attachments/assets/8d18c933-a264-443c-8947-5a97381b359d)
For creating local interconnects, first we will deposit Titanium suing sputtering process all over the substrate.</br>
![image](https://github.com/user-attachments/assets/814699e6-760a-4bb3-9091-97ad6b91eb4a)
Next step is to create the connects between titanium and source drain. This is done by heating the wafer at an ambient temperature of 600-700 degreese celcius under N2 gas for 60sec. This will result in TiSi2(a low resistive metal contact on gate) contacts created on source and drain. Also TiN layer will be formed, it is used only for local communication.</br>
![image](https://github.com/user-attachments/assets/d4a34079-cf24-47ff-a3dd-7f996cb6bd4d)
To bring up the required contacts on top, we will put Mask11 and etch out the area we want to be coming out. We want Source, Drain and Gate area to be coming out.</br>
![image](https://github.com/user-attachments/assets/489eeb8c-e0c9-45c8-b2c5-ccac42ecac94)
We will etch out the extra TiN layer using RCA cleaning.</br>
![image](https://github.com/user-attachments/assets/1175a90c-49d6-4451-8dc1-3756c2dba141)
![image](https://github.com/user-attachments/assets/8186100f-304b-4b8b-ba25-a457f87d3f56)

### 7. Higher Level Metal Formation
#### 8) Higher level metal formation
Here we observe there is non planarity which is not good for depositing higher metal interconnects.So we will planarise this surface by using thick layer of SiO2 which is doped with phosphorus and boron. The reason of doping is that phosphorus protects the layer from reactive sodium ions and boron is used to reduce the temperature as this wafer will be exposed to certain high temperature so boron will help in reducing the temperature.</br>
![image](https://github.com/user-attachments/assets/cbfbaafe-3b02-42b0-8cc5-68551107595b)
To remove th hills and bumps we do polishing, CMP.</br>
![image](https://github.com/user-attachments/assets/a2f002e7-81a0-4a0a-b050-09b458dcc118)
Next is creating the metal contacts by drilling, so this also done by photlithography technique. By using Mask12.</br>
![image](https://github.com/user-attachments/assets/b0b855b8-a272-41b4-8f25-c0a8f161a333)
Now we will remove the mask by washing away the photoresist. We will create  thin layer ~10nm of TiN, it acts as Adhesion layer between SiO2 and acts a barrier layer for bottom and top interconnects.</br>
![image](https://github.com/user-attachments/assets/7ff6c2ab-5dbd-4397-8167-294ad99e53cc)
Then we will  deposit a blanket of Tungsten(W) layer, this will help to create a very good contact from bottom to top.</br>
![image](https://github.com/user-attachments/assets/6f25a73b-811f-413d-bc2f-8d856874d739)
NExt, is CMP, removing the extra tungsten from top.</br>
![image](https://github.com/user-attachments/assets/f706e0a8-03d4-413f-8e8a-8c8ab4be1474)
Now we will deposit metal Aluminium layer on top to take the metal contacts above. Further we will mask to expose the specific areas.</br>
![image](https://github.com/user-attachments/assets/4e09044c-6e21-4023-b3c2-d9fee381d166)
![image](https://github.com/user-attachments/assets/67ef4b08-5da7-4b54-bc59-6df84022c897)
We got the first layer of metal interconnect below.</br>
![image](https://github.com/user-attachments/assets/2e113595-a846-4b33-8c90-5bc0b38c9be5)
We will repeat the above processes to get further layer of metal interconnects.</br>
![image](https://github.com/user-attachments/assets/171d3200-c2cc-4305-9e55-4fdca7533125)
![image](https://github.com/user-attachments/assets/ef56e117-0ade-431b-bbef-db17a8a93ddf)

After Mask14 again a thin layer or TiN is deposited.</br>
![image](https://github.com/user-attachments/assets/7ec2ce83-ec2a-4c50-ab25-9b7bebab8d8d)

Now agaian depositing Tungsten(W) on top.</br>
![image](https://github.com/user-attachments/assets/83bb5f50-60eb-4679-806e-35770d7ccd89)

Now we will deposit the third layer of interconnect using Mask15. Also the thickness is more than the bottom layer. When we go from bottom to top the thickness of metal layers increase.</br>
![image](https://github.com/user-attachments/assets/73c05b80-3087-4c3d-bf15-8d29d3d0ebec)

After this we will deposit the Si3N4 layer, we use Si3N4 to protect the chip as this is a good protectant layer from the outside world.</br>
![image](https://github.com/user-attachments/assets/e30a6a1e-670b-4017-8e48-72cdd1b615b7)

Finally, we will use Mask16 to drill out the final contacts outside.</br>
![image](https://github.com/user-attachments/assets/2a00a984-9319-4e4f-8305-06cf85d25078)

### 8.Lab introduction to Sky130 basic layers layout and LEF using inverter
The layers which we see here are required for basic CMOS inverter.The above one is PMOS and below is NMOS, Red line is Polysilicon.</br>
On the riight side we see color palatte which shows the layers.</br>
In skywater130A the first layer is local interconnect layer which is shown by light blue colour, the purple colour is Metal 1, pink color is Metal 2, n well is shown by solid slanting dashed lines
![image](https://github.com/user-attachments/assets/845516c4-7292-4734-8bf2-4bb5867d91cf)

We know when a poly crosses n-diffusion it's an NMOS and similarly when a ploy crosses p-diffusion it's a PMOS.</br>
We can check this if it holds true or not by selecting that part and type 'what' on tkcon.</br>
![image](https://github.com/user-attachments/assets/22c007b7-e923-415b-89b6-084268542079)
Similarly we can do for PMOS as well.</br>

Now to check if the PMOS drain is connected to NMOS drain, in magic press s three times after placing the cursor over drain.</br>
![image](https://github.com/user-attachments/assets/d00ed81c-9d5a-445c-a8bb-e9b95b48c75a)

Also in CMOS the source of PMOS is connected to VDD and source of NMOS is connected to GND.</br>
![image](https://github.com/user-attachments/assets/31508655-a747-4706-a1fc-c9a1c1874dda)

![image](https://github.com/user-attachments/assets/46a73564-b1d5-4137-810a-0b7515e45352)

### 9.Lab steps to create std cell layout and extract spice netlist
The CMOS inverter we see is being taken from the repository <a href="https://github.com/nickson-jose/vsdstdcelldesign">https://github.com/nickson-jose/vsdstdcelldesign</a>
Also, we need to ensure the final design needs to be DRC(design rule check) clean.</br>
How to know the logical involved in formation of inverter? FOr that we will extract SPICE and do SPICE simulations in ngspice.</br>
For this we will go to tkcon, and see where we are by `pwd`, then we will type `extract all`.</br>
![image](https://github.com/user-attachments/assets/aa20ea2a-65b7-4c8a-86fe-ae251bc3a681)

We will see if that has been extracted. So `sky130A_inv_ext` is present</br>
![image](https://github.com/user-attachments/assets/241f4757-8eaa-4de3-b7e2-aa710bd57569)

Next we will use this `.ext` file to create our SPICE file which will be used in ngspice tool.In tkcon we will write the command `ext2spice cthresh 0 rthresh 0`, this will extract the parasitic capacitances and resistances, then we will write `ext2spice` and enter.</br>
![image](https://github.com/user-attachments/assets/b1f1853f-66f8-4f69-bbaa-c0e668c36905)

After we will see that spice file has been created.</br>
![image](https://github.com/user-attachments/assets/8c96490c-a992-46aa-9fe8-f9e35e2b9661)

We will now check what's there inside the spice file.</br>
![image](https://github.com/user-attachments/assets/784f818e-ad7f-453e-b3ed-c13603935350)

## Sky130 Tech File Labs
### 1.Lab steps to create final SPICE deck using Sky130 tech
#### Let us try to read the spice deck.</br>
![image](https://github.com/user-attachments/assets/fa0e7a72-0c28-4c5d-affa-8d427a9c244a)

We need to take the dimensions as the dimension of grid in spice model that we have extracted. SO we will edit the values in SPICE deck according to what mentioned</br>
![image](https://github.com/user-attachments/assets/a62fa8ed-83de-4f5c-a469-9f497edc5e79)
Also include the pmos and nmos files which are there in the libs folder. Use command `.include ./libs/pshort.lib` for PMOS and `.include ./libs/nshort.lib` command for NMOS.</br>
![image](https://github.com/user-attachments/assets/c695afcf-234f-4349-9a75-3e341dad7647)

Now make the definition for the supply voltage `VDD VPWR 0 3.3V` , `VSS VGND 0 0V`, and Input files `Va A VGND PULSE(0v 3.3V 0 0.1ns 2ns 4ns)`. Also add the command `.tran 1n 20n`, `.control` , `run`,`.endc`,`.end`.</br>
Also add the model files of nmos and pmos.</br>
![image](https://github.com/user-attachments/assets/1be025dc-4ed4-4c35-ae5a-cbc279b27dcc)


Now our SPICE deck is ready, run `ngspice sky130_inv.spice`.
![image](https://github.com/user-attachments/assets/f825708a-837c-46ba-9948-1ae1eb06f365)

Now to plot the graph: `plot y vs time a`.</br>
![image](https://github.com/user-attachments/assets/4fbb0bd8-652d-4249-beb8-6d7ecfaad8a4)

The graph we get is shown below.</br>
![image](https://github.com/user-attachments/assets/f76439d1-40ea-4ff7-9cf8-7e2a084fe434)

we can see some spikes. So we will load the spice file again, C3 change 0.24fF to 2fF.Again run ngspice</br>
![image](https://github.com/user-attachments/assets/d0af0835-6675-4173-abd9-eadb3bbdb11b)




### 2.Lab steps to characterize inverter using sky130 model files
#### We need to find different parameters; 'rise tran', 'fall tran', 'propagation delay', 'fall cell delay'
 a) **rise tran**-time taken by o/p to transit from 20% of VDD to 80% of VDD.</br>
    ![Screenshot 2025-05-20 014229](https://github.com/user-attachments/assets/c9a106e2-628c-4156-ab7e-dc1c8b767e56)
    ![image](https://github.com/user-attachments/assets/56558b6e-6256-42b0-8270-e88d98ed062f)
    The rise time=(2.245-2.181)ns=64ps</br>

 b) **fall time**-time taken by o/p to transit from 80% of VDD to 20% of VDD.</br>
    ![image](https://github.com/user-attachments/assets/bcfc0e8b-9a4d-4d48-bc41-c32fde699d57)
    the fall time=(8.01307-4.052)ns=3.96ns</br>

 c)**cell rise delay/propagation delay**-time difference between 50% of i/p and 50% of o/p when output is rising.</br>
     ![image](https://github.com/user-attachments/assets/9879dc05-ad26-47f9-af26-10f318b228fe)
     The cell rise delay=(2.21036-2.1496)ns=60.76ps</br>

 d)**cell fall delay**-time difference between 50% of i/p and 50% of o/p when output is falling.</br>
     ![image](https://github.com/user-attachments/assets/f19dd9dc-9ced-4027-801e-6adce28c202c)
     The cell fall delay=(4.077-4.04988)ns=27.2ps</br>

#### Therefore, we successfully have done the process calculations and characterize our inverter. Next we will create a LEF file and plugin the LEF file into picorv32a.

### 3.Lab introduction to Magic tool options and DRC rules
#### We need to understand the DRC rules.
For this we can go to website: <a href="http://opencircuitdesign.com/"> http://opencircuitdesign.com/</a> , and learn about Magic tool and various DRC rules.</br>
To know about skywater130 pdks: <a href="https://www.skywatertechnology.com/sky130-open-source-pdk/"> https://www.skywatertechnology.com/sky130-open-source-pdk/</a>.</br>
Github repository for skywater-pdks: <a href="https://github.com/google/skywater-pdk"> https://github.com/google/skywater-pdk</a>. </br>

### 4. Lab introduction to Sky130 pdk's and steps to download labs
#### To download layout and DRC documentation:
To download lab files for DRC documentation.</br>
`wget http://opencircuitdesign.com/open_pdks/archive/drc_tests.tgz`</br>
`tar xfz drc_tests.tgz`</br>
Open `drc_tests` file in this folder.</br>
![image](https://github.com/user-attachments/assets/a966c705-06a3-49e2-93ec-805346079a6d)
![image](https://github.com/user-attachments/assets/101d93e1-26bb-49fd-a431-6432fb20b44d)


Do `ls -al` to list down what is there inside.</br>
There is a .magicrc directory in this, open it using `vim .magicrc`.It is the starup for magic, it verifies the technology file for magic.Although not suggested to make any changes in this directory.</br>
![image](https://github.com/user-attachments/assets/96e66e62-a81f-4d7d-aa00-48edb62825c6)



### 5.Lab introduction to Magic and steps to load Sky130 tech-rules
We can start the magic with visual graphics using the command, `magic -d XR &`.</br>
From the file-->open-->met3.mag.</br>
![image](https://github.com/user-attachments/assets/954ba083-1d2a-4031-86e5-00cacd83cda2)

Here it shows number of independent layout, in which most of them show DRC errors.Each of these examples have a name and these names are called out in google_skywater_documentation.</br>

For writing the commands we have the tkcon window, type `drc why` in the tkcon window.In return we will see why the DRC rule has been violated</br>
![image](https://github.com/user-attachments/assets/7e86ae4b-99de-4095-bdda-80288327a96b)

Now select a large area by left and right click of mouse, then hover over m3contact from colour palatte-->press p-->then on tkcon type `cif see VIA2`.</br>
We'll see a bunch of black squares, these are metal contacts.</br>
![image](https://github.com/user-attachments/assets/7948cd33-0e05-47d8-b16a-48f06c64721c)

### 6.Lab exercise to fix poly.9 error in Sky130 tech-file
This will be the first exercise of the lab.</br>
In magic load the file `load poly.mag`.</br>
![image](https://github.com/user-attachments/assets/7f4cfff2-c374-4f41-bbf2-2c1b085add1c)
![image](https://github.com/user-attachments/assets/e488850f-a02a-45f7-9ebc-38d4f7e1a324)

We need to find the error. We look for the file sky130A.tech inside the drc_tests folder.</br>
Search for poly.9 by pressing esc-->write /poly.9--> we found 2 matches.</br>
![image](https://github.com/user-attachments/assets/1cdd6d30-3b7a-4547-aedf-970aa32f0c29)
![image](https://github.com/user-attachments/assets/b9430f7e-c5a6-47db-8b7a-41d2be1180fd)

Make the required changes, and save the file</br>
![image](https://github.com/user-attachments/assets/df855984-e8d1-497b-8625-1b6f2b22b1a1)
![image](https://github.com/user-attachments/assets/068452eb-820a-4f3c-9242-542cdc7ae3f7)

To check, the run the command on tkcon command window `tech load sky130A.tech`.</br>
then type command `drc check`, to check if it matches with the existing DRC rules.</br>
![image](https://github.com/user-attachments/assets/7cdce110-2256-4db9-ba74-fa95cd631558)

### 7.Lab exercise to implement poly resistor spacing to diff and tap
There are more changes to be made:</br>
We will copy the 3 resistors and check for different varieties of diffusion and tap, we will check for n-diffusion, p-diffusion, n-tap and p-tap.</br>

# Sky 130 Day 4- Pre Layout timing analysis and importance of good clock tree
## Timing Modelling using Delay Tables 
### Lab Steps to convert grid info to track info
Till now we are done with floorplan and placements. Also given `.mag` file --> we have seen how toextract spice model and do the characterisation. </br>
For placement and routing in openlane, we need not require the entire .mag file, we just need the inner and outer boundary, power and ground rails and input and output ports.</br>
This is where LEF(library exchange format) file comes into picture. LEF file basically protects the IP. So, our next objective to extract the LEF file from `.mag` file and then the extracted LEF file would be plugged into picorv32a flow.</br>
**There are certain guidelines to be followed while making standard cells.** </br>
1) The input and output port must lie on the intersection of verical and horizontal tracks.</br>
2) The width of standard cell must be 'odd' multiple of track 'horizontal' pitch and the height should be the 'odd' multiple of track 'vertical' pitch.</br>

Now what actually a track is? For this go to `openlane_working_dir/pdks/sky130A/libs.tech/openlane/sky130_fd_sc_hd/` , there is a file `tracks.info` --> open the file.</br>
![image](https://github.com/user-attachments/assets/dcddeea8-fab9-40f0-9ece-06cd6f4ee906)
A track is a virtual line along which routing tools can place wires (routes) for connecting different pins or logic cells. Tracks exist on each metal layer, and are defined by:</br>
Direction (horizontal or vertical)</br>
Pitch (distance between adjacent tracks)</br>
Starting coordinate</br>
Tracks help create a grid-based routing architecture, simplifying and standardizing the routing process.</br>
Routes are the metal traces, like traces of li1, metal1 and metal2 and so on.</br>
PNR is automated, so we need to specify where we want the routes to go. This specification is given by tracks. Each of the tracks is placed at (offset, pitch) i.e. (0.23, 0.46)um horizontally and (0.17, 0.34)um vertically for li1, metal 1, and metal 2 layers.</br>
![image](https://github.com/user-attachments/assets/b4383c72-5eb1-49ae-ab38-5c0c24047b05)

Now if we talk about the first rule-->the input and output ports must lie on the intersection of vertical and horizontal tracks. In the image below we can see the the ports are on li1 layer. To ensure that the ports are on the intersection of the tracks, we will need to convert the grid into the tracks.</br>
In magic if you press `g` the grids get activated. You can a small black box when we press `g`. These are the references of the layout, Now we will convert the grid into tracks and verify whether A nd Y ports lie on the intersection of horizontal and vertical tracks of li1.</br>
![image](https://github.com/user-attachments/assets/ef01a91e-20e8-42a8-b702-56feef39ee23)

For this we will go to the tracks file and open tkcon, write `help grid`, we will get to know what all arguements a grid needs.</br>
![image](https://github.com/user-attachments/assets/5c2e9599-e8d9-47fa-b243-3b149d90f8a9)

Here we can see that we need [xspacing [ yspacing [xorigin  yorigin]]], here x and y spacing are the pitch given in li1 layer for x and y. The origin is offset value for x and y. Therefore make the grid according to that.</br>
![image](https://github.com/user-attachments/assets/2e05a7b0-7e4e-48e0-85eb-10e3214d2d76)
Press `enter`, We will see the grids are coverted into tracks, also if we zoom in the port A and Y lies on the intersection of horizontal and vertical tracks.This will ensure that the route can reach the traqack from x and y directions.</br>
![image](https://github.com/user-attachments/assets/f2ea20eb-8e49-4ff4-ab2f-ee234f91fc34)

### Lab steps to convert magic layout to standard cell LEF
Now the second requirement is that the width of the standard cell must be odd multiple of x direction pitch. We can see that here the width is multiple of 3 to the horizontal pitch. Similarly we will check for the height of the standard cell</br>
![image](https://github.com/user-attachments/assets/1bdcb464-3ecf-4f79-a42a-7f412f2181a0)
After this we need to decide the port names and values, they provide nothing but the pins in the layout. For this we need to select the layer where port is needed to be defined--> go to Edit--> go to text </br>
![image](https://github.com/user-attachments/assets/690f7c7d-3128-4660-bd58-a33b0774d6e7)
Give the required values.</br>
![image](https://github.com/user-attachments/assets/f9c04729-53b0-4f20-8068-c2cbc056dacc)

Similarly we need to define the ports for other layers as well. To define port A is an input port and Y is an output port we define `port class` and `port use`. We can further get the information at `https://github.com/nickson-jose/vsdstdcelldesign` </br>
Once these parameters are set, we are ready to extract the LEF file from .mag.</br>
Also save the mag file as the name you want it to be saved.</br>

![image](https://github.com/user-attachments/assets/a6e3fede-53e1-4f25-8b14-75b14117d335)

![image](https://github.com/user-attachments/assets/bc7aa4c8-f1f0-483e-aac4-8352c1be2c77)

Open the `.mag` file in magic created in vsdstdcelldesign folder.</br>
![image](https://github.com/user-attachments/assets/c692680c-a5fd-428c-9fdb-452139557a2e)

To extract LEF, write the command `lef write` in tkcon. The lef file will be created in the same name as magic file i.e. `sky130_vsdinv.mag`.</br>
A lef file will be created under vsdstdcelldesign.</br>
![image](https://github.com/user-attachments/assets/1587954d-792d-4df9-ba33-f413b29d885a)

If we open the lef file, we will see what all changes we did in the magic.</br>
![image](https://github.com/user-attachments/assets/560a9498-76ed-4d14-9431-44289dcb9204)
Now we will pluf in these files in picorv32a folder.</br>

### Introduction to timing libs and steps to include new cell in synthesis
We have created the lef file, now we will copy the lef file into src folder where all the designs are kept at one place. Copy the lef file by using command `cp` to the location of src.</br>
![image](https://github.com/user-attachments/assets/d58618c2-5b27-40f7-85a9-b21697d96b7f)

The lef file has been included.</br>
![image](https://github.com/user-attachments/assets/736815c3-cc1c-401a-a207-7e50ae9e1c6e)

We need to include our custom cell into openlane flow and first stage in openlane is 'synthesis'. We have to ensure that the ABC maps netlist to the cells in the library. So we need to have the library, which has our cell definition for synthesis that also has been included ss the part of vsdstdcelldesign.</br>
![image](https://github.com/user-attachments/assets/15c1c563-760f-446d-8287-9730b1bcdf17)

This is how a library looks like, it has complete characterisation of the cell, soecifying the cell_rise, cell_fall time.</br>
Also we have different library files as typical, fast and slow files for different voltages, temperature etc.</br>
![image](https://github.com/user-attachments/assets/ee1d7bde-e3c8-46fd-8049-a20768a7f12e)

Now our next objective is that the tool should map vsddtdcelldesign during the synthesis of slow. So for that we will copy the libraries in the src.</br>
![image](https://github.com/user-attachments/assets/2e8596fd-b962-4efc-aa32-3ef250e4e65b)
![image](https://github.com/user-attachments/assets/24a8bc89-accf-4336-87f4-447ad5043224)

Now we need to modify our confing.tcl which in picorv32a folder.</br>
![image](https://github.com/user-attachments/assets/b5fa0374-7d74-485c-97bc-688e76a673ee)

Write the commands as shown below in the image.</br>
![image](https://github.com/user-attachments/assets/ec66f2cc-4db9-4679-9eee-e2b3835faed9)

The modifications to be done are:</br>
1) In the design's config.tcl file add the below line to point to the lef location which is required during spice extraction.</br>
    `set ::env(EXTRA_LEFS) [glob $::env(OPENLANE_ROOT)/designs/$::env(DESIGN_NAME)/src/*.lef]` </br>
2) Include the below command to include the additional lef into the flow:</br>
    `set lefs [glob $::env(DESIGN_DIR)/src/*.lef]`
  
    `add_lefs -src $lefs`

In Openlane we will run the following steps:</br>
```docker```
```./flow.tcl -interactive```
```package require openlane 0.9```
```prep -design picorv32a -tag 27-06_06-12 -overwrite```
```set lefs [glob $::env(DESIGN_DIR)/src/*.lef]```
```add_lefs -src $lefs```
```run_synthesis```
Now we will ckeck if the openlane maps the LIBS_SYNTH from config.tcl</br>
![image](https://github.com/user-attachments/assets/eba51239-d9ff-49b1-899f-bc89d9795556)

### Introduction of delay tables
**Power Aware CTS** - In logic And gate if one input is connected to logic '1' and other input is at Clk, then the output will be equal to the clock given i.e. the clock will propagate at the output. Also if we give logic '0' then at the output the clock will be blocked. Similarly in an OR gate if one input is logic '0' and other input is clk, then at the output the clock will be propagated and at logic '1' the clock will be blocked.</br>
![image](https://github.com/user-attachments/assets/7c9ca04a-ea34-431c-ac10-451fac3ab781)
The main advantage we can get out of the this is that for the rest of the circuit there will be no switching and power supply consumed at that point of time. It will save a lot of power.</br>

In the below circuit, we can see Splitting the complete load using buffers. Also after "Clock gating", will the cicuit work according to our requirements?</br>
We will swap the buffer with AND and OR gates and see how the characteristics change.</br>
![image](https://github.com/user-attachments/assets/f04ac2ee-8e6e-4381-b5e1-b87b319666a4)

Before swapping, we made some assumptions and taken observations:</br>
![image](https://github.com/user-attachments/assets/52c6a92a-73a5-4c13-a196-6b68f239abfc)

We have observed that the output capacitance of the buffer for the entire circuit is not constant, load at the output will be varying and since the load is varying so input transition is also varying. Due to this there will be a variety of delays, so how to capture that?
For this we have 'Delay Tables'. These are 2 Dimensional Tables, prepared by taking specific buffer and while keeping the input slew rate constant and changing the output capacitance, delay is observed and noted down the value. We make delay tables of each and every cell of buffer, and this becomes the 'Timing Models' of buffers.</br>
![image](https://github.com/user-attachments/assets/7c59743f-9a7c-4d7e-8039-6e788a5cb50c)

### Delay Tables usage part 1
Delay tables are a representation of delay for a particular cell, similar table we'll be having for each and every kind of gates i.e. AND, OR, NOT... gates will have individual delay tables.</br>

Let us take the delay table for buffer of size 1, suppose we want to find the delay for input slew rate 60ps and output load as 70Ff, so the delay will be the value where both intersects. Similarly we can find the delay for buffer of size 2 as well.</br>
Also with sizes we mean the W/L ratio of pmos and nmos which are there in the buffers.</br>
We will try to calculate the delay of buffer with size 1, here input slew rate is 40ps and output load is suppose 60Ff, as it lies between 50Ff and 70Ff the values are extrapolated from the table.</br>
Let the delay be x9' for buffer of size 1.</br>
![image](https://github.com/user-attachments/assets/1f930af6-71a3-44d8-b9f2-75de995c838b)

### Delay Tables usage part 2
Now let us consider the buffer of size 2, we got the delay for that buffer as x15. The output delay for one part of buffer will be x9' + x15. Similarly if the buffer is same in second part, the delay will be same and when we'll calculate the 'Latency' at the output there will be no Skew. But for different delays in both the parts, the Skew won't be 0. This can lead to Propagation delay, Static hold time delays and so on.</br>

### Labs step to configure synthesis settings to fix slack and include vsdinv
Let's do some modification in README folder under configuration file.</br>
![image](https://github.com/user-attachments/assets/99acf63f-e3a4-4baa-b305-6592c36d0edf)

After we ran the synthesis, we can see slack time delay which is very high. Slack is a measure of timing margin — it tells you how much “extra time” or “delay buffer” a signal path has before violating its timing constraint. </br>
Slack = Required Time – Arrival Time </br>
Arrival time < Required time → Good!</br>
Arrival time > Required time → Bad!</br>
Let's make the synthesis 'Timing Driven'. We will try to find the balance between delay and the area.</br>
At present the chip area is as shown below, we need to make balance using synthesis strategies.</br>
Also the 'Total Negative slack'(tns) and 'Worst Negative slack'(wns) is too high here.</br>
![image](https://github.com/user-attachments/assets/a6f89a8d-53f4-4222-9234-cf6260618f86)

If we set the strategy to 1, the area should increase a little bit but the timing will improve.</br>
We will set the strategy by writing `set ::env(SYNTH_STRATEGY) "DELAY 3"` , this will set the strategy to delay 3.</br>

Now we will set the SYNTH_BUFFERING, it Inserts buffers automatically on nets that drive too many loads (high fanout).Helps to reduce delay, improve signal integrity, and meet timing closure.</br>
We will check if the buffer strategy is on or not ```echo $::env(SYNTH_BUFFERING)```.</br>
We will also check if cell sizing is on or not by ```echo $::env(SYNTH_SIZING)```</br>
Set the driving cells as well; this ensures that there are driving cells at the input in case of high fanout.</br>
AFter writing the following commands, when we did `run_synthesis`, we got the following errors.</br>
![image](https://github.com/user-attachments/assets/20e8e33e-f5aa-40ad-bc8f-7edbc06b9675)

To rectify this we need to follow the steps below:</br>
```prep -design picorv32a -tag 29-06_05-12 -overwrite```

```set lefs [glob $::env(DESIGN_DIR)/src/*.lef]```

```add_lefs -src $lefs```

```echo $::env(SYNTH_STRATEGY)```

```set ::env(SYNTH_STRATEGY) "DELAY 3"```

```echo $::env(SYNTH_BUFFERING)```

```echo $::env(SYNTH_SIZING)```

```set ::env(SYNTH_SIZING) 1```

```echo $::env(SYNTH_DRIVING_CELL)```

```run_synthesis```

These steps creates a netlist which has a reduced slack and the chip area has increased with, tns and wns has reduced to 0.</br>
![image](https://github.com/user-attachments/assets/470acd24-e49f-4062-b669-8d5330eb8fd0)
![image](https://github.com/user-attachments/assets/f695d549-2bb7-4457-8661-d411873d4326)

AFter the synthesis is successfully run, we will now do ```run_floorplan```.</br>
But we got the error as shown below,</br>
![image](https://github.com/user-attachments/assets/68ffe417-7745-4346-927c-955825d5dde6)

To rectify this we will run the following commands,<br>
```init_floorplan```

```place_io```

```tap_decap_or```
![image](https://github.com/user-attachments/assets/36cb8e20-45e6-463b-b4b8-78803484bb60)
![image](https://github.com/user-attachments/assets/2eeeb7fd-0a41-428e-92cc-7f355e81424e)
![image](https://github.com/user-attachments/assets/bd5fbbf4-505f-4ba8-9592-4e4d7324bbe8)

After this, we are now good to run placement, ```run_placement```.</br>
We will see the iterations, the overflow should decrease with increasing iterations.</br>
![image](https://github.com/user-attachments/assets/919c5461-af5f-4570-9805-8bdfbb3ccea7)

The Placement ran successfully.<br>
![image](https://github.com/user-attachments/assets/41570bec-e985-4acf-8b3e-6492629fba01)

We will find the vsd_inv cell here. We will see the overlapping, means Abettment which means the power and ground rails are shared between the cells.</br>
![image](https://github.com/user-attachments/assets/206ea60c-fc45-4faa-8ef4-e716bacb2af7)

Now in tkcon we will write ```expand```.</br>
![image](https://github.com/user-attachments/assets/aa8534d9-50f3-48aa-ad43-6d846cd37012)

## Timing Analysis with Ideal clock using open STA
### Setup timing Analysis and introduction to Flip-Flop Setup time
We will now take the ideal clock first, to introduce about Timing Analysis then we will introduce the real clocks and do the Real Timing Analysis.</br>
Let us consider a 'Single clock' which refers to a timing path where both the launch flip-flop and the capture flip-flop are triggered by the same clock signal.</br>
Given the clock specifications as: 
**Clock Frequency(f)=1GHz** </br>
**Clock period(T)=1/f=1ns** </br>
Now what is actually "Setup Timing Analysis", It ensures that data launched from one flip-flop arrives at the next flip-flop before the clock edge that is intended to capture it. The check is done between a rising edge (or falling edge) of the same clock for both launch and capture.</br>

Now will do the analysis between '0' and 'T' clock period. We sent at edge to the launch flop at '0' clock period and at T=1ns period the second edge reached to capture flop.</br>
Let's say here we have combinatonal delay of theta and set up timing analysis says that this combinational delay should be less than the T for system to work properly.</br>
![image](https://github.com/user-attachments/assets/6e98a0e9-d9f6-4d72-bf7a-6ca0d6cb3a32)

Now we will introduce more practical scenarios in it,</br>
The first thing to be considered is the Flop itself. If we open a particular flop, there is a lot of combinational circuits in it. There are several Mosfets, Capacitances and gates inside it.</br>
![image](https://github.com/user-attachments/assets/59c9f5db-2a30-465c-b6f4-4ca9f29cd4be)

Inside the capture clock, we can see 2 multiplexers. So when the clock is '0', the delay considered will be only of Mux1 whereas if he clock is '1' the delay of both the Multiplexers Mux1 and Mux2 is considered.</br>

So there is some finite amount of time which is required to the D input to settle and this amount of time is reffered to as SET UP TIME.</br>

Hence finite time 's' required before clk edge for 'D' to reach Qm.</br>

So, we can write that the internal delay of the MUX1 = set up time(S).</br>

So, now θ<T becomes θ<(T-S).</br>

### Introduction to clock jitter and uncertainity
Next practical scenario is called "JITTER". The clock is being created by PLL(Phased-locked loop) and the clock is expected to send signal at t=0, 2T, 3T, ..., but the clock source created out of PLL is made using wires; so there is resistance and capacitances. Due to which it is not expected to give clock edge at exactly t=0ns. So there is clock edge present which is in built. This is called "Jitter".</br>
![image](https://github.com/user-attachments/assets/1d9e3544-109e-411c-9489-301926978db1)

So initially the period was from 0 to T, now it is shrinked to less than 0 and T. This temperory variation even though very less can cause failures in circuit. And also we have kept θ<(T-S), now this will also change. Jitter has to be modelled in the above equation only. </br>
Now let's model Jitter using a second parameter i.e. "Uncertainity". we will club both time for clock edge and uncertainity.</br>
![image](https://github.com/user-attachments/assets/33ba8829-5fd9-49ad-846d-43c2696b7b98)

So, earlier the combinational delay was supposed to be less than (T-S) will now become θ<(T-S-US), where SU = Setup uncertainity time.<br>
![image](https://github.com/user-attachments/assets/7fbf3d1c-16f0-4134-9abe-bf0f841fbd99)

Let us take an example; T=1ns, S=10ps=0.01ns, SU=90ps=0.09ns.</br>
The required combinational delay(θ) = 1ns-0.01ns-0.09ns= 0.9ns.</br>
With this timing analysis let's try to identify the timing path from the existing scenario.</br>
![image](https://github.com/user-attachments/assets/fa8eb62c-07b9-4438-8ac8-d46cf710843b)

![image](https://github.com/user-attachments/assets/edf65b38-bc27-48b3-b8ce-2807b02bce60)

As calculated above the combinational delay expected to be is θ<0.9ns.Once we assure that the delay obtained is equal to the expected delay, then we will be assured that the placement done is correctly optimised according to the requirements using "single clock".</br>
Next challenge we need to undergo is similar kind of timing analysis using "Multiple clock".

### Labs steps to configure OpenSTA for post synth Timing Analysis
To reduce the slack and timing violations in Openlane, we require a separate tool which is "OpenSTA".</br>
Before that do the synthesis;
```docker```

```./flow.tcl -interactive```

```package require openlane 0.9```

```prep -design picorv32a```

```set lefs [glob $::env(DESIGN_DIR)/src/*.lef]```

```add_lefs -src $lefs```

```set ::env(SYNTH_SIZING) 1```

```run_synthesis```
![image](https://github.com/user-attachments/assets/1fddcbd8-af8f-4ae3-bac8-0c0e7bfe8010)

Now, we need to create a new ```pre_sta.conf``` file under openlane folder using text editor or simply vim editor.</br>
![image](https://github.com/user-attachments/assets/fd216646-b4de-41be-84e0-e2cca704fc51)


Under the folder ```/openlane/designs/picorv32a/src/```, we need to create the file ```my_base.sdc```.</br>
![image](https://github.com/user-attachments/assets/6b7504d8-8e1a-49bd-bd2b-f70b41f8cd4b)


The changes in above file is same as in ```base.sdc``` which is in folder ```openlane/scripts```.</br>
The ```pre_sta.conf``` file is the file where we'll be doing the Pre-layout Timing Analysis.</br>

We will do ```sta pre_sta.conf``` as shown below.</br>
![image](https://github.com/user-attachments/assets/d1713c12-921f-437a-b20d-2b83a09a9e7d)

![image](https://github.com/user-attachments/assets/d0a42eeb-ea74-4895-9706-babe276d8834)
![image](https://github.com/user-attachments/assets/1c34f164-9cac-4313-a48a-619e26fd0aae)
![image](https://github.com/user-attachments/assets/405b2d9a-d494-4e33-8fab-89cb3e63bb7e)

### Lab steps to Optimize synthesis to reduce setup violations
The Delay of any cell is number of input slew and output load, so more the I/P slew more will be the delay and similarly more output capacitances more will be the delay.</br>
We will now try to optimize the value of fanout by ```set ::env(SYNTH_MAX_FANOUT) 4```, do the following steps:

```prep -design picorv32a -tag 29-06_13-23 -overwrite```

```set lefs [glob $::env(DESIGN_DIR)/src/*.lef]```

```add_lefs -src $lefs```

```set ::env(SYNTH_SIZING) 1```

```set ::env(SYNTH_MAX_FANOUT) 4```

```echo $::env(SYNTH_DRIVING_CELL)```

```run_synthesis```


set the fanout to be 4.</br>
![image](https://github.com/user-attachments/assets/dacff8e8-e759-4e56-98a0-663bf5075ec9)

![image](https://github.com/user-attachments/assets/0951f5e1-ccb9-48c1-9908-fd8edc21ea1b)

Now, the slack has been updated in ```pre_sta.conf```, if we do ```sta pre_sta.conf``` we will see the new slack.<br>

![image](https://github.com/user-attachments/assets/0ad705a1-e9a7-4d0f-8247-9f2fc5c732af)
![image](https://github.com/user-attachments/assets/ee2975d3-79dc-40f2-b6aa-29810262b9e1)

### Lab steps to do basic timing ECO
We can see in below example, the Or gate which has driving strength of 2 is driving 4 fanout.</br>

![image](https://github.com/user-attachments/assets/f33492f0-e713-4bbd-b63a-31aead370adf)

For this we need to replace the current OR gate with Driving Strength of 4 by using the following commands.</br>
**1) Report the connections to the net**</br>
```report_net -connections_11672```

![image](https://github.com/user-attachments/assets/a5c74ec8-caa2-4bec-88b4-27a09cd416c7)

**2) Check the command Syntax**</br>
```help replace_cell```

**3) Replace the cell with higher driving strength**</br>
```replace_cell _14514_ sky130_fd_sc_hd__or3_4```

![image](https://github.com/user-attachments/assets/66655953-4096-4228-8be1-3fd213253d97)

**4) Now generate the custom timing report**</br>
```report_checks -fields {net cap slew input_pins} -digits 4```

![image](https://github.com/user-attachments/assets/29afcf3c-3bf4-411f-8006-dcf0469eea30)

We can see the slack is reduced from -23.90 to -23.523.</br>

![image](https://github.com/user-attachments/assets/5959f054-d8ae-4d31-bd31-0ab0de405f86)

### Clock Tree routing and buffering using H-tree Algorithm
Let's briefly explain what "Clock Tree Synthesis" is all about.</br>

Here we can see that clk1 is connected to FF1, FF2 and below also FF1 and FF2.</br>

![image](https://github.com/user-attachments/assets/da7083e3-4fea-4faf-9e9d-70e443f301e2)

the purpose is to make clk1 reach respective FFs. Let's blindly do the connections.</br>
![image](https://github.com/user-attachments/assets/cdc04f87-78bd-4e19-b95a-85d3cd1fe0b0)

We will what problems arise in this connection. Here we can see that the clock t1 and t2 is not equal due to obvious physical desugn reasons. So there will be 'skew' which won't be equal to 0. To make this skew=0, this couldn't be the required tree. This is a bad driven tree.</br>
![image](https://github.com/user-attachments/assets/b213a8b1-3d73-4791-8f34-53f9bce9ccff)

To solve this, we will do H-tree synthesis. An H-tree is a specific and commonly used clock distribution network topology used in Clock Tree Synthesis (CTS). It is especially useful for distributing the clock signal symmetrically and evenly across large chips. Here, the (clk) will enter from a midpoint, allowing it to reach all flip-flops at approximately the same time. Similarly, we will connect clk2 to the flip-flops using the midpoint approach. This will ensure that the timing required by the clocks to reacht he FFs will be almost same and the skew value will come close to 0.</br>

![image](https://github.com/user-attachments/assets/72d5e341-b507-4eaf-930f-113d52535110)

Next is **clock tree Buffering**, we know that the clock requires wires and these wires have their resiatances and capacitances. Like for example if the signal starts from clk1 and reaches to FFs, there might capacitance charging in between, which can lose the signal integrity till it reaches the final point. The solution of this problem is to add "repeaters".</br>

![image](https://github.com/user-attachments/assets/873a4962-07f7-4307-933f-6b7240ac255b)

Due to the wire length and the presence of RC networks, the waveform at the output does not match the input. To resolve this issue, we use repeaters. The key difference between repeaters used for the clock and those used for the data path is that clock repeaters maintain equal rise and fall times.</br>
The first step is to remove the existing clock route, place two repeaters, and allow the clock signal to pass through them. In this setup, the waveform generated at the input will properly propagate to the output. We can add as many repeaters as needed to maintain the continuous flow of the clock signal until the output.</br>

![image](https://github.com/user-attachments/assets/f9c30d0b-7c28-4f17-afbf-05113134fbbd)

### Crosstalk and clock net sheilding
**Clock Net shielding**: Till now we have built the clk tree in such a fashion that the skew between the launch flop and capture flop is 0. Skew means the latency difference between clk ports of the flop pins. Clk net shielding is the critical net scene in the design. But if phenonemon of crosstalks occurs in the line, everything built will be deteriorated.</br>
In clock net shielding we will take a particular clock and shield it from the outside world. This prevents coupling between wires which causes "glitch" and "delta t delay".</br>

![image](https://github.com/user-attachments/assets/74ba95ae-7412-4f6d-b126-c3cd1707ace1)

Let’s consider one clock net. Whenever there is a switching activity on the aggressor net, the coupling capacitance between adjacent wires can cause interference. If this capacitance is significant, any switching activity on the aggressor will directly affect the nearby clock net, leading to signal integrity problems like glitches or unexpected delays.</br>

![image](https://github.com/user-attachments/assets/fea90ca2-47bd-41f5-8444-4c6fb1889790)

Shielding is a technique used to protect the net from problems like glitch and delta delay. In shielding, an extra wire is placed between two signal wires where coupling capacitance is generated. This extra wire is either connected to ground (GND) or VDD to absorb interference.</br>
We see the amount of delta delay because of the bump when we are switching from logic '1' to logic '0'. And skew is not anymore 0 here. So the impact of crosstalk delta delay is that it make the skew value non-zero.</br>

![image](https://github.com/user-attachments/assets/6f3d0b72-5a08-4b97-9aff-231bf036199e)

### Lab steps to run CTS using TritonCTS
We now need to replace the old netlist with the newly generated netlist with reduced slack. Replace the previous synthesis file with the modified one.</br>
The image below shows the netlist before doing the modifications.</br>

![image](https://github.com/user-attachments/assets/a4892908-afdc-4b49-bb96-dee57863266d)

To do this, follow the steps below:
**1) Navigate to the directory**
```bash
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/29-06_13-23/results/synthesis/
```

**2) List the contents**
```bash
ls -ltr
```

**3) Copy the old netlist**
```bash
cp picorv32a.synthesis.v picorv32a.synthesis_old.v
```

**4) Verify the copied netlist**
```bash
ls -ltr
```

We will run Synthesis, floorplan, placement and cts in openlane directory. Follow the steps below:
```bash
prep -design picorv32a 

set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs

set ::env(SYNTH_STRATEGY) "DELAY 3"
set ::env(SYNTH_SIZING) 1

run_synthesis

init_floorplan
place_io
tap_decap_or

run_placement

# If any error occurs during CTS, use the following command
unset ::env(LIB_CTS)

run_cts
```

This flow begins by initializing the design using the given tag, followed by the addition of required LEF files. It proceeds with synthesis using a delay-driven approach and continues through the stages of floorplanning, IO placement, tap/decap insertion, placement, and clock tree synthesis (CTS). If an error occurs during CTS, running the ```unset ::env(LIB_CTS)``` command can help resolve the issue before attempting CTS again.

![Screenshot 2025-07-01 142315](https://github.com/user-attachments/assets/b8ceafd0-4d96-42b7-a6a7-18844d30bbfd)

![Screenshot 2025-07-01 142439](https://github.com/user-attachments/assets/f314afaf-d2c9-4801-a2ff-e4a9f558b7fe)

![Screenshot 2025-07-01 142540](https://github.com/user-attachments/assets/d2b7b552-bd4c-4e01-9fd6-5d79a108518c)

![Screenshot 2025-07-01 142627](https://github.com/user-attachments/assets/25381267-13f2-47b2-9bed-d62b6b36863a)

![Screenshot 2025-07-01 142646](https://github.com/user-attachments/assets/d40ccd7e-b68a-4097-ad56-e29993fe16d6)

![Screenshot 2025-07-01 142712](https://github.com/user-attachments/assets/97342d1b-b475-4cc8-a8ed-af432bc0a978)

After this we will get a new netlist created, with file name ```picorv32a.synthesis_cts.v``` in the location given below:</br>
```bash
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/01-07_08-51/results/synthesis/
```
**Purpose of CTS**

Before CTS:

The clock signal is typically just a single net connected directly to all clocked elements (flip-flops, registers).</br>

This leads to high clock skew and uneven delays across the design.</br>

After CTS:

The tool inserts buffers/inverters and builds a balanced tree to distribute the clock signal evenly.</br>

The result is a well-balanced clock tree that minimizes skew and latency.</br>

**Why a New Netlist is Generated**
The original synthesized netlist did not have actual clock buffers, inverters, or the tree structure.</br>

After run_cts, the tool modifies the netlist by:</br>

Adding clock buffers (e.g., BUFs or INVs).</br>

Creating new nets and connections for the clock distribution.</br>

Updating timing information (like arrival times, skew, etc.).</br>

### Lab steps to verify CTS runs
To create a database in OpenROAD using LEF and DEF files, follow these steps:</br>

First, navigate to the directory containing the required LEF and DEF files.</br>

Next, launch the OpenROAD tool by running:</br>
```nginx
openroad
```
Once inside the OpenROAD environment, execute the following commands to build the database:</br>
**To load the LEF file:**
```bash
read_lef /openLANE_flow/designs/picorv32a/runs/02-04_05-27/tmp/merged.lef
```

**To load the DEF file:**
```bash
read_def /openLANE_flow/designs/picorv32a/runs/02-04_05-27/results/cts/picorv32a.cts.def
```

To save the OpenROAD database as ```pico_cts.db```:</br>
```nginx
write_db pico_cts.db
```

After running these commands, you’ll find the ```pico_cts.db``` database file in your OpenLane directory.</br>

### Lab steps to analyze timing with real clocks using OpenSTA















































































































 














































































   



   

   





















































































































































 









 


 


 
 

 

 

 
 
























