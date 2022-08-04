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
  
  
  # Day 1 - Inception of open-source EDA, OpenLANE and Sky130 PDK
 ## Basic IC Design Terminologies
  During the Physical Designing, one will come across multiple terminologies that are frequently used. Some of them are mentioned below:
  - Package: It is a case that surrounds the circuit material to protect it from physical damage or corrosion and allow mounting of the electrical contacts connecting it to the printed circuit board (PCB). The below snippet shows an IC with 48 pins and Quad Flat No-Leads(QFN) package.
  - Die: A die is a small block of semiconducting material on which a given functional circuit is fabricated.
