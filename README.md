# Advanced Physical Design Workshop using Openlane sky130nm
This Repo consists of all the documentation done during Physical Design Workshop using Openlane Flow and Google's SkyWater 130nm PDK.

# Table of Contents
  - [RTL to GDSII Process Flow](#rtl-to-gdsii-process-flow)
  - [About Google SkyWater PDK](#about-google-skywater-pdk)
  - [List of All Open-Source Tools Used](#list-of-all-open-source-tools-used)
  - [Setting Up Environment](#setting-up-environment)
  - [Day 1 - Inception of open-source EDA, OpenLANE and Sky130 PDK](#day-1---inception-of-open-source-eda-openlane-and-sky130-pdk)
    - [Basic IC Design Terminologies](#basic-ic-design-terminologies)
    - [Introduction To RISC-V](#introduction-to-risc-v)
    - [SoC Design and OpenLANE](#soc-design-and-openlane)
      - [Open-Source PDK Directory Structure](#open-source-pdk-directory-structure)
      - [What is OpenLANE](#what-is-openlane)
    - [Open-Source EDA Tools](#open-source-eda-tools)
      - [OpenLANE Initialization](#openlane-initialization)
      - [Design Preparation](#design-preparation)
      - [Design Synthesis and Results](#design-synthesis-and-results)
      
# RTL to GDSII Process Flow
  RTL to GDSII Flow refers to the process of transforming logical Register Transfer Level(RTL) Design to a fabrication ready layout file in GDSII format. GDSII file is given to the foundary for chip fabrication.
  
  ![image](https://user-images.githubusercontent.com/88900482/182808199-5ff8f08a-9aa3-4aca-9cdf-805a1918c607.png)

  
  The RTL to GSDII flow consists of following steps:
  - RTL Synthesis
  ![image](https://user-images.githubusercontent.com/88900482/182809196-0245789b-c685-4d5a-a68e-9ab94722faad.png)

  ![image](https://user-images.githubusercontent.com/88900482/182815664-1ed778f1-8802-4c35-8726-946b007c0a3d.png)

  - Floor and Power planning

![image](https://user-images.githubusercontent.com/88900482/182815824-e72b3afe-ece7-4c34-a730-a1e7f759cbc0.png)

![image](https://user-images.githubusercontent.com/88900482/182815992-ef064dd0-b8d7-4d9c-94c9-66542526e114.png)

![image](https://user-images.githubusercontent.com/88900482/182816423-88a328bb-9c28-4ab2-9f52-9f0b41c006ac.png)

![image](https://user-images.githubusercontent.com/88900482/182817180-6356b946-966b-4f7e-acaf-70ec05825177.png)


  - Placement

  ![image](https://user-images.githubusercontent.com/88900482/182817369-031a70c1-6e22-464a-a068-06a3f6615de8.png)
  
  - Clock Tree Synthesis(CTS)

  ![image](https://user-images.githubusercontent.com/88900482/182817618-28028839-59be-4ab3-8b77-bcff3fbb5c8f.png)

  - Routing
  ![image](https://user-images.githubusercontent.com/88900482/182818198-f48883d4-4056-453c-bedb-75e6b12b6492.png)

  - GDSII format
 
  ![image](https://user-images.githubusercontent.com/88900482/182817806-e97eceb4-5ca2-48a6-8cb5-de6e01d1204e.png)

 
  # List of All Open-Source Tools Used
  | Name of Tool | Application / Usage |
  | --- | --- |
  | [Yosys](https://github.com/YosysHQ/yosys) | Synthesis of RTL Design |
  | ABC | Mapping of Netlist |
  | [OpenSTA](https://github.com/The-OpenROAD-Project/OpenSTA) | Static Timing Analysis |
  | [OpenROAD](https://github.com/The-OpenROAD-Project/OpenROAD) | Floorplanning, Placement, CTS, Optimization, Routing |
  | [TritonRoute](https://github.com/The-OpenROAD-Project/TritonRoute) | Detailed Routing |
  | [Magic VLSI](http://opencircuitdesign.com/magic/) | Layout Tool |
  | [NGSPICE](https://github.com/imr/ngspice) | SPICE Extraction and Simulation |
  | SPEF_EXTRACTOR | Generation of SPEF file from DEF file |
  
  
  # Day 1 - Inception of open-source EDA, OpenLANE and Sky130 PDK
 ## Basic IC Definitions
  The product of Physical design flow is IC which has two parts package and Chip (Die) as shown below.
  - Package: It is a case that surrounds chip.
  - Chip: It is a semiconductor die which consists of circuit functionality fabricated on the same

  ![image](https://user-images.githubusercontent.com/88900482/183603861-f5bc753f-133e-47f1-b345-b340a0694636.png)

 ## Invoking Openlane 
  To start openlane in Linux Ubantu we need to run the docker command in the openlane terminal and give command `./flow.tcl -interactive ` for interactive mode of operation. 
  
  <img width="738" alt="O1" src="https://user-images.githubusercontent.com/88900482/183676867-15e07727-99f9-479f-901a-5538df06d9c9.PNG">
Required package is selected by command ` package require openlane 0.9 `. 
Then preparing our design for Openlane flow with command ` prep -design <design-name> `

 <img width="492" alt="O2" src="https://user-images.githubusercontent.com/88900482/183679672-b710f028-59b0-49a9-857e-74985f5c136d.PNG">
 
<img width="556" alt="S1-prep" src="https://user-images.githubusercontent.com/88900482/183679808-4b6dd550-cf69-46fa-a45a-24464a9c1ee4.PNG">
 
  ### Running Synthesis of design and its results
  
  First step in Openlane flow is runnig RTL synthesis of Picorv32a for this input is Picorv32a.v file. It is done with command `run_synthesis`
  
  <img width="666" alt="synthesis-verilog" src="https://user-images.githubusercontent.com/88900482/183682218-1f311de1-87e3-41db-833b-c0954e482573.PNG">

<img width="553" alt="syn-comp" src="https://user-images.githubusercontent.com/88900482/183682372-51bddeec-57e8-4c1f-9ced-c2b3bbdfa559.PNG">

# Day 2 - Running Floorplan and Placement and Analysing results

## Floorplan
Step 2 in Openlane flow after synthesis is floorplan. It is run by command `run_floorplan`
 To see floorplan in magic we have to read merged.lef and picorv32a.floorplan.def that are resulted from floorplan run. 
 
 <img width="384" alt="magic2" src="https://user-images.githubusercontent.com/88900482/183688201-bf965c87-71eb-47e4-8b9f-3524216156e1.PNG">
 
 <img width="958" alt="magic1" src="https://user-images.githubusercontent.com/88900482/183688327-b8c6c8f7-f818-4e89-9623-f6a46e5d1590.PNG">

## Placement
Standard cells are placed in Placement run which is executed by command `run_placement `

<img width="664" alt="placement" src="https://user-images.githubusercontent.com/88900482/183688826-22d18d55-8061-4a9b-a6a4-b98da0f0ed29.PNG">

<img width="359" alt="place_magic" src="https://user-images.githubusercontent.com/88900482/183688918-4aaa4588-e743-4a1d-9ae9-e66e6fc9b62e.PNG">

<img width="939" alt="std_cell_place" src="https://user-images.githubusercontent.com/88900482/183689086-ec3a98be-2a69-4771-ac90-7f4a3d6f7bbf.PNG">



