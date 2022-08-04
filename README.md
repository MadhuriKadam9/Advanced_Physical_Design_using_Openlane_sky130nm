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

  
  - Static Timing Analysis(STA)
  - Design for Testability(DFT)
  - Floorplanning
  - Placement
  - Clock Tree Synthesis(CTS)
  - Routing
  - GDSII format
 
 
  # List of All Open-Source Tools Used
  | Name of Tool | Application / Usage |
  | --- | --- |
  | [Yosys](https://github.com/YosysHQ/yosys) | Synthesis of RTL Design |
  | ABC | Mapping of Netlist |
  
  
  # Day 1 - Inception of open-source EDA, OpenLANE and Sky130 PDK
 ## Basic IC Design Terminologies
  During the Physical Designing, one will come across multiple terminologies that are frequently used. Some of them are mentioned below:
  - Package: It is a case that surrounds the circuit material to protect it from physical damage or corrosion and allow mounting of the electrical contacts connecting it to the printed circuit board (PCB). The below snippet shows an IC with 48 pins and Quad Flat No-Leads(QFN) package.
  - Die: A die is a small block of semiconducting material on which a given functional circuit is fabricated.
