# NASSCOM &VSD -PHYSICAL DESIGN USING OPENLANE

## Table of Contents
- [Day - 1 Inception of Open-Source EDA, OpenLane and Sky130 PDK](#day---1-inception-of-open-source-eda-openlane-and-sky130-pdk)
- [Day - 2 Good Floor plan Vs Bad floor plan](#day---2-good-floorplan-vs-bad-floorplan-and-introduction-to-libraryt-cells)
- [Day - 3 Design Library Cells using magic](#day--3-design-library-cell-using-magic-layout-and-ngspice-charcterization)
- [Day - 4 Timing Analysis and Clock Tree Synthesis](#day-4-timing-analysis-and-clock-tree-synthesis-cts)
- [Day - 5 Final Steps in RTL2GDS](#day-5--final-steps-in-rtl2gds)
- [Acknowledgement](#acknowledgement)
- [References](#references)

## Day - 1 Inception of Open-Source EDA, OpenLane and Sky130 PDK
### Introduction to RISC-V
RISC-V is an open-source instruction set architecture (ISA) that is adopted in the world of computer architecture and processor design. It was originally developed at the University of California, Berkeley in 2010 and has since grown into a global collaboration of researchers and industry experts. The key characteristic of RISC-V is its simplicity and modularity. It follows the Reduced Instruction Set Computer (RISC) design philosophy, which emphasizes a small and streamlined set of instructions that are easy to decode and execute. RISC-V offers a base set of instructions, called the "RV32I," which provides essential operations for general-purpose computing. Additional optional instruction sets, such as RV32F for single-precision floating-point operations or RV64G for 64-bit computing, can be added to meet specific application requirements. One of the major advantages of RISC-V is its open nature. The ISA specifications, reference implementations, and software tools are freely available, allowing anyone to study, modify, or implement their own RISC-V processors without licensing fees or restrictions. This openness has fostered a vibrant ecosystem of hardware designers, software developers, and researchers who collaborate and innovate around the RISC-V architecture.

### How the software applications run on the hardware ?
All the software applications we use in our daily lives rely on hardware to run. The system software is responsible for translating the application program into binary language, which the hardware can understand and execute. The primary components of system software include the Operating System (OS), Compiler, and Assembler. The OS plays a crucial role in managing various aspects of the computer system. It provides an environment for the application program to run and handles tasks such as memory management, process scheduling, and input/output operations. Depending on the underlying architecture, such as MIPS, x86, x64, or RISC-V, the OS translates the application program into assembly language instructions. The compiler is responsible for converting high-level programming languages, like C or Java, into assembly-level language instructions. This translation process is influenced by the specific architecture on which the software will be executed. Different architectures have their own instruction sets, and the compiler ensures that the instructions generated are compatible with the targeted architecture. Once the code is in assembly language, the assembler comes into play. It takes the assembly code and translates it into binary code, which is a sequence of 0s and 1s that can be directly dumped into the hardware.



**RTL IPs**, or Register Transfer Level Intellectual Property, refer to pre-designed and pre-verified digital hardware components or blocks that are described at the Register Transfer Level (RTL). RTL is a hardware description language (HDL) representation of a digital circuit or a portion of a circuit. In the context of integrated circuit (IC) design, an IP refers to a reusable building block that can be integrated into a larger design. RTL IPs, specifically, are designed at the register transfer level, which represents the flow of data between registers and the operations performed on that data.These IPs can be licensed from IP vendors or developed in-house. They provide a level of abstraction that allows designers to focus on higher-level design aspects rather than implementing low-level details from scratch. RTL IPs offer various advantages, such as improved productivity, faster time-to-market, and increased design reliability. By using RTL IPs, designers can leverage optimized and well-tested building blocks, reducing the likelihood of errors and bugs. Additionally, using RTL IPs promotes design reuse, enabling designers to create complex systems by assembling and integrating different IP blocks.

**EDA (Electronic Design Automation) tools** are software applications used in the design, development, and analysis of electronic systems, including integrated circuits (ICs), printed circuit boards (PCBs), and other electronic components. These tools automate various tasks involved in the design process, increasing efficiency and reducing time-to-market.

**Process Design Kit (PDK)** is a set of files used within the semiconductor industry to model a fabrication process for the design tools used to design an integrated circuit.  PDK’s are often specific to a foundry, and may be subject to a non-disclosure agreement.  While most PDK’s are proprietary to a foundry, certain PDKs are opensource and entirely within the public domain.Traditionally, PDKs have been proprietary and provided by semiconductor foundries, limiting access and customization options for IC designers. However, open-source PDKs aim to promote collaboration, innovation, and accessibility by making the design kit freely available to the community. By providing an open-source PDK, designers can modify and customize the kit to suit their specific requirements. This flexibility allows for greater innovation, collaboration, and knowledge sharing within the design community. It also lowers the barriers to entry for new designers and encourages participation in the development of new ICs and electronic systems. Some of the  open-source PDKs are : SKY130, GFU180, ASAP7 etc

### Simplified RTL to GDSII flow 
The RTL to GDSII flow basically involves :
1. **RTL Design** -  The process begins with the RTL design phase, where the digital circuit is described using a hardware description language (HDL) like VHDL or Verilog. The RTL description captures the functional behavior of the circuit, specifying its logic and data paths.

2. **RTL Synthesis** - RTL synthesis converts the high-level RTL description into a gate-level netlist. This stage involves mapping the RTL code to a library of standard cells (pre-designed logic elements) and optimizing the resulting gate-level representation for area, power, and timing. The output of RTL synthesis is typically in a format called the gate-level netlist.

3. **Floor and Power Planning** - is a crucial step in the digital design flow that involves partitioning the chip's area and determining the placement of major components and functional blocks. It establishes an initial high-level layout and defines the overall chip dimensions, locations of critical modules, power grid distribution, and I/O placement.The primary goals of floor planning are: Area Partitioning, Power Distribution, Signal Flow and Interconnect Planning, Placement of Key Components, Design Constraints and Optimization.

4. **Placement** - Placement involves assigning the physical coordinates to each gate-level cell on the chip's layout. The placement process aims to minimize wirelength, optimize signal delay, and satisfy design rules and constraints. Modern placement algorithms use techniques like global placement and detailed placement to achieve an optimal placement solution.

5. **Clock Tree Synthesis** - Clock tree synthesis (CTS) is a crucial step in the digital design flow that involves constructing an optimized clock distribution network within an integrated circuit (IC). The primary goal of CTS is to ensure balanced and efficient clock signal distribution to all sequential elements (flip-flops, registers) within the design, minimizing clock skew and achieving timing closure.

6. **Routing** - Routing connects the gates and interconnects on the chip based on the placement information. It involves determining the optimal paths for the wires and vias that carry signals between different components. The routing process needs to adhere to design rules, avoid congestion, and optimize for factors like signal integrity, power, and manufacturability.

7. **Sign-off** - Sign-off analysis refers to the final stage of the electronic design process, where comprehensive verification and analysis are performed to ensure that the design meets all the necessary requirements and specifications. It involves a series of checks and simulations to confirm that the design is ready for fabrication and meets the desired functionality, performance, power, and reliability targets. 

8. **GDSII File Generation** - Once the layout is verified and passes all checks, the final step is to generate the GDSII file format, which represents the complete physical layout of the chip. The GDSII file contains the geometric information necessary for fabrication, including the shapes, layers, masks, and other relevant details.

### Introduction to OpenLANE
OpenLane is an automated RTL to GDSII flow based on several components including OpenROAD, Yosys, Magic, Netgen, CVC, SPEF-Extractor, KLayout and a number of custom scripts for design exploration and optimization. It also provides a number of custom scripts for design exploration and optimization. The flow performs all ASIC implementation steps from RTL all the way down to GDSII. Currently, it supports both A and B variants of the sky130 PDK, the C variant of the gf180mcu PDK, and instructions to add support for other (including proprietary) PDKs are documented. The whole documentation can be found here [https://github.com/The-OpenROAD-Project/OpenLane].
OpenLane abstracts the underlying open source utilities, and allows users to configure all their behavior with just a single configuration file.


# OpenLane Architecture

![A diagram showing the general stages of the OpenLane flow as a series of blocks](https://github.com/AnupriyaKrishnamoorthy/NASSCOM-PD-ANU/assets/30011675/00d41df4-30f4-45b6-8e35-d934cdb493fc)


## OpenLane Design Stages

OpenLane flow consists of several stages. By default all flow steps are run in sequence. Each stage may consist of multiple sub-stages. OpenLane can also be run interactively as shown [here][25].

1. **Synthesis**
    1. `yosys/abc` - Perform RTL synthesis and technology mapping.
    2. `OpenSTA` - Performs static timing analysis on the resulting netlist to generate timing reports
2. **Floorplaning**
    1. `init_fp` - Defines the core area for the macro as well as the rows (used for placement) and the tracks (used for routing)
    2. `ioplacer` - Places the macro input and output ports
    3. `pdngen` - Generates the power distribution network
    4. `tapcell` - Inserts welltap and decap cells in the floorplan
3. **Placement**
    1. `RePLace` - Performs global placement
    2. `Resizer` - Performs optional optimizations on the design
    3. `OpenDP` - Performs detailed placement to legalize the globally placed components
4. **CTS**
    1. `TritonCTS` - Synthesizes the clock distribution network (the clock tree)
5. **Routing**
    1. `FastRoute` - Performs global routing to generate a guide file for the detailed router
    2. `TritonRoute` - Performs detailed routing
    3. `OpenRCX` - Performs SPEF extraction
6. **Tapeout**
    1. `Magic` - Streams out the final GDSII layout file from the routed def
    2. `KLayout` - Streams out the final GDSII layout file from the routed def as a back-up
7. **Signoff**
    1. `Magic` - Performs DRC Checks & Antenna Checks
    2. `KLayout` - Performs DRC Checks
    3. `Netgen` - Performs LVS Checks
    4. `CVC` - Performs Circuit Validity Checks

OpenLane integrated several key open source tools over the execution stages:
- RTL Synthesis, Technology Mapping, and Formal Verification : [yosys + abc][4]
- Static Timing Analysis: [OpenSTA][8]
- Floor Planning: [init_fp][5], [ioPlacer][6], [pdn][16] and [tapcell][7]
- Placement: [RePLace][9] (Global), [Resizer][15] and [OpenPhySyn][28] (formerly), and [OpenDP][10] (Detailed)
- Clock Tree Synthesis: [TritonCTS][11]
- Fill Insertion: [OpenDP/filler_placement][10]
- Routing: [FastRoute][12] or [CU-GR][36] (formerly) and [TritonRoute][13] (Detailed) or [DR-CU][36]
- SPEF Extraction: [OpenRCX][37] or [SPEF-Extractor][27] (formerly)
- GDSII Streaming out: [Magic][14] and [KLayout][35]
- DRC Checks: [Magic][14] and [KLayout][35]
- LVS check: [Netgen][22]
- Antenna Checks: [Magic][14]
- Circuit Validity Checker: [CVC][31]

> Everything in Floorplanning through Routing is done using [OpenROAD](https://github.com/The-OpenROAD-Project/OpenROAD) and its various sub-utilities.

## OpenLane Output

All output run data is placed by default under ./designs/design_name/runs. Each flow cycle will output a timestamp-marked folder containing the following file structure:

```
<design_name>
├── config.json/config.tcl
├── runs
│   ├── <tag>
│   │   ├── config.tcl
│   │   ├── {logs, reports, tmp}
│   │   │   ├── cts
│   │   │   ├── signoff
│   │   │   ├── floorplan
│   │   │   ├── placement
│   │   │   ├── routing
│   │   │   └── synthesis
│   │   ├── results
│   │   │   ├── final
│   │   │   ├── cts
│   │   │   ├── signoff
│   │   │   ├── floorplan
│   │   │   ├── placement
│   │   │   ├── routing
│   │   │   └── synthesis
```

To delete all generated runs under all designs:
`make clean_runs`

## Installation for MacOS (Apple Chip M1,M2,M3)
So far all we did was to learn about the flow and it's working. The workshop has contents tailored to Intel based chips and also Ubuntu, since most of the softwares haven't been updated to work with Apple's Chip, we have to do some run-around. First, the workshop works based on virtualbox and virtual machine, since VirtualBox hasn't released a stable version yet, we would have to use UTM for MacOS. And the disk-image wouldn't be compatible with UTM. So you need to have the unaltered picorv32a files, which can be found in the repository [here][40]

### Installation of Ubuntu using UTM
For the installation and use of Ubuntu as a Virtual Machine in MacOS, I followed this [video][https://www.youtube.com/watch?v=MVLbb1aMk24]. Once installed while rebooting if you do not get this [!image](<img width="434" alt="UTM_Reboot" src="https://github.com/AnupriyaKrishnamoorthy/NASSCOM-PD-ANU/assets/30011675/da11646a-9d8d-451c-9a8e-25b35b175b0e">
) do not panic just shut down the VM and follow the next steps.
Now you have ubuntu in a VM, the next step is to download and install openlane and docker.

### OpenLane Installation
Prior to the installation of the OpenLane install the dependencies and packages using the command shown below :</br>
``` 
sudo apt-get update
sudo apt-get upgrade
sudo apt install -y build-essential python3 python3-venv python3-pip make git

Docker Installation for Openlane
```
Docker Installation :</br>
```
sudo apt install apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io
sudo docker run hello-world

sudo groupadd docker
sudo usermod -aG docker $USER
sudo reboot 


# Check for successful installation
sudo docker run hello-world 
```
Once you run the above docker run, if you have installed it properly then the screen will appear like [this](<img width="734" alt="Successful Docker Installation" src="https://github.com/AnupriyaKrishnamoorthy/NASSCOM-PD-ANU/assets/30011675/7d16470d-9d77-4e0e-86ca-8133f5671442">).


**Steps to install OpenLane, PDKs and Tools**</br>
```
cd $HOME
git clone https://github.com/The-OpenROAD-Project/OpenLane --recurse-submodules 
cd OpenLane
make
make test
cd /home/anupriyavsd/OpenLane/designs/ci
cp -r * ../
```
After a successful ```make``` when you run ```make test``` you will have a ```Basic test passed``` on your terminal. At this step when openlane is running, include the [picorv32a][40]
In order to start open lane and run synthesis we would follow the next steps.

### Steps to run synthesis in OpenLane Interactive mode:


```
cd ~/OpenLane
make mount
./flow.tcl -interactive
package require openlane 0.9
prep -design picorv32a
run_synthesis
```

[1]: ../for_developers/docker.md
[4]: https://github.com/YosysHQ/yosys
[5]: https://github.com/The-OpenROAD-Project/OpenROAD/tree/master/src/ifp
[6]: https://github.com/The-OpenROAD-Project/OpenROAD/tree/master/src/ppl
[7]: https://github.com/The-OpenROAD-Project/OpenROAD/tree/master/src/tap
[8]: https://github.com/The-OpenROAD-Project/OpenSTA
[9]: https://github.com/The-OpenROAD-Project/OpenROAD/tree/master/src/replace
[10]: https://github.com/The-OpenROAD-Project/OpenROAD/tree/master/src/dpl
[11]: https://github.com/The-OpenROAD-Project/OpenROAD/tree/master/src/cts
[12]: https://github.com/The-OpenROAD-Project/OpenROAD/tree/master/src/grt
[13]: https://github.com/The-OpenROAD-Project/OpenROAD/tree/master/src/TritonRoute
[14]: https://github.com/RTimothyEdwards/magic
[15]: https://github.com/The-OpenROAD-Project/OpenROAD/tree/master/src/rsz
[16]: https://github.com/The-OpenROAD-Project/OpenROAD/tree/master/src/pdn
[18]: https://github.com/RTimothyEdwards/qflow/blob/master/src/addspacers.c
[19]: https://github.com/The-OpenROAD-Project/
[20]: https://github.com/git-lfs/git-lfs/wiki/Installation
[21]: /usage/exploration_script.md
[22]: https://github.com/RTimothyEdwards/netgen
[24]: ./for_developers/pdk_structure.md
[25]: https://github.com/The-OpenROAD-Project/OpenLane/blob/master/docs/source/reference/interactive_mode.md
[26]: ./usage/chip_integration.md
[27]: https://github.com/AUCOHL/spef-extractor
[28]: https://github.com/scale-lab/OpenPhySyn
[29]: ./usage/hardening_macros.md
[30]: ./usage/building_the_pdk.md
[31]: https://github.com/d-m-bailey/cvc
[32]: ./for_developers/code_contribution.md
[33]: ./authors.md
[34]: ./reference/openlane_commands.md
[35]: https://github.com/KLayout/klayout
[36]: https://github.com/cuhk-eda/cu-gr
[37]: https://github.com/The-OpenROAD-Project/OpenROAD/tree/master/src/rcx
[38]: ./for_developers/issue_regression_tests.md
[39]: https://github.com/cuhk-eda/dr-cu
[40]: https://github.com/AnupriyaKrishnamoorthy/NASSCOM-PD-ANU/tree/65f03d525c953fc0513856928e923acfec3de57b/picorv32a
