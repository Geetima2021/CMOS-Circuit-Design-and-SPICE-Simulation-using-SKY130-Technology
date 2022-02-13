# CMOS Circuit Design and SPICE Simulation using SKY130 Technology

![1](https://user-images.githubusercontent.com/63381455/152677837-bd7bb6ee-e8db-450d-a13c-688f4ee58bfb.png)


# Table of contents

- [Overview](#overview)
- [Why do we need circuit design and spice simulation?](#circuit)
- [NMOS- Basic element in circuit design](#NMOS)
- [Regions of operation of NMOS](#operation)
  - [Cut off region](#cut)
  - [Resistive or Linear region of operation](#linear)
  - [Saturation region of operation](#sat)
- [Introduction to spice](#spice)
  - [Circuit description in spice syntax](#syntax)
  - [Spice deck to generate the ID-Vds curve of a NMOS transistor using sky130 technology](#Deck1)
  - [Spice deck for w=0.39u L=0.15u and generate ID-VDS graph](#deck2)
  
- [Program included](#prog)

# Overview

[VLSI system design](https://www.vlsisystemdesign.com/cmos-circuit-design-spice-simulation-using-sky130-technology) workshop on circuit design and spice simulation is base on the open source tool ngspice and Google/Skywater 130nm PDK. The use of the open source tools and pdk is advantegeous for the learner with  respect to usage of these tools after the workshop. [VLSI system design](https://www.vlsisystemdesign.com/cmos-circuit-design-spice-simulation-using-sky130-technology) workshop focusses on both theoritical as well as lab based learning. Starting form the concepts of basic element in cicuit design NMOS, its characteristics curve IDVgs, IDVds, its regions of operation - drain current equation, threshold voltage equation, effect of long channel and short channel device (velocity saturation)on the behavioral characteristic of NMOS. Spice scripting and simulation of the learned concept gives a more practical analysis of its characteritics. CMOS inverter circuits its design consideration is learned iand simulated including its voltage transfer characteristics, static behaviour evaluation defining its robustness based on the 4 parameters viz switching threshold voltage (Vm), noise magin, power supply variation and device variation. Also the dynamic characteristics – propagation delay of the CMOS is learned and simulated. The importance of the transistor sizes while designing a CMOS inverter for a particular application is seen. All the above design consideration assist in finding the delay table and noise margin for differnent transistor sizes which can be compared with the actual values provided by the partipdk cular technology file. This woekshop gives a detail understanding on the basic building block of the VLSI design – CMOS.


# [Why do we need circuit design and spice simulation?](#circuit)

Circuit design is basically the designing of the logic gates like and, or, nand, buffer or any other circuit using particular connection fashion and size of the pmos and nmos transistor for obtaining the required functionality of a particular circuit. Spice simulation on the other hand is requited to fed in input waveform to the the circuit inorder to analyses the output. It also gives a very important table known as delay table which is base of crrosstalk, clock tree syntesis, sta, physical design etc. Without simulation the VLSI indusry has to face more challenges. An NMOS circuit and its spice deck is as shown below. The detail description of the spice deck shall be done later.

![2_1](https://user-images.githubusercontent.com/63381455/153574079-4f5b74e7-c21c-4fee-bca8-06c15718d3dd.png)

# [NMOS- Basic element in circuit design](#NMOS)

The pictorial representation of an NMOS device with its various terminals is as shown in figure below. Is consist of the followng

1. 4 terminal device - drain, gate, source, body
2. Ptype substrate
3. Consist of Isolation region (Si0<sub>2</sub>)
4. n<sup>+</sup> drain and source terminal
5. Gate oxide
6. PolySi or metal gate – drives the nmos


![Nmos](https://user-images.githubusercontent.com/63381455/153582815-e7c5a70f-7b06-4ab4-8dac-e6cc8159161c.png)

# [Regions of operation of NMOS](#operation)

There are three regions of operation in NMOS transistor. In each of the region The drain current (ID) changes with respect to the gate to source voltage (Vgs) and drain to source voltage(Vds). 
When Vsb = 0
- Initiallly Gate voltage, source voltage and drain voltage is at zero Volt. 
- The source substrate and drain substrate reqion forms a pn juction diode and both the jucnction are off as no voltage is applied to the drain and source terminal, 
- A high resistance area is formed between the source and the drain. Now 
- When the Vgs voltage is increased a positive charge is formed at the gate terminal which repels the positive charge of the p type substrate from the substrate which leaves negative charge at the surface. 
- Vgs is  further increase more and more positive charge of the ptype substrate repels resulting in depletion region. 
- the semiconductor surface inverts to n type material and the phenomenon is known as strong inversion.

***The gate to source voltage at which the strong inversion occurs is known as “Threshold voltage"***. 

When Vsb = +ve voltage

- The formation of the channel (strong inversion at the surface) is delayed as compared to when Vsb is 0.
- n<sup>+</sup> and the p substrate forms depletion region across the source substrate and drain substarte junction 
- Width of the depletion region more towards the source substrate junction
- More gate to source voltage Vgs required for strong inversion to occur
 
Threshold voltage:
```bash
Vgs = Vto, when Vsb = 0 
		Vto is a function of manufacturing process
		
		Vgs = Vto + V1, when Vsb = +ve 
		V1 = γ(sqroot(|– 2Φf +Vsb)  - sqroot(|-12Φf |))
		
		where,  Φf  = Fermi potential = -Φt *ln(Na/ni)
		γ = body effect coefficient which expresses the changes in Vsb whose uint is
		sqroot(V) or V^0.5
			γ  = div(sqroot(2qNa εSi,Cox)) 
		where , q = charge of electron 1.602*10^(-19) coloumbs
			Na = acceptor doping concentration
			 εSi = relative permitivity of Si  = 11.7
			 Cox = oxide capacitance
 ```
All the above parameter are manufacturing process parameters, given by the foundry.
	
 V<sup>0.5</sup>

## [Cut off region](#cut)

In this region the gate to source voltage Vgs is less than the threshold voltage and thus no channel appears between the source and the drain region indicating a high resistance area between the source and the drain. Hence, the drain current is zero.

For cut off, 0 < Vgs < Vth 
ID = 0

<!---![3](https://user-images.githubusercontent.com/63381455/153581528-0e461759-bd12-4f00-8c3e-7201c17dae21.png)--->




The programs below are performed for sky130 technology node and TT corner

1a. Spice deck for w=5u L=2u and generate ID-VDS graph
```bash
Spice deck to generate the ID-Vds curve of a NMOS transistor using sky130 technology 
*Model Description
.param temp=27

*Include model file
.lib "../sky130_fd_pr/models/sky130.lib.spice" tt

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
1b. Spice deck for w=0.39u L=0.15u and generate ID-VDS graph

```bash
Spice deck for w=0.39u L=0.15u and generate ID-VDS graph

*Model description
.param temp = 27 

*Netlist description
XM1 Vdd n1 0 0 sky130_fd_pr__nfet_01v8 W =0.39 L = 0.15 
R1 in n1 55      
Vin in 0 1.8V    
Vdd Vdd 0 1.8V   

*include model file
.lib "../sky130_fd_pr/models/sky130.lib.spice" tt

*Simulation commands
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


The above two programs is written to generate the IDVDS curve for different transistor sizes .The snapshot of the output is as shown in the figure below. The individual output for the program can be found [here](https://github.com/Geetima2021/CMOS-Circuit-Design-and-SPICE-Simulation-using-SKY130-Technology/tree/main/image/output_TT).

![ID_Vds_Lc_sc](https://user-images.githubusercontent.com/63381455/152710606-e9c80981-27e0-47be-9a5f-e9790679b422.JPG)

2a. Spice deck for w=5u L=2u to generate Id-Vgs curve

```bash
*Model Description
.param temp=27

*Include model file
.lib "../sky130_fd_pr/models/sky130.lib.spice" tt

*Netlist Description
XM1 Vdd n1 0 0 sky130_fd_pr__nfet_01v8 w=5 l=2
R1 n1 in 55
Vdd vdd 0 1.8V
Vin in 0 1.8V

*simulation commands
.op
.dc Vin 0 1.8 0.1 Vdd 0 1.8 1.8


*interactive interpretor command
.control
run
display
setplot dc1
plot -Vdd#branch
.endc
.end

```

2b. Spice deck for w=0.39u and l=0.15u to generate Id-Vgs curve

```bash
Spice deck for w=0.39u and l=0.15u to generate Id-Vgs curve

*Model description
.param temp = 27

*Netlist description
XM1 Vdd n1 0 0 sky130_fd_pr__nfet_01v8 W =0.39 L = 0.15 
R1 in n1 55      
Vin in 0 1.8V    
Vdd Vdd 0 1.8V   

*include model file
.lib "../sky130_fd_pr/models/sky130.lib.spice" tt

*Simulation commands
.op  
.dc Vin 0 1.8 0.1 

*interactive interpretor command
.control
run    
display  
setplot dc1
plot -Vdd#branch  
.endc
.end
```
In the above two program the IDVgs curve for different transistor sizes and the snapshot of the same is shown below. The output of the above program can be viwed individually [here](https://github.com/Geetima2021/CMOS-Circuit-Design-and-SPICE-Simulation-using-SKY130-Technology/tree/main/image/output_TT).

![IDVgs_LC_sc](https://user-images.githubusercontent.com/63381455/152711755-c0767cf6-9040-4d17-b721-aae885d9f8fc.JPG)

3. Spice deck to plot the dynamic characteristics(rise and fall delay) of cmos inverter circuit for wp/lp = 2.34wn/ln

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

*include model file
.lib "../sky130_fd_pr/models/sky130.lib.spice" tt

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


4. These set of programs analyis the static and dynamic characteristics for CMOS inverter of differnt sizes. Here, the CMOS robustness for the variation in switching threshold, noise margin, power supply variation and device variation is plotted and verified as per theoritical understanding. Also, the change in the dynamic characteristic (rise delay, fall delay) is observe and the importance of selecting a proper set of transistor size as per requirement is seen.

Spice deck to plot the vtc characteristics of a cmos inverter circuit for wp/lp = 2.34wn/ln
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

*include model file
.lib "../sky130_fd_pr/models/sky130.lib.spice" tt

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

5. Spice deck to plot the dynamic characteristics(rise and fall delay) of cmos inverter circuit for wp/lp = wn/ln

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

*include model file
.lib "../sky130_fd_pr/models/sky130.lib.spice" tt

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

The delay table for different PMOS size with respect to constant NMOS size using sky130technology file for tt corner is as shown below. The analysis starts from w = 0.42 and l = 0.14um. The programs required for evaluation of the rise delay and fall delay for different (w/l) ratio of NMOS and PMOS transistor is [here](https://github.com/Geetima2021/CMOS-Circuit-Design-and-SPICE-Simulation-using-SKY130-Technology/tree/main/Program) and so is the static characterisctics switching threshold evaluation. 

Table1: Delay table using sky130 tt corner

| **wp/lp** | **x.wn/ln** | **Rise delay (ps)** | **Fall delay(ps)** | **Switching threshold (V)** |
|-----------|-------------|---------------------|--------------------|----------------------------
| wp/lp     | 1(wn/ln)    |         149        |         73       |   0.831765   |  
| wp/lp     | 2(wn/ln)     |         88        |         75        |   0.889839   |  
| wp/lp     | 2.5(wn/ln)   |         73        |         75        |  0.895161    |
| wp/lp     | 3(wn/ln )    |         69         |         76       |   0.895161   |       
| wp/lp     | 4(wn/ln)     |         59         |         78       |    0.916129  |     
| wp/lp     | 5(wn/ln)     |         52         |         80        |   0.929032  |      

Table2: Delay table using sky130 ff corner

| **wp/lp** | **x.wn/ln** | **Rise delay (ps)** | **Fall delay(ps)** | **switching threshold (V)** |
|-----------|-------------|---------------------|--------------------|-----------------------------|
| wp/lp     | 1(wn/ln)    |         114         |         60         | 0.809677                    |
| wp/lp     | 2(wn/ln)    |         69          |         62         | 0.877419                    |
| wp/lp     | 2.5(wn/ln)  |          60         |         62         | 0.887097                    |
| wp/lp     | 3(wn/ln)    |         54          |         62         | 0.891935                    |
| wp/lp     | 4(wn/ln)    |          48         |         64         | 0.914516                    |
| wp/lp     | 5(wn/ln)    |          42         |         65         | 0.935484                    |

Table3: Delay table using sky130 ss corner

| **wp/lp** | **x.wn/ln** | **Rise delay (ps)** | **Fall delay(ps)** | **switching threshold (V)** |
|-----------|-------------|---------------------|--------------------|-----------------------------|
| wp/lp     | 1(wn/ln)    |         226         |         93         |             0.85            |
| wp/lp     | 2(wn/ln)    |         122         |         96         |           0.906452          |
| wp/lp     | 2.5(wn/ln)  |         103         |         97         |           0.908065          |
| wp/lp     | 3(wn/ln)    |          95         |         98         |           0.903226          |
| wp/lp     | 4(wn/ln)    |          76         |         100        |           0.922581          |
| wp/lp     | 5(wn/ln)    |          64         |         102        |           0.935484          |


 
<!---| **wp/lp** | **x.wn/ln** | **Rise delay (ps)** | **Fall delay(ps)** | ** Switching threshold (V)** |
|-----------|-------------|---------------------|--------------------|-------------------------
| wp/lp     | 1.wn/ln     |         183        |         78       |    0.819836 |     
| wp/lp     | 2.wn/ln     |         104         |         81        |  0.858065  |  
| wp/lp     | 2.5(wn/ln)   |         88       |         81       |  0.870968    |
| wp/lp     | 3.wn/ln     |         77         |         78       |  0.879032   |        
| wp/lp     | 4.wn/ln     |         64         |         82       |  0.896774   |     
| wp/lp     | 5.wn/ln     |         57         |         85        |  0.909677  | ---->   


<!---| **(Wp/Lp)um** | **(Wn/Ln)um** | **Switching threshold (Vm) V** | **Rise delay(ps)**     | **Fall delay (ps)**    |
|---------------|---------------|--------------------------------|------------------------|------------------------|
| Wp/Lp         | 1.Wn/Ln       | ~0.99                          | 1.1641 - 1.0159 = 148  | 2.07769 - 2.00578= 72  |
| Wp/Lp         | 2.Wn/Ln       | 1.1519 = ~1.2                  | 1.09627 -01.01591 = 80 | 2.08198 - 2.00582 = 76 |
| Wp/Lp         | 3.Wn/Ln       | 1.25129 =~1.25                 | 1.07287 - 1.0159 = 57  | 2.08619 - 2.00583 = 80 |
| Wp/Lp         | 4.Wn/Ln       | 1.32054 =~1.32                 | 1.06101 -1.01589 = 45  | 2.0933 - 2.00593 = 84  |
| Wn/Ln         | 5.Wn/Ln       | 0.918462 =~1.4                 | 1.05387 - 1.01548 = 37 | 2.0944 - 2.00582 = 88  |--->

<!---Certain observations is made from the above table

- For (Wp/Lp) = 2.(Wn/Ln), the rise and fall delay is observe to be approximately equal and thus these set of transistor pair is best for the clock buffers.
- The other transistor sizes can still find application in the data path as regular inverters/buffer where compromise on the rise and fall delay can be made.
-  When Wp/Lp is increased, the rise delay is isgnificantly reduced because time required for the output capacitor to charge decreases significantly and the reason is the availability of a bigger area to charge the capacitor.
   Ron(PMOS) ~ 2.5*Ron(NMOS)
- Due to fabrication process obtaining the exact sizes of the transistor is not possible and the small variation in the switching threshold between the different transistor sizes act as boon in managing the device behaviour.--->

6. Spice deck to find the noise margin of a cmos inverter circuit for (wp/lp=1/0.15) and (wn/ln=0.36/0.15)

```bash
Spice deck to find the noise margin of a cmos inverter circuit for (wp/lp=1/0.15) and (wn/ln=0.36/0.15)

*Model description
.param temp = 27

*Netlist description

XM1 out in Vdd Vdd sky130_fd_pr__pfet_01v8 W =1 L = 0.15
XM1 out in 0 0 sky130_fd_pr__nfet_01v8 W =0.36 L = 0.15 
cload out 0 50fF

Vdd Vdd 0 1.8V
Vin in 0 1.8V

*include model file
.lib "../sky130_fd_pr/models/sky130.lib.spice" tt

*simulation commands
.op
.dc Vin 0 1.8V 0.01

*interactive interpretor commandw
.control
run
display
setplot dc1
plot out vs in
.endc
.end

```

![09_noisemargin_wf](https://user-images.githubusercontent.com/63381455/152843755-83a4eb04-74a0-4db2-91c3-3469e562ad19.png)


![09_noisemargin](https://user-images.githubusercontent.com/63381455/152843762-231be30e-8ef5-4314-a2b0-8ec7773097e1.png)


The noise margin defines the maximum allowable range for which the device can operate properly. NMH and NML is calculated using the formula below:

``` bash
   NMH = VOH - VIH
   NML = VIL - VOL
```   
   From the above plot 
```bash
VOH = 1.63125
VIH = 0.964516
NMH = 1.63125 - 0.964516 
    = 0.666734
```
```bash
VIL = 0.801613
VOL = 0.121875
NML = 0.801613 - 0.121875 
    = 0.679738
```
<!---The table below shows the noise margin for differnt transistor sizes. It is obseve that when (Wp/Lp) = 2.(Wn/Ln) there is a rise at the NMH because PMOS is responsible for holding the charges on the capacitance. When the size of PMOS is increased, a low-resistance path from supply to the capacitance is formed and as a result of that, the capacitance is able to retain the charge for a longer amount of time resulting in an increased NMH. - When (Wp/Lp) = 4.(Wn/Ln) there is a drop at the NML because the NMOS has now become weaker than the PMOS - When (Wp/Lp) = 5.(Wn/Ln) the NMh almost comes to a static point. - In the above table, NMl is not affected much but NMh has increased by 120mV but this range is still acceptable and this proves the CMOS inverter robustness with respect to the Noise MaFinally, the areas that can be used for digital and analog applications are stated in the figure below:
 
| **(Wp/Lp)um** | **(Wn/Ln)um** | **Switching threshold (Vm) V** | **NMH** | **NML** |
|---------------|---------------|--------------------------------|---------|---------|
| Wp/Lp         | 1.Wn/Ln       | ~0.99                          | 0.3V    | 0.3V    |
| Wp/Lp         | 2.Wn/Ln       | 1.1519 = ~1.2                  | 0.35V   | 0.3V    |
| Wp/Lp         | 3.Wn/Ln       | 1.25129 =~1.25                 | 0.4V    | 0.3V    |
| Wp/Lp         | 4.Wn/Ln       | 1.32054 =~1.32                 | 0.42V   | 0.27V   |
| Wn/Ln         | 5.Wn/Ln       | 0.918462 =~1.4                 | 0.42V   | 0.27V   |--->


7. Spice deck to check the dc characteristics of the CMOS transistor with varying power supply

This program shows another CMOS robustness characteristics, power supply variation. The output below shows that the change input voltage of CMOS keeping the size of the transistor constant does not have any impact on the behaviour of CMOS.
```bash
Spice deck to check the dc characteristics of the CMOS transistor with varying power supply

*Model parameters
.param temp =27

*Netlist description
XM1 out in Vdd Vdd sky130_fd_pr__pfet_01v8 w=1 l=0.15
XM2 out in 0 0 sky130_fd_pr__nfet_01v8 w=0.36 l=0.15
cload out 0 50fF
Vin in 0 1.8V
Vdd Vdd 0 1.8V

*include model file
.lib "../sky130_fd_pr/models/sky130.lib.spice" tt

*Control command

.control
let powersupply = 1.8
alter Vdd = powersupply
     let supplyvoltagevariation = 0
     dowhile supplyvoltagevariation < 6
     dc Vin 0 1.8 0.01
     let powersupply = powersupply - 0.2
     alter Vdd = powersupply
     let supplyvoltagevariation = supplyvoltagevariation + 1
    end

plot dc1.out vs in dc2.out vs in dc3.out vs in dc4.out vs in dc5.out vs in xlabel "input voltage [V]" 
ylabel "output voltage [V]"  title "Inverter dc characteristics as a function of supply voltage" 

.endc
.end
```

![10_powsup_var_wf](https://user-images.githubusercontent.com/63381455/152846501-56336f02-80c4-4d1e-8522-1599c3b17e11.png)


![10_powsup_var](https://user-images.githubusercontent.com/63381455/152846503-41a87b13-312b-40d7-a339-d348134f113b.png)


8. Spice deck to plot the dc characteristics of a cmos inverter circuit for 7wp/0.15lp and 0.42wn/0.15ln

 This program shows another robust behaviour of the CMOS inverter, device variation. As seen from the output characteristics it is observe that the no change in the behavioural aspect of CMOS occurs accept that the switching threshold is shifted towards right.

```bash
Spice deck to plot the dc characteristics of a cmos inverter circuit for 7wp/0.15lp and 0.42wn/0.15ln

*Model Description
.param temp=27

*Including model file
.lib "../sky130_fd_pr/models/sky130.lib.spice" tt

*Netlist Description
XM1 out in vdd vdd sky130_fd_pr__pfet_01v8 w=7 l=0.15
XM2 out in 0 0 sky130_fd_pr__nfet_01v8 w=0.42 l=0.15
Cload out 0 50fF
Vdd vdd 0 1.8V
Vin in 0 1.8V

*simulation commands

.op
.dc Vin 0 1.8 0.01

*interactive interpretor command
.control
run
setplot dc1
display
plot out vs in
.endc
.end

```

![11_deviceVariation_wfVm](https://user-images.githubusercontent.com/63381455/152848582-b940d2db-eb05-4100-86c1-8094914cc61b.png)


![11_deviceVariation](https://user-images.githubusercontent.com/63381455/152848590-12cbdb5b-bae5-40da-84b6-fe99ccbd65b3.png)
