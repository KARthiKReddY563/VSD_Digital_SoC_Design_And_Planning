
# VSD - Digital VLSI SoC Design and Planning

2 Week digital VLSI SoC design and planning workshop with complete RTL2GDSII flow organised by VSD in collaboration with NASSCOM




##  Section 1 - Inception of open-source EDA, OpenLANE and Sky130 PDK 

Task 1
  1. To run 'picorv32a' design synthesis using OpenLANE flow and generate necessary outputs.
  2. Calculate the Flop ratio.
       
       
Flop Ratio = Number of D Flip Flops / Total Number of Cells


Percentage of DFF's = Flop Ratio * 100

Implementation 

Commands to invoke the OpenLANE flow and perform synthesis
 
 ```# Change directory to openlane flow directory
cd Desktop/work/tools/openlane_working_dir/openlane

# alias docker='docker run -it -v $(pwd):/openLANE_flow -v $PDK_ROOT:$PDK_ROOT -e PDK_ROOT=$PDK_ROOT -u $(id -u $USER):$(id -g $USER) efabless/openlane:v0.21'
# Since we have aliased the long command to 'docker' we can invoke the OpenLANE flow docker sub-system by just running this command
docker

# Now that we have entered the OpenLANE flow contained docker sub-system we can invoke the OpenLANE flow in the Interactive mode using the following command
./flow.tcl -interactive

# Now that OpenLANE flow is open we have to input the required packages for proper functionality of the OpenLANE flow
package require openlane 0.9

# Now the OpenLANE flow is ready to run any design and initially we have to prep the design creating some necessary files and directories for running a specific design which in our case is 'picorv32a'
prep -design picorv32a

# Now that the design is prepped and ready, we can run synthesis using following command
run_synthesis

# Exit from OpenLANE flow
exit

# Exit from OpenLANE flow docker sub-system
exit


 ```

Screenshots of the above commands ran

![Screenshot 2024-09-23 175829](https://github.com/user-attachments/assets/50f45b7a-925f-4494-9fde-8aa065e4b67d)



![IMG-20240923-WA0013](https://github.com/user-attachments/assets/947472b4-1c15-47b1-971c-6f065d3f2de6)

Screenshots of synthesis statistics report file with required values to calculate flop ratio.

![WhatsApp Image 2024-09-29 at 16 39 02_9aa1acda](https://github.com/user-attachments/assets/885fdad3-4fea-49d4-be81-951881e6ec29)



![WhatsApp Image 2024-09-29 at 16 39 02_7e77b4cf](https://github.com/user-attachments/assets/473bba60-39b2-4f04-bd74-4c40472623ec)

Calculation of Flop Ratio and DFF% from synthesis statistics report file

```math
Flop Ratio = 1613/14876 = 0.108429685

Percentage of DFF's = 0.108429685 * 100 = 10.84296854%

```

 # Section 2 - Good floorplan vs bad floorplan and introduction to library cells 

 Implementation
Section 2 tasks:-

1. Run 'picorv32a' design floorplan using OpenLANE flow and generate necessary outputs.
2. Calculate the die area in microns from the values in floorplan def.
3. Load generated floorplan def in magic tool and explore the floorplan.
4. Run 'picorv32a' design congestion aware placement using OpenLANE flow and generate necessary outputs.
5. Load generated placement def in magic tool and explore the placement.
  
  ```math
Area\ of die in microns = Die width in microns * Die height in microns
```
#### 1. Run 'picorv32a' design floorplan using OpenLANE flow and generate necessary outputs.

Commands to invoke the OpenLANE flow and perform floorplan

```bash
# Change directory to openlane flow directory
cd Desktop/work/tools/openlane_working_dir/openlane

# alias docker='docker run -it -v $(pwd):/openLANE_flow -v $PDK_ROOT:$PDK_ROOT -e PDK_ROOT=$PDK_ROOT -u $(id -u $USER):$(id -g $USER) efabless/openlane:v0.21'
# Since we have aliased the long command to 'docker' we can invoke the OpenLANE flow docker sub-system by just running this command
docker
```
```tcl
# Now that we have entered the OpenLANE flow contained docker sub-system we can invoke the OpenLANE flow in the Interactive mode using the following command
./flow.tcl -interactive

# Now that OpenLANE flow is open we have to input the required packages for proper functionality of the OpenLANE flow
package require openlane 0.9

# Now the OpenLANE flow is ready to run any design and initially we have to prep the design creating some necessary files and directories for running a specific design which in our case is 'picorv32a'
prep -design picorv32a

# Now that the design is prepped and ready, we can run synthesis using following command
run_synthesis

# Now we can run floorplan
run_floorplan
```

Screenshot of floorplan run

![Screenshot 2024-09-23 180522](https://github.com/user-attachments/assets/035e3cff-be3f-4bba-868b-2ebfab04c399)

#### 2. Calculate the die area in microns from the values in floorplan def.

Screenshot of contents of floorplan def

![WhatsApp Image 2024-09-29 at 16 39 05_9222792f](https://github.com/user-attachments/assets/aee6c325-29ac-4f44-bf2c-98ec70c60863)

According to floorplan def

```math
1000 Unit Distance = 1 Micron

Die width in unit distance = 660685 - 0 = 660685

Die height in unit distance = 671405 - 0 = 671405

Distance in microns = Value in Unit Distance/1000

Die width in microns = 660685/1000 = 660.685 Microns

Die height in microns = 671405/1000 = 671.405 Microns

Area of die in microns = 660.685 * 671.405 = 443587.212425 Square Microns
```


Commands to load floorplan def in magic in another terminal

```bash
# Change directory to path containing generated floorplan def
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/23-09_12-27/results/floorplan/

# Command to load the floorplan def in magic tool
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &
```

Screenshots of floorplan def in magic

![WhatsApp Image 2024-09-29 at 16 39 25_91c53eb6](https://github.com/user-attachments/assets/eb683466-fd15-4fb5-b9a9-477e7e3acd37)


Equidistant placement of ports

![WhatsApp Image 2024-10-02 at 19 48 20_8711e371](https://github.com/user-attachments/assets/29fa07e3-5ca1-41c0-8169-217e8d8fa141)

Port layer as set through config.tcl

![WhatsApp Image 2024-09-29 at 16 39 57_cafba4d5](https://github.com/user-attachments/assets/92b238cb-cb07-4eb9-abe8-c5c3df2c832e)

![WhatsApp Image 2024-10-02 at 19 56 50_d3eb2cd7](https://github.com/user-attachments/assets/c2d53889-bd53-4e0d-99f4-25e063e009f6)

Decap Cells and Tap Cells

![WhatsApp Image 2024-09-29 at 16 39 57_be8f7a4a](https://github.com/user-attachments/assets/ff2ef567-f7dd-4008-adb2-de67d72cff64)

Diogonally equidistant Tap cells
![WhatsApp Image 2024-09-29 at 16 39 57_f90bfbae](https://github.com/user-attachments/assets/fc3d8ffa-bd61-41a0-888e-0043c109d44f)

Unplaced standard cells at the origin

![WhatsApp Image 2024-09-29 at 16 39 57_394a3396](https://github.com/user-attachments/assets/f6b64d58-ca1d-4238-bfb7-6ef16d051c73)

Command to run placement

```tcl
# Congestion aware placement by default
run_placement
```

Screenshots of placement run
![image](https://github.com/user-attachments/assets/1e485942-40e4-4e35-ad1b-452fe6198e49)

![WhatsApp Image 2024-09-29 at 16 38 57_1f1a60d3](https://github.com/user-attachments/assets/78d1f928-5860-4b8a-9285-c1c99a23b9b5)

#### 5. Load generated placement def in magic tool and explore the placement.

Commands to load placement def in magic in another terminal

```bash
# Change directory to path containing generated placement def
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/17-03_12-06/results/placement/

# Command to load the placement def in magic tool
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &
```

Screenshots of floorplan def in magic

![WhatsApp Image 2024-09-29 at 16 39 57_8c88e123](https://github.com/user-attachments/assets/91cfb575-6623-4757-af53-54ea87b9d613)

Standard cells legally placed 

![WhatsApp Image 2024-09-29 at 16 39 57_1f5d5090](https://github.com/user-attachments/assets/8e2c9ed2-3a4b-4537-b8ba-b5b4fda253e0)

Commands to exit from current run

```tcl
# Exit from OpenLANE flow
exit

# Exit from OpenLANE flow docker sub-system
exit
```
## Section 3 - Design library cell using Magic Layout and ngspice characterization 


### Implementation

* Section 3 tasks:-
1. Clone custom inverter standard cell design from github repository: [Standard cell design and characterization using OpenLANE flow](https://github.com/nickson-jose/vsdstdcelldesign).
2. Load the custom inverter layout in magic and explore.
3. Spice extraction of inverter in magic.
4. Editing the spice model file for analysis through simulation.
5. Post-layout ngspice simulations.
6. Find problem in the DRC section of the old magic tech file for the skywater process and fix them.





#### 1. Clone custom inverter standard cell design from github repository

```bash
# Change directory to openlane
cd Desktop/work/tools/openlane_working_dir/openlane

# Clone the repository with custom inverter design
git clone https://github.com/nickson-jose/vsdstdcelldesign

# Change into repository directory
cd vsdstdcelldesign

# Copy magic tech file to the repo directory for easy access
cp /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech .

# Check contents whether everything is present
ls

# Command to open custom inverter layout in magic
magic -T sky130A.tech sky130_inv.mag &
```

Screenshot of commands run

![Screenshot from 2024-09-29 16-44-15](https://github.com/user-attachments/assets/976404d0-2213-4131-8de4-53a1db53b24e)

#### 2. Load the custom inverter layout in magic and explore.

Screenshot of custom inverter layout in magic

![Screenshot from 2024-09-29 16-44-23](https://github.com/user-attachments/assets/1802f534-018f-472c-b3ab-5a1cdfb4e92b)

NMOS and PMOS identified

![Screenshot from 2024-09-29 23-01-52](https://github.com/user-attachments/assets/691029de-64f7-402e-9bc0-5d75652f87da)

![Screenshot from 2024-09-29 21-56-31](https://github.com/user-attachments/assets/1690cf74-d4ec-4287-8294-e834ee2b2a9c)

Output Y connectivity to PMOS and NMOS drain verified

![Screenshot from 2024-09-29 23-16-09 (1)](https://github.com/user-attachments/assets/20eab7c0-275d-4516-95cb-412cb42c54a9)

PMOS source connectivity to VDD (here VPWR) verified

![Screenshot from 2024-09-29 23-16-41](https://github.com/user-attachments/assets/b6935c8e-19d8-42d3-818a-5973b4e7ce2e)

NMOS source connectivity to VSS (here VGND) verified

![Screenshot from 2024-09-29 23-16-30](https://github.com/user-attachments/assets/bf9474a7-f896-41b4-a652-3e16391aa9fd)

Deleting necessary layout part to see DRC error

![Screenshot from 2024-09-29 23-22-24](https://github.com/user-attachments/assets/e7156e5f-6145-4ff0-a188-3699a23af76f)

#### 3. Spice extraction of inverter in magic.

Commands for spice extraction of the custom inverter layout to be used in tkcon window of magic

```tcl
# Check current directory
pwd

# Extraction command to extract to .ext format
extract all

# Before converting ext to spice this command enable the parasitic extraction also
ext2spice cthresh 0 rthresh 0

# Converting to ext to spice
ext2spice
```

Screenshot of tkcon window after running above commands

![Screenshot from 2024-09-29 23-20-42](https://github.com/user-attachments/assets/04d021de-6abe-4f69-b01c-307e3d64a6fa)

Screenshot of created spice file

![Screenshot from 2024-09-29 23-24-09](https://github.com/user-attachments/assets/461c1af0-6e90-4142-b173-93dbc9090632)

#### 4. Editing the spice model file for analysis through simulation.

Measuring unit distance in layout grid

![Screenshot from 2024-09-29 23-27-51](https://github.com/user-attachments/assets/46679748-204b-4576-b02b-51f10450408c)

Final edited spice file ready for ngspice simulation

![Screenshot from 2024-09-29 23-34-40](https://github.com/user-attachments/assets/963b9eb4-8c09-4ec5-8b27-2952af39d2e1)

#### 5. Post-layout ngspice simulations.

Commands for ngspice simulation

```bash
# Command to directly load spice file for simulation to ngspice
ngspice sky130_inv.spice

# Now that we have entered ngspice with the simulation spice file loaded we just have to load the plot
plot y vs time a
```

Screenshots of ngspice run

![Screenshot from 2024-09-29 23-36-57](https://github.com/user-attachments/assets/c67caf19-2d2f-4e5a-a0ba-126457fc09cc)

![Screenshot from 2024-09-29 23-37-26](https://github.com/user-attachments/assets/9ce5d8c9-5891-45a8-baf5-678b78828f1b)

Screenshot of generated plot

![Screenshot from 2024-09-29 23-38-03](https://github.com/user-attachments/assets/9b58d19a-19f2-485a-b8bd-62e7d95721bc)

Rise transition time calculation

```math
Rise transition time = Time taken for output to rise to 80% - Time taken for output to rise to 20%
```
```math
20% of output = 660 mV
```
```math
80% of output = 2.64 V
```

20% Screenshots

![Screenshot from 2024-09-29 23-42-38](https://github.com/user-attachments/assets/76b8523e-a726-4edf-8fbd-34d373298cd1)

![Screenshot from 2024-09-29 23-42-44](https://github.com/user-attachments/assets/6629e331-30ab-4807-9d4c-b2a37c039721)


80% Screenshots

![Screenshot from 2024-09-29 23-43-57](https://github.com/user-attachments/assets/fc5b66e7-2c38-4be6-8d81-12b8b24ff8e9)

![Screenshot from 2024-09-29 23-43-49](https://github.com/user-attachments/assets/a5841cb8-1244-4805-9c9d-979dde29ee05)

```math
Rise transition time = 2.24638 - 2.18242 = 0.06396 ns = 63.96 ps
```

Fall transition time calculation

```math
Fall transition time = Time taken for output to fall to 20% - Time taken for output to fall to 80%
```
```math
20% of output = 660 mV
```
```math
80% of output = 2.64 V
```

20% Screenshots

![Screenshot from 2024-10-02 21-58-28 1](https://github.com/user-attachments/assets/49ca4b44-e322-4d3f-a0bd-7be164d92156)

![Screenshot from 2024-10-02 21-58-32 1](https://github.com/user-attachments/assets/2de43fd6-7beb-4361-94f3-94ec3a61c302)

80% Screenshots

![Screenshot from 2024-10-02 21-59-25 1](https://github.com/user-attachments/assets/835a9e0d-6252-4379-bbc7-b4107e71a125)

![Screenshot from 2024-10-02 21-59-29 1](https://github.com/user-attachments/assets/8090ac3e-a0e7-4801-ab4b-2109a959d9fa)

```math
Fall transition time = 4.0955 - 4.0536 = 0.0419 ns = 41.9 ps
```

Rise Cell Delay Calculation

```math
Rise Cell Delay = Time taken for output to rise to 50% - Time taken for input to fall to 50%
```
```math
50% of 3.3 V = 1.65 V
```

50% Screenshots

![WhatsApp Image 2024-10-02 at 22 13 49_2de01a5d](https://github.com/user-attachments/assets/3c3f1b7d-ef7d-4fec-9509-49eae308a346)

![WhatsApp Image 2024-10-02 at 22 13 49_7acceb53](https://github.com/user-attachments/assets/ea5c8c72-80fe-42ac-9833-69af1284119b)

```math
Rise Cell Delay = 2.21144 - 2.15008 = 0.06136 ns = 61.36 ps
```

Fall Cell Delay Calculation

```math
Fall Cell Delay = Time taken for output to fall to 50% - Time taken for input to rise to 50%
```
```math
50% of 3.3 V = 1.65 V
```

50% Screenshots

![WhatsApp Image 2024-10-02 at 22 13 49_61f8fb41](https://github.com/user-attachments/assets/20b9f4f4-914d-4c5a-8b19-df8c4c8e8183)

![WhatsApp Image 2024-10-02 at 22 13 49_7131e0b9](https://github.com/user-attachments/assets/cab04013-228a-4fb4-9f8f-b952c089fc67)

```math
Fall Cell Delay = 4.07 - 4.05 = 0.02 ns = 20 ps
```

#### 6. Find problem in the DRC section of the old magic tech file for the skywater process and fix them.

Link to Sky130 Periphery rules: [https://skywater-pdk.readthedocs.io/en/main/rules/periphery.html](https://skywater-pdk.readthedocs.io/en/main/rules/periphery.html)

Commands to download and view the corrupted skywater process magic tech file and associated files to perform drc corrections

```bash
# Change to home directory
cd

# Command to download the lab files
wget http://opencircuitdesign.com/open_pdks/archive/drc_tests.tgz

# Since lab file is compressed command to extract it
tar xfz drc_tests.tgz

# Change directory into the lab folder
cd drc_tests

# List all files and directories present in the current directory
ls -al

# Command to view .magicrc file
gvim .magicrc

# Command to open magic tool in better graphics
magic -d XR &
```

Screenshots of commands run

![Screenshot from 2024-09-29 23-49-57](https://github.com/user-attachments/assets/7b13623f-a585-427e-bcad-d92e84921b8d)

![Screenshot from 2024-09-29 23-50-06](https://github.com/user-attachments/assets/79e6698d-9edb-4d5b-9858-a65319d2f523)

Screenshot of .magicrc file

![Screenshot from 2024-09-29 23-50-28](https://github.com/user-attachments/assets/660fecab-a6d0-47ea-9135-795274c7785e)

**Incorrectly implemented poly.9 simple rule correction**

Screenshot of poly rules

![image](https://github.com/user-attachments/assets/e53003b9-0196-4743-a69d-b22ab4cb815a)

Incorrectly implemented poly.9 rule no drc violation even though spacing < 0.48u

![WhatsApp Image 2024-10-02 at 22 26 37_97dd7e0c](https://github.com/user-attachments/assets/c83a30c6-3188-4b2a-a7ec-146761ac6f42)


New commands inserted in sky130A.tech file to update drc

![Screenshot from 2024-09-30 00-15-50](https://github.com/user-attachments/assets/1eec6cb7-7eb4-47ef-ac09-c7bee9c869a0)

![Screenshot from 2024-09-30 00-23-00](https://github.com/user-attachments/assets/7f19e46c-6051-41ed-84f2-3570346e12c2)



Commands to run in tkcon window

```tcl
# Loading updated tech file
tech load sky130A.tech

# Must re-run drc check to see updated drc errors
drc check

# Selecting region displaying the new errors and getting the error messages 
drc why
```

Screenshot of magic window with rule implemented

![Screenshot from 2024-09-30 00-25-10](https://github.com/user-attachments/assets/15d66e88-58b2-4016-9d65-39bf846981d1)

![Screenshot from 2024-09-30 00-46-55](https://github.com/user-attachments/assets/f1bb5420-cf49-4e5a-a650-dcc64e65528d)

**Incorrectly implemented difftap.2 simple rule correction**

Screenshot of difftap rules

![image](https://github.com/user-attachments/assets/0e9c00e5-c310-402e-89a9-3dd2018a7e14)


Incorrectly implemented difftap.2 rule no drc violation even though spacing < 0.42u

![WhatsApp Image 2024-10-02 at 22 36 20_7731d86c](https://github.com/user-attachments/assets/6e08229c-a654-49be-90f5-5ca3f1e5046f)

New commands inserted in sky130A.tech file to update drc

![WhatsApp Image 2024-10-02 at 22 36 18_a723a90d](https://github.com/user-attachments/assets/3a975ab9-e72e-4d6c-b8d1-0cd3d9a7e819)


Commands to run in tkcon window

```tcl
# Loading updated tech file
tech load sky130A.tech

# Must re-run drc check to see updated drc errors
drc check

# Selecting region displaying the new errors and getting the error messages 
drc why
```

Screenshot of magic window with rule implemented

![WhatsApp Image 2024-10-02 at 22 36 19_94d342a2](https://github.com/user-attachments/assets/5ab324b3-a3d3-4804-bfb0-77c63879c513)

**Incorrectly implemented nwell.4 complex rule correction**

Screenshot of nwell rules

![image](https://github.com/user-attachments/assets/b2b5fdf1-4736-4a84-9866-62863bf1ff9c)

Incorrectly implemented nwell.4 rule no drc violation even though no tap present in nwell

![Screenshot from 2024-09-30 01-12-20](https://github.com/user-attachments/assets/5fd41b1e-0f83-422a-a2a7-90e4e61d1ce1)

New commands inserted in sky130A.tech file to update drc

![Screenshot from 2024-09-30 01-04-06](https://github.com/user-attachments/assets/7b1b025b-456a-4749-beb9-d9704d486bdb)

![Screenshot from 2024-09-30 01-04-28](https://github.com/user-attachments/assets/4a2cca17-2d79-4329-99a7-b8d60602e968)

Commands to run in tkcon window

```tcl
# Loading updated tech file
tech load sky130A.tech

# Change drc style to drc full
drc style drc(full)

# Must re-run drc check to see updated drc errors
drc check

# Selecting region displaying the new errors and getting the error messages 
drc why
```

Screenshot of magic window with rule implemented

![Screenshot from 2024-09-30 01-12-20](https://github.com/user-attachments/assets/3897d886-f14d-4870-9a56-f15195fa8460)



   
 








      
           
   
 


