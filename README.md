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

















