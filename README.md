# Advanced Physical Design Workshop using Openlane sky130nm
This Repo consists of all the documentation done during Physical Design Workshop using Openlane Flow and Google's SkyWater 130nm PDK.

# Table of Contents
  - [RTL to GDSII Process Flow](#rtl-to-gdsii-process-flow)
  - [List of All Open-Source Tools Used](#list-of-all-open-source-tools-used)
  - [Day 1 - Inception of open-source EDA, OpenLANE and Sky130 PDK](#day-1---inception-of-open-source-eda-openlane-and-sky130-pdk)
    - [Basic IC Definitions](#basic-ic-definitions)
    - [Invoking Openlane](#invoking-openlane)
      -[Running Synthesis of design and its results](#running-synthesis-of-design-and-its-results)
  - [Day 2 - Running Floorplan and Placement and Analysing results](#day-2---running-floorplan-and-placement-and-analysing-results)
    - [Floorplan](#floorplan)
    - [Placement](#placement)  
  - [Day 3 - Design of Standard cell using Magic Layout and ngspice characterization](#day-3---design-of-standard-cell-using-magic-layout-and-ngspice-characterization)
    - [Inverter Layout to Netlist creation](#inverter-layout-to-netlist-creation)
    - [Spice Simulation of Inverter netlist](#spice-simulation-of-inverter-netlist)
  - [Day 4 - Pre-layout timing analysis and importance of good clock tree](#day-4---pre-layout-timing-analysis-and-importance-of-good-clock-tree)       - [Extracting lef file from Inverter mag file](#extracting-lef-file-from-inverter-mag-file)
      -[Grid change](#grid-change)
      -[Writing lef](#writing-lef)
      -[Copying all required files on Picorv32a src folder](#copying-all-required-files-on-picorv32a-src-folder)
      -[Setting Environmental Variable](#setting-environmental-variable)
    - [Running Synthesis](#running-synthesis) 
    - [Running Floorplan](#running-floorplan)
    - [Analysing Floorplan in Magic](#placement-analysing-floorplan-in-magic)
    - [To configure OpenSTA for post-synth timing analysis](#to-configure-opensta-for-post-synth-timing-analysis)
    - [Clock Tree Synthesis using TritonCTS](#clock-tree-synthesis-using-tritoncts)
 - [Day 5 - Final steps for RTL2GDS](#day-5---final-steps-for-rtl2gds)     
    - [Power Distribution Network Generation](#power-distribution-network-generation)
    - [Final Layout Picorv32a](#final-layout-picorv32a) 
    - [SPEF File Generation](#spef-file-generation)
    - [Placement](#placement)
- [References](#references)
- [Acknowledgement](#acknowledgement)    
    
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
  To start openlane in Linux Ubantu we need to run the docker command in the openlane terminal.
  Then give command `./flow.tcl -interactive ` for interactive mode of operation. 
  
  <img width="738" alt="O1" src="https://user-images.githubusercontent.com/88900482/183676867-15e07727-99f9-479f-901a-5538df06d9c9.PNG">
  
Required package is selected by command ` package require openlane 0.9 `
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


# Day 3 - Design of Standard cell using Magic Layout and ngspice characterization

## Inverter Layout to Netlist creation
Inverter layout is imported from vsdsdcell git hub repo and it is then opened in magic with sk130A.tech file

<img width="485" alt="inv_lay" src="https://user-images.githubusercontent.com/88900482/183697216-4aef554a-0738-408a-a7bf-f3716750a931.PNG">

Then RC parasitics are extracted by using following commands in tkcon window. It gives .ext and .spice file as output of this event.
` extract all`
` ext2spice cthresh 0 rthresh 0`
` ext2spice `

Modified Spice netlist of Inverter is shown below.

![image](https://user-images.githubusercontent.com/88900482/183698497-d262ee5e-3fea-4cb7-b465-802e9df94817.png)

## Spice Simulation of Inverter netlist
Modified Spice netlist of Inverter is then run using ngspice with following command. 
`ngspice <name-of-SPICE-netlist-file>`

<img width="555" alt="op" src="https://user-images.githubusercontent.com/88900482/183699433-c84af498-3619-44f7-96a2-fe84030a2dca.PNG">

<img width="855" alt="opplot" src="https://user-images.githubusercontent.com/88900482/183699483-847e11cf-c4ff-4a0f-992e-ec7f754aca88.PNG">

Transient Response of Inverter
<img width="896" alt="wf" src="https://user-images.githubusercontent.com/88900482/183699555-b41756bb-7ec7-49de-a6d2-74897cea1490.PNG">

# Day 4 - Pre-layout timing analysis and importance of good clock tree

## Extracting lef file from Inverter mag file

![image](https://user-images.githubusercontent.com/88900482/183939979-6f106e98-9cd1-4d6b-b8cf-1842b75a64b7.png)

Grid dimensions of layout i.e `sky130_vsdinv.mag` are changed as per given in above `track.info` file, as routing of li layer happens only on these grid lines.

### Grid change
Grid dimension is changed by typing below command in tkcon window 
` grid 0.46um 0.34um 0.23um 0.17um `

<img width="706" alt="vsdinvmag" src="https://user-images.githubusercontent.com/88900482/183702204-bd6c9c6b-7502-4e33-bef1-3e2d5f193e40.PNG">

<img width="906" alt="gridchange" src="https://user-images.githubusercontent.com/88900482/183702254-2f012116-284a-4b93-b55b-74f4abfb7457.PNG">

Also we can verify that Input and output ports are there on intersection of horizontal and vertical grid lines as shown below
Width of the std cell should be odd multiple of X pitch
Width of cell = Check inner boundary distance = 2+0.5+0.5=3 i.e. odd multiple of grid box

![image](https://user-images.githubusercontent.com/88900482/183943420-faf8d4cc-32cc-4e05-9d85-b320313ce987.png)


### Writing lef
To write lef file execute command `lef write ` in tkcon window as below. This will create sky130_vsdinv.lef in vsdstdcelldesign folder.

![image](https://user-images.githubusercontent.com/88900482/183702384-a01ce0a7-f318-46d4-87d3-de7698f5de9b.png)

![image](https://user-images.githubusercontent.com/88900482/183702445-d71112a5-b292-4ecc-9140-e886fb1cf29a.png)

### Copying all required files on Picorv32a src folder
In order to plug in our Sky130_vsdinv.lef file with Picorv32a. we need to copy all sky130_fd_sc_* files and Sky130_vsdinv.lef file in Picorv32a src folder as shown below.

![image](https://user-images.githubusercontent.com/88900482/183951735-697ab199-d6c6-458b-a289-929dcaf9d83d.png)

### Setting Environmental Variable 
Edited the config.tcl file to set environmental variables as shown below
`set ::env(LIB_FASTEST) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__fast.lib"`
`set ::env(LIB_SLOWEST) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__slow.lib"`

![image](https://user-images.githubusercontent.com/88900482/184130108-2e930bdf-9de4-4611-8a94-f9bdd5e33b41.png)
Then invoking theh docker and preparing the previous runs folder to overwrite as shown below 

`madhurib.saksham@sky130-pd-workshop-08:~/Desktop/work/tools/openlane_working_dir/openlane$ docker`
`bash-4.2$ ./flow.tcl –interactive`
`% prep -design picorv32a -tag 06-08_06-23 -overwrite`

![image](https://user-images.githubusercontent.com/88900482/184132940-bb0dd560-4060-4670-927f-4d4abdb8c8b1.png)

Add below two lines on the fly

`set_lefs [glob $::env(DESIGN_DIR)/src/*.lef]`
`add_lefs -src $lefs`

![image](https://user-images.githubusercontent.com/88900482/184133529-a8351461-1c17-44b8-8855-6ec8824feddf.png)

## Running Synthesis
One the design preparation is done we can see that it has merged sky130_vsdinv.lef file
Then execute command `run_synthesis`

![image](https://user-images.githubusercontent.com/88900482/184134116-37899fa2-67d1-4b4d-bf4d-748a663afe72.png)

Here we can observe 1554 cells of sky130_vsdinv are included. Chip area for module picorv32a is 147712.918400

![image](https://user-images.githubusercontent.com/88900482/184134545-bba4aa3d-13fd-4f0a-ac37-e0f9f673dec4.png)

Slack Violation is 23.89ns as it is –ve slack
Tns = total negative slack     wns= worst negative slack
We need to work on this in order to convert the slack in 0 or positive value. Then only we can met the slack.

Setting various synthesis variables on the fly as shown below
`set ::env(SYNTH_STRATEGY) 1`
`set ::env(SYNTH_SIZING) 1`
`set ::env(SYNTH_BUFFERING) 1`
 
 ![image](https://user-images.githubusercontent.com/88900482/184136648-ea3c8e1b-c7e6-4648-bd0a-19f68c7af5f9.png)
 
 Then running synthesis again
 
 ![image](https://user-images.githubusercontent.com/88900482/184136763-539d848c-6e8d-4d07-9ccb-995b2c81a07d.png)

## Running Floorplan
Once Synthesis is done we will run floorplan by executing below commands one by one as per the requirement in new openlane flow

- `init_floorplan`
![image](https://user-images.githubusercontent.com/88900482/184138418-f3efb400-05dc-4611-9c73-204c92ded450.png)


- `place_io`
![image](https://user-images.githubusercontent.com/88900482/184138485-76e2aaf0-1a7e-446f-8cb5-2d438b598c19.png)


- `global_placement_or`
![image](https://user-images.githubusercontent.com/88900482/184138573-517f73af-bb0f-496d-9cb9-1f2c8053ad0b.png)


- `tap_decap_or`
![image](https://user-images.githubusercontent.com/88900482/184138616-92253b65-5531-4eb2-833a-dcd6270e0a5d.png)


- `global_placement_or`
![image](https://user-images.githubusercontent.com/88900482/184138660-b34909fc-7aff-47e6-9a0e-ce31b2422b68.png)


- `gen_pdn`
![image](https://user-images.githubusercontent.com/88900482/184138697-50e2763c-5859-4519-9247-67a6ccb8afee.png)


- `run_placement`
![image](https://user-images.githubusercontent.com/88900482/184138737-a009e77d-5258-45c2-b4b4-da11a51d2ef3.png)


To check whether our vsdinv.lef is included in merged.lef file, open merged .lef as shown below

![image](https://user-images.githubusercontent.com/88900482/184138273-db864f54-6056-4e1b-84d7-ce72d90074e6.png)

## Analysing Floorplan in Magic
Once we have done with the placement we will read corresponding merged.lef and picorv32a.placement.def files in magic by executing command given below.

`06-08_06-23/results/placement$ magic -T /home/madhurib.saksham/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &`

This will open Picorv32a floorplan with placement in magic as shown below

![image](https://user-images.githubusercontent.com/88900482/184140103-4cbd8175-1bbd-4cfd-ac58-ed1dd78e7cb6.png)

![image](https://user-images.githubusercontent.com/88900482/184140239-a451a7a6-50fa-4987-94e7-248fdf2756df.png)

![image](https://user-images.githubusercontent.com/88900482/184140400-fdf7b45a-9755-46e3-8361-593cd1d50e86.png)

![image](https://user-images.githubusercontent.com/88900482/184140443-f19b42f1-4f64-442f-94f5-9bd493b3c380.png)

In above diagram layout we can see it has included our sky130_vsdinv.lef cell also metal 1 is shared between two vertical cells this is calle abuttment. Power and GND rails are shared.

## To configure OpenSTA for post-synth timing analysis
OpenSTA is tool used for Static timing Analysis. It is inside openroad of Openlane flow. So to use openSTA we need to invoke openroad in openlane flow by executing command `openroad`

sta command works on pre_sta.conf file. So updating this file from vsdstdcell repo as given below and copying that file in openlane directory.

![image](https://user-images.githubusercontent.com/88900482/184143956-efa61177-281a-4e9a-b534-77c83398a6bd.png)

This pre_sta.conf file also reads various sky130_fd_sc_hd_* files and my_base.sdc file as shown below.

![image](https://user-images.githubusercontent.com/88900482/184144824-9106a338-3fa4-4b65-9aee-d0e22906cd26.png)

![image](https://user-images.githubusercontent.com/88900482/184144858-ee446a67-1062-4d1e-b56c-6cd04e1ae63d.png)

Now Static timing Analysis is performed by executing following command `sta pre_sta.conf`

![image](https://user-images.githubusercontent.com/88900482/184145263-5b512466-6249-4b81-a2ae-20633cb884d1.png)

![image](https://user-images.githubusercontent.com/88900482/184145298-c292b18b-b8ba-4da9-aee8-7d08aad63c3c.png)

## Clock Tree Synthesis using TritonCTS

Clock Tree Synthesis is the process in which real clock beffers are added in design to ditribute clock signal to all the sequential elements without resulting in latency or clock skew.

To ensure this various clock distribution techniques are there as follows
  1. H - Tree
  2. X - Tree
  3. Fish bone
 
 CTS is executed using TritonCTS tool in Openlane. It is always done after floorplan and placement as it works on placement.def file which is resulted after placement run.
 
 It is done by executing command `run_cts`
 
 <img width="538" alt="cts_success" src="https://user-images.githubusercontent.com/88900482/184153315-395d4563-c3ac-4b2e-925c-2412ddf9a589.PNG">

# Day 5 - Final steps for RTL2GDS
 ## Power Distribution Network Generation
   In a normal RTL to GDSII flow the PDN generation is done before the placement step, but in the OpenLANE flow PDN is executed after the Clock Tree Synthesis(CTS). This step generates all the tracks and power rails on layout required for the design. 
   PDN Generation is executed by runnig following commands one by one.
   
    `init_floorplan`
    `place_io`
    `global_placement_or`
    `detailed_placement`
    `tap_decap_or`
    `detailed_placement`
    `gen_pdn`
    

![image](https://user-images.githubusercontent.com/88900482/184164673-2aea6047-fac5-495e-982d-c593308374d8.png)

    
   ## Routing using TritonRoute
   
   OpenLANE uses TritonRoute, to carry out routing of tracks.
   The routing process is executed in two parts:
   1. Global Routing - Routing guides in the form of rectangls are generated for interconnects.
   2. Detailed Routing - This accepts lef, def and Preprocessed routing guides. It gives detailed routing with optimized wire length and via count.
   
   TritonRoute 14 ensures there are no DRC violations after routing. But it takes a lot of time and memory for execution.
   
   Routing is done by running following command.
   
    run_routing
   
   ![image](https://user-images.githubusercontent.com/88900482/184161167-ba3912c1-4467-44e7-ada9-1e8bfcce3896.png)
   
   ## Final Layout Picorv32a
   
   <img width="343" alt="final" src="https://user-images.githubusercontent.com/88900482/184163457-9d23198f-f3f1-4a8b-acd0-c7bc92300435.PNG">

   <img width="442" alt="final" src="https://user-images.githubusercontent.com/88900482/184163149-217dce7e-1ae0-46ca-9412-d4d384526506.PNG">

   ## SPEF File Generation
   
   In the new openlane flow SPEF extraction is carried out inside wrapper, so no need to do it explicitly.
   We can see the picorv32a.spef file in /reults/routing folder as shown below.
   
   ![image](https://user-images.githubusercontent.com/88900482/184162202-6c2a482b-9a1b-4747-a42e-f38a44ef6221.png)
   
# References
  - RISC-V: https://riscv.org/
  - VLSI System Design: https://www.vlsisystemdesign.com/
  - OpenLANE: https://github.com/The-OpenROAD-Project/OpenLane
  - VSDSTDCELL: https://github.com/nickson-jose/vsdstdcelldesign.git

# Acknowledgement
  - [Kunal Ghosh](https://github.com/kunalg123), Co-founder, VSD Corp. Pvt. Ltd.
  - [Nickson Jose](https://github.com/nickson-jose)
  


   
