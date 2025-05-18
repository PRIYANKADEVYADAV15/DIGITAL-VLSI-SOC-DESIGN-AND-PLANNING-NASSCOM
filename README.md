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
1 microns = 1000 database units, so given DIAREA (0 0) (66065 67145), converted into microns becomes,</br>
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














































































































 









 


 


 
 

 

 

 
 
























