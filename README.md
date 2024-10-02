
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
           
   
 








      
           
   
 


