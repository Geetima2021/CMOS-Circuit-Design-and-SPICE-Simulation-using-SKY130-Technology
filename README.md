# CMOS Circuit Design and SPICE Simulation using SKY130 Technology



![1](https://user-images.githubusercontent.com/63381455/152677837-bd7bb6ee-e8db-450d-a13c-688f4ee58bfb.png)

# Programs included

# Table of contents

- [Overview](#overview)
- 
- [Program included](#prog)

# Overview

This repository deals with the CMOS circuit design and spice simulation using SKY130 tecvhology as a part of the internship program by [VLSI system design](https://www.vlsisystemdesign.com/cmos-circuit-design-spice-simulation-using-sky130-technology/). CMOS is known for its robustness and is the most widely in designing  of circuit. As a beginner level course the emphasis is understanding the PMOS and NMOS transistor finally followed by the CMOS transistor. The first section of the course the theortical understanding of the NMOS transistor followed by its practical implementation to understand the different mode of operation of the NMOS transistor.


# Program included

1. Spice deck for w=5 L=2 and generate ID-VDS graph

```bash
Spice deck to generate the ID-Vds curve of a NMOS transistor using sky130 technology 
*Model Description
.param temp=27

*Including sky130 library files
.lib "/home/geetima/MySpace/Process_corner_variation/TT_spice_130nm/sky130_fd_pr/models/sky130.lib.spice" tt

*Netlist Description

XM1 Vdd n1 0 0 sky130_fd_pr__nfet_01v8 w=5 l=2
R1 n1 in 55
Vdd vdd 0 1.8V
Vin in 0 1.8V

*simulation commands

.op
.dc Vdd 0 1.8 0.1 Vin 0 1.8 0.2

*interactive interpretor command
.control
run
display
setplot dc1
plot -Vdd#branch
.endc
.end
```
The output of the above program is as shown below
![01_IDVds_wF](https://user-images.githubusercontent.com/63381455/152681380-3079cb09-40b0-4487-ab1f-528f79453cb5.png)
![01_IDVds](https://user-images.githubusercontent.com/63381455/152681383-7b122a81-7e0c-400d-b8e1-1455c7651c6d.png)


2. Spice deck for w=5u L=2u and generate Id-Vgs curve

```bash
*Model Description
.param temp=27

*Including sky130 library files
.lib "/home/geetima/MySpace/Process_corner_variation/TT_spice_130nm/sky130_fd_pr/models/sky130.lib.spice" tt

*Netlist Description

XM1 Vdd n1 0 0 sky130_fd_pr__nfet_01v8 w=5 l=2
R1 n1 in 55
Vdd vdd 0 1.8V
Vin in 0 1.8V

*simulation commands
`

.op
.dc Vin 0 1.8 0.1 Vdd 0 1.8 1.8

.control

run
display
setplot dc1
plot -Vdd#branch
.endc
.end

```
![01_IDVgs_quadratic](https://user-images.githubusercontent.com/63381455/152681566-33afe775-c71b-473d-8c03-dd39fdc1a568.png)

3. Spice deck for w=5u L=2u and generate ID-VDS graph

```bash
Spice deck for w=0.39u L=0.15u and generate ID-VDS graph

*Model description
.param temp = 27 

*Netlist description
XM1 Vdd n1 0 0 sky130_fd_pr__nfet_01v8 W =0.39 L = 0.15 
R1 in n1 55      
Vin in 0 1.8V    
Vdd Vdd 0 1.8V   

*include library for model sky130_fd_pr__nfet_01v8 description
.lib "/home/geetima/MySpace/Process_corner_variation/TT_spice_130nm/sky130_fd_pr/models/sky130.lib.spice" tt


*Simulation commands
.op  
.dc Vdd 0 1.8 0.1 Vin 0 1.8 0.2 

*control commands

.control
run    
display  
setplot dc1
plot -Vdd#branch  
.endc
.end
```
![02_IDVds_0 39_w015l_wf](https://user-images.githubusercontent.com/63381455/152681706-ea1e36ed-1b4d-41a9-a563-1d45f53d1b82.png)
![02_IDVds_0 39_w015l](https://user-images.githubusercontent.com/63381455/152681708-229fac8f-6abd-438f-a232-422ff47b520b.png)

4. Spice deck for w=0.39u and l=0.15u to generate Id-Vgs curve

```bash
Spice deck for w=0.39u and l=0.15u to generate Id-Vgs curve

*Model description
.param temp = 27

*Netlist description
XM1 Vdd n1 0 0 sky130_fd_pr__nfet_01v8 W =0.39 L = 0.15 
R1 in n1 55      
Vin in 0 1.8V    
Vdd Vdd 0 1.8V   

*include library for model sky130_fd_pr__nfet_01v8 description
.lib "/home/geetima/MySpace/Process_corner_variation/TT_spice_130nm/sky130_fd_pr/models/sky130.lib.spice" tt


*Simulation commands
.op  
.dc Vin 0 1.8 0.1 

*control commands

.control
run    
display  
setplot dc1
plot -Vdd#branch  
.endc
.end
```
![02_IDVgs_0 39_w015l_wf](https://user-images.githubusercontent.com/63381455/152692664-6eea9ddb-6265-4262-bda6-02efdef8cfab.png)
![02_IDVgs_0 39_w015l](https://user-images.githubusercontent.com/63381455/152692672-4faf43d4-7550-4ac2-86b8-8cf5e402ad1b.png)



5. Spice deck to plot the dynamic characteristics(rise and fall delay) of cmos inverter circuit for wp/lp = 2.34wn/ln

```bash
Spice deck to plot the dynamic characteristics(rise and fall delay) of cmos inverter circuit for wp/lp = 2.34wn/ln

*Model description
.param temp = 27

*Netlist description

XM1 out in Vdd Vdd sky130_fd_pr__pfet_01v8 W =0.84 L = 0.15
XM1 out in 0 0 sky130_fd_pr__nfet_01v8 W =0.36 L = 0.15 
cload out 0 50fF

Vdd Vdd 0 1.8V
Vin in 0 pulse 0 1.8V 0 10ps 10ps 2ns 4ns

*include library
.lib "/home/geetima/MySpace/Process_corner_variation/TT_spice_130nm/sky130_fd_pr/models/sky130.lib.spice" tt

*simulation commands
.op
.tran 10ps 10ns

*interactive interpretor command

.control
run
display
setplot tran1
plot in out
.endc
.end

```
![03_riseFall_2 34wl_wf](https://user-images.githubusercontent.com/63381455/152681840-7b0078e8-33b7-46a7-9cc6-faae6b709947.png)
![03_riseFall_2 34wl](https://user-images.githubusercontent.com/63381455/152681843-4b5273fc-fd11-455f-ba7b-f8b071b0b47a.png)

6. Spice deck to plot the vtc characteristics of a cmos inverter circuit for wp/lp = 2.34wn/ln
```bash
Spice deck to plot the vtc characteristics of a cmos inverter circuit for wp/lp = 2.34wn/ln

*Model description
.param temp = 27

*Netlist description

XM1 out in Vdd Vdd sky130_fd_pr__pfet_01v8 W =0.84 L = 0.15
XM1 out in 0 0 sky130_fd_pr__nfet_01v8 W =0.36 L = 0.15 
cload out 0 50fF

Vdd Vdd 0 1.8V
Vin in 0 1.8V

*include library
.lib "/home/geetima/MySpace/Process_corner_variation/TT_spice_130nm/sky130_fd_pr/models/sky130.lib.spice" tt

*simulation commands
.op
.dc Vin 0 1.8V 0.01

*interactive interpretor command

.control
run
display
setplot dc1
plot in out
.endc
.end
```
![03_vtc_2 34wl_wf](https://user-images.githubusercontent.com/63381455/152681920-f5cf0fa0-2bf9-4094-bb34-8c1cd3248615.png)
![03_vtc_2 34wl](https://user-images.githubusercontent.com/63381455/152681922-93c4189c-1c95-4f93-a00e-1a0e85f8f9ad.png)

```bash
Spice deck to plot the dynamic characteristics(rise and fall delay) of cmos inverter circuit for wp/lp = wn/ln

*Model description
.param temp = 27

*Netlist description

XM1 out in Vdd Vdd sky130_fd_pr__pfet_01v8 W =0.84 L = 0.15
XM1 out in 0 0 sky130_fd_pr__nfet_01v8 W =0.84 L = 0.15 
cload out 0 50fF

Vdd Vdd 0 1.8V
Vin in 0 pulse 0 1.8V 0 10ps 10ps 2ns 4ns

*include library
.lib "/home/geetima/MySpace/Process_corner_variation/TT_spice_130nm/sky130_fd_pr/models/sky130.lib.spice" tt

*simulation commands
.op
.tran 10ps 10ns

*interactive interpretor command

.control
run
display
setplot tran1
plot in out
.endc
.end

```
![04_riseFall_1wl_wf](https://user-images.githubusercontent.com/63381455/152682013-253c279e-64e1-40de-a43d-938e3881fd39.png)
![04_riseFall_1wl](https://user-images.githubusercontent.com/63381455/152682015-f13c3249-ffd0-42be-8d2e-239e91ae474c.png)

<!---The delay table for different PMOS size with respect to constant NMOS size for sky 130nm technology node is as shown below --->

<!---![delay](https://user-images.githubusercontent.com/63381455/152683309-3b51ac22-5c82-48e0-87be-585a9c0e5e54.png)--->

The delay table for different PMOS size with respect to constant NMOS size using tsmc 250um technology file is as shown below

| **(Wp/Lp)um** | **(Wn/Ln)um** | **Switching threshold (Vm) V** | **Rise delay(ps)**     | **Fall delay (ps)**    |
|---------------|---------------|--------------------------------|------------------------|------------------------|
| Wp/Lp         | 1.Wn/Ln       | ~0.99                          | 1.1641 - 1.0159 = 148  | 2.07769 - 2.00578= 72  |
| Wp/Lp         | 2.Wn/Ln       | 1.1519 = ~1.2                  | 1.09627 -01.01591 = 80 | 2.08198 - 2.00582 = 76 |
| Wp/Lp         | 3.Wn/Ln       | 1.25129 =~1.25                 | 1.07287 - 1.0159 = 57  | 2.08619 - 2.00583 = 80 |
| Wp/Lp         | 4.Wn/Ln       | 1.32054 =~1.32                 | 1.06101 -1.01589 = 45  | 2.0933 - 2.00593 = 84  |
| Wn/Ln         | 5.Wn/Ln       | 0.918462 =~1.4                 | 1.05387 - 1.01548 = 37 | 2.0944 - 2.00582 = 88  |
  

