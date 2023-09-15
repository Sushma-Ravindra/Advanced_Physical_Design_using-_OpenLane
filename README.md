# Advanced_Physical_Design_using-_OpenLane


## Table of Contents

- [Day1-Introduction](#Day1-introduction)
- [Day-2- Floorplan and Library Cells](#day-2--floorplan-and-library-cells)
- [Day-3-Magic Layout and NG spice characterization](#day-3-magic-layout-and-ng-spice-characterization)
- [Acknowledgements](#acknowledgements)
  


## Day1

<details>
  <summary>Talking to Computers</summary>


An Arduino board is somewhat similiar to what is aimed to be designed at the end of the course. Breaking its componenets down for better understanding of what it looks like and what is to be designed is what will follow. The Arduino processr contains most essentially, an SOC RISCV chip, pads, and ssome foundary Ips such as ADC,SRAM DSP etc. It also contains Macros such as SPI block etc.

![image](https://github.com/Sushma-Ravindra/Advanced_Physical_Design_using-_OpenLane/assets/141133883/f42b60ad-803f-44a3-a747-4b8b5c90363b)


ISA: In layman's terms, ISA or Instruction Set Architecture is simply the language of the computers. The RISCV architecture implements instructions into the CPU core of the computer. The applications use high level languages which is compiled by the complier into instrictions for the computer which is further converted into binary by the assembler which is finally understood by the hardware

  </details>
  <details>
  <summary>SOC design and OpenLane</summary>

Digital ASIC design using Open Source Tools: 
1) RTL Designs: libcores.org, opencores.org,github.com
2) EDA Tools: Qflow, OpenRoad, OpenLane
3) PDK: Google:Skywater
  
ASIC flow: RTL TO GSDII flow: 
__Synthesis -> Floor and PowerPlan -> Placement -> Clock Tree Synthesis -> Routing -> SignOff (Tapeout)__


Synthesis: RTL to gate level Netlist. 
FloorPLan: Patrition and pinrows ets. Powerpin connections to rails
PLacement: Alignment- Global and Detailed 
CTS: Clock Network design
Routing: Implement Interconnect using metal layers, Global and deayled routing 
GDSII: DRC,LVS,STA and tapeout.


Challenges with Open Source Tools: Configuration, Calibration and some missing tools can be encountered while building an ASIC chip which must be effectively dealt with.

OpenLane and STrive Chipsets

Skywater PDK is used. Openlane provides a large number ofdesign examples and can be used to harden macros and chips. It is containerized and tuned for skywater130nm pdk.


OpenLane ASIC flow: To build clean GDSII with no human interaction. 

The following diagram gives a detailed explanantion of ASIC flow through OpenLane


![image](https://github.com/Sushma-Ravindra/Advanced_Physical_Design_using-_OpenLane/assets/141133883/56d2d88a-3185-428c-ad7e-21c89dbc6e22)

Below is the simplified flow:

    RTL Design (Register-Transfer Level): At this stage, engineers create a high-level description of the desired chip's functionality using a hardware description language like VHDL or Verilog. This description defines how data moves between registers and logic gates in the chip.

    Synthesis: The RTL code is synthesized into a gate-level representation. This step transforms the high-level description into a netlist of logic gates that can be implemented in silicon. Optimization techniques are applied to improve performance, power consumption, and area usage.

    Floorplanning: Engineers create a layout plan, or floorplan, that specifies where different functional blocks will be placed on the chip. This step considers factors like power distribution and signal routing.

    Placement : The synthesized gates are physically placed on the chip according to the floorplan. This step aims to minimize the physical distance between related gates to improve performance.

    Routing: Wires are connected between the gates to establish the logical connections defined in the RTL code. This step involves complex algorithms to optimize for speed, power, and area.

    Physical Verification : The design is thoroughly checked for issues like timing violations, manufacturing defects, and design rule violations. Tools ensure that the chip will function correctly and be manufacturable.

    Mask Generation: The final layout, or mask, is generated based on the design. This mask provides a blueprint for the semiconductor fabrication process.

    Manufacturing: The mask is used to manufacture the physical semiconductor wafer in a semiconductor fabrication facility (fab). This involves a series of intricate processes, including photolithography, etching, and doping, to create the actual chip.

    Testing: After fabrication, each chip is rigorously tested to identify defects and ensure functionality.

    Packaging: The individual chips are packaged into protective casings that include pins or connectors for interfacing with other electronic components.

    GDS2 Format: GDS2 is a file format used to represent the final chip layout and mask data. It contains information about the physical layout of the chip, including the positions of gates, wires, and other elements.




  </details>


  </details>
  <details>
  <summary>Opensource EDA tools</summary>

__Installing OpenLane__

```
git clone https://github.com/The-OpenROAD-Project/OpenLane
cd OpenLane
make
make test

```
__Invoking OpenLne__

```
make mount
./flow.tcl -interactive
package reqire openlane 0.9
prep -design picorv32a
run_synthesis

```

![Screenshot from 2023-09-10 10-46-13](https://github.com/Sushma-Ravindra/Advanced_Physical_Design_using-_OpenLane/assets/141133883/41dad0ac-486a-4aa5-b7e5-a05d4d8b9866)


![Screenshot from 2023-09-10 10-46-06](https://github.com/Sushma-Ravindra/Advanced_Physical_Design_using-_OpenLane/assets/141133883/58eae739-fec1-4eb8-990b-001d73b50c94)



Below obtained is the synthesis report:



![Screenshot from 2023-09-10 10-52-43](https://github.com/Sushma-Ravindra/Advanced_Physical_Design_using-_OpenLane/assets/141133883/5da4cd23-0fe1-48e5-8eed-e7c2a0178a47)


![Screenshot from 2023-09-10 10-56-46](https://github.com/Sushma-Ravindra/Advanced_Physical_Design_using-_OpenLane/assets/141133883/a76aa8ad-339c-4990-abcb-3d409fdfad04)

  
![Screenshot from 2023-09-10 10-59-14](https://github.com/Sushma-Ravindra/Advanced_Physical_Design_using-_OpenLane/assets/141133883/4f084d39-b8ab-4cfc-9231-f9ecbb6adb37)

 
  
</details>



## Day2 

<details>
  <summary>Chip Floorplanning Considerations</summary>


__Utilization Factor and Floorplanning__

Physical processor core is an independent execution unit that can run one program thread at a time in parallel with other cores.

Processor die is a single continuous piece of semiconductor material (usually silicon). A die can contain any number of cores. Up to 15 are available on the Intel product line. Processor die is where the transistors making up the CPU actually reside.

Processor package is what you get when you buy a single processor. It contains one or more dies, plastic/ceramic housing for dies and gold-plated contacts that match those on your motherboard.

Core:
The width of the core typically refers to the physical size or dimensions of the central processing unit (CPU) or processor core within a microchip. It is usually measured in nanometers (nm) or micrometers (µm). For example, you might hear about a "14nm core" or a "7nm core," indicating the feature size of the core's transistors.
The height of the core is not commonly referred to in the same way as the width. Instead, the core's size is usually described in terms of its area, which is determined by multiplying its width and height.

Die (also known as the chip or silicon die):
The width of the die is typically the physical measurement of the semiconductor wafer after all the individual ICs (integrated circuits) have been fabricated on it but before they are cut apart. Die widths can vary significantly depending on the specific manufacturing process and the design of the chips being produced. They can range from a few millimeters to several centimeters or more.

Similar to the core, the height of the die is not a common parameter of discussion. Instead, the die's size is often described in terms of its area, which is the product of its width and height.

![Screenshot from 2023-09-10 11-16-00](https://github.com/Sushma-Ravindra/Advanced_Physical_Design_using-_OpenLane/assets/141133883/2b43fd32-ef7d-44bc-ae01-a748dfa74705)

Utilization factor of 1 means the chip is square chip.


```
  Utilisation Factor =    Area occupied by netlist
                       __________________________
                          Total area of core

  Aspect Ratio =  Height
                 ________
                  Width

```





__Concept of Preplaced Cells__

Pre-placed cells are IPs comprising huge combinational logic which once placed, maintain a fixed position. Since they are placed before placement and routing, the are known as pre-placed cells.
These pre placed cells can be implemented as an out of the box compoenent of the whole circuit. Therefore , it provides for reuse of these IP blocks. Ips can be Memory blocks, clocks or gating cells, comparator or muxes. 
Since they are placed before placement and routing, the are known as pre-placed cells.Once their placement is decided then the synthesis tool will not touch them.
Unlike standard cells in a digital design, which are typically placed automatically by place-and-route tools, pre-placed cells are manually positioned by the designer. The designer selects specific locations on the chip for these cells based on design considerations, performance requirements, or other constraints.
Thus they are implemenetd once and reused again and again. Thus they are placed cliser to input pins.


![image](https://github.com/Sushma-Ravindra/Advanced_Physical_Design_using-_OpenLane/assets/141133883/b35f90f5-5c84-41a2-bd7d-62fd87667e5d)


__Decoupling Capacitors__

Pre-placed cells must then be surrounded with decoupling capacitors (decaps). The resistances and capacitances associated with long wire lengths can cause the power supply voltage to drop significantly before reaching the logic circuits. This can lead to the signal value entering into the undefined region, outside the noise margin range. Decaps are huge capacitors charged to power supply voltage and placed close the logic circuit. Their role is to decouple the circuit from power supply by supplying the necessary amount of current to the circuit. They pervent crosstalk and enable local communication.
Switching requires huge cuurent surge by the components.
Decoupling capacitors are essential components in electronic circuit design that help maintain a stable power supply, filter out noise, and improve the overall performance and reliability of electronic systems, particularly in digital and mixed-signal applications. Proper selection, placement, and sizing of decoupling capacitors are critical for their effectiveness in reducing noise and maintaining voltage stability.
They are huge capacitances which are charged, they help in decoupling the circuits from the supply during switching. During switching , it discharges, else it replenished from the power supply.

![image](https://github.com/Sushma-Ravindra/Advanced_Physical_Design_using-_OpenLane/assets/141133883/6addf8c7-5509-4d44-a888-e3bcc16028c7)



__Power Planning__

Consider a 16 bit bus where in multiple capacitance need to discharge simultaneouslt based on the 16 bit input shream, thus a ground bounce is observed which is a surge compared to the normal 0 voltage value. Similar effect can be observed in Vdd as well.
Each block on the chip, however, cannot have its own decap unlike the pre-placed macros. Therefore, a good power planning ensures that each block has its own VDD and VSS pads connected to the horizontal and vertical power and GND lines which form a power mesh.


__Pin PLacement__

The netlist defines connectivity between logic gates. The place between the core and die is utilised for placing pins. The connectivity information coded in either VHDL or Verilog is used to determine the position of I/O pads of various pins. Then, logical placement blocking of pre-placed macros is performed so as to differentiate that area from that of the pin area.
The netlist specifies how the logic gates within the chip are interconnected. It provides information about which gates are connected to one another and how signals flow through the design.
After the I/O pad positions are determined, the design process includes logical placement blocking of pre-placed macros. This step involves arranging pre-placed macros (blocks of predefined logic) in a way that distinguishes them from the area reserved for the I/O pins. This separation ensures that the macros do not interfere with the connectivity and signal paths associated with the pins.
Clock pins are bigger in size because they drive the whole circuit and so we need least resistnace path for it.

![WhatsApp Image 2023-09-10 at 12 02 58](https://github.com/Sushma-Ravindra/Advanced_Physical_Design_using-_OpenLane/assets/141133883/967c0dcc-fd8d-45e6-88de-bf56a1ed7627)



__Floorplan using openlane__

To run floorplan in openlane:

```
run_floorplan

```

To view results Magic is invoked after moving to the results/floorplan directory:

```
cd /OpenLane/designs/picorv32a/runs/RUN_2023.09.10_07.02.28/results/floorplan

magic -T /home/sushma/.volare/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.min.lef def read picorv32.def

```



![image](https://github.com/Sushma-Ravindra/Advanced_Physical_Design_using-_OpenLane/assets/141133883/b6d622ab-e8f7-4322-bb5a-77e2096efe6c)


![image](https://github.com/Sushma-Ravindra/Advanced_Physical_Design_using-_OpenLane/assets/141133883/cab845d6-30b0-4414-961f-0066e4c3df49)



</details>



<details>
  <summary> Library Binding and Placement </summary>

 __Netlist Binding and Initial Place design__

 Library contains info about shape, size, delays,various flavours etc of every cell(gates).
 First we need to bind the netlist with physical cells. We have shapes for OR, AND and every cell for pratice purpose. But in reality we dont have such shapes, we have give an physical dimensions like rectangles or squares weight and width.This information is given in libs and lefs. Now we place these cells in our design by initilaising it.
 The next step in the OpenLANE ASIC flow is placement. The synthesized netlist is to be placed on the floorplan. Placement is perfomed in 2 stages:
 Global Placement: It finds optimal position for all cells which may not be legal and cells may overlap. Optimization is done through reduction of half parameter wire length.
 Detailed Placement: It alters the position of cells post global placement so as to legalise them.
 Legalisation of cells is important from timing point of view.


__Optimize Placement__

Considering wire lengths that ccontribute to its capacitances and further delay, we try to optimize the placements by inserting repeaters etc. 

![WhatsApp Image 2023-09-10 at 14 06 20](https://github.com/Sushma-Ravindra/Advanced_Physical_Design_using-_OpenLane/assets/141133883/e9b2b829-0a75-4cc8-a88f-ec73717e190f)


__Final Placement OPtimization__

Timing analysis is done for the existing conditions wherein buffers are already placed. For high frequrncy circuits, components are placed close to each other so as to not cause any sort of wire delays.

__Need for Library Characterization__


STEPS: 
1) Logic Synthesis: Output will be the netlist with gates and their interconnections.
2) Floorplan: Netlist import and sizes of die and wafer are decided
3) Placement: Positions are decided based on timing constraints.
4) ClockTreeSynthesis: Clock signal reaching all clock ports.
5) Routing: Interconnects and wiring.

   ![WhatsApp Image 2023-09-10 at 14 55 13](https://github.com/Sushma-Ravindra/Advanced_Physical_Design_using-_OpenLane/assets/141133883/0cab3e68-76ab-4a27-9415-7041432e692c)


Library characterization is the process of characterizing electronic components and gates, such as logic gates, flip-flops, and other building blocks, to create models that accurately represent their behavior under various conditions. This characterization provides information about how components respond to different inputs, delays, power consumption, and more. These components are common in all the above steps of the design flow.

__Congestion aware Placement__

![image](https://github.com/Sushma-Ravindra/Advanced_Physical_Design_using-_OpenLane/assets/141133883/6b9ab3a6-0af7-40dd-a14d-ba20a4f7554d)


![image](https://github.com/Sushma-Ravindra/Advanced_Physical_Design_using-_OpenLane/assets/141133883/3121648b-a571-4cfc-9187-7b36b11e3c39)


![image](https://github.com/Sushma-Ravindra/Advanced_Physical_Design_using-_OpenLane/assets/141133883/291c7b00-0ab1-4c8d-aed9-2a3eb6618738)

</details>

<details>
  <summary> Cell design and Characterization flows </summary>
  

__Inputs for cell design flow__

Steps:

![image](https://github.com/Sushma-Ravindra/Advanced_Physical_Design_using-_OpenLane/assets/141133883/c029d960-899b-4ae4-b1a3-b7a1956b8ac4)



Inputs: PDKs:Basically reules given by the foundries, such as DRCs and LVS rules spice models and user defined specs.

User defined specs: Cell height is difference between ground and supply raild. Similarly, width is defined by drive strength which must be designed by library developer.
Other such specs are power supply, metal layers, pin locations etc.


![image](https://github.com/Sushma-Ravindra/Advanced_Physical_Design_using-_OpenLane/assets/141133883/1c4ec075-ea8d-4494-9942-0c8253ac39a8)


__Circuit Design step__

Design Steps: Circuit design, characerization of parameters such as drain current, vth etc. Output of this stage is a .cdl file

![image](https://github.com/Sushma-Ravindra/Advanced_Physical_Design_using-_OpenLane/assets/141133883/cf9e8efc-d107-4353-9bdb-177525b62a6b)


__Layout design step__

The function is implemented using NMOS and PMOS and then their network graphs is obtained. Then Euler's path(path traversed only 1 time) enables us to draw the stick diagrams.

![image](https://github.com/Sushma-Ravindra/Advanced_Physical_Design_using-_OpenLane/assets/141133883/2c101c91-5752-44bb-add0-46d0150980f6)

This stick diagram is converted into layout using the DRC rules etc.  This is the library cell. 
The output will be GDSII, lef and netlist. ALso, timing, noise etc can also be extracted from here. 


__Characterization Flow__


Characterization in VLSI refers to the process of analyzing and documenting the electrical behavior of electronic components, such as transistors, logic gates, memory cells, and standard cells, under various operating conditions. Characterization is essential for accurate circuit simulation and helps ensure that integrated circuits (ICs) meet their performance, power, and timing requirements.
The following is the flow:

1)Read in the models and tech files
2)Read extracted spice Netlist
3)Recognise behavior of the cells
4)Read the subcircuits
5)Attach power sources
6)Apply stimulus to characterization setup
7)Provide neccesary output capacitance loads
8)Provide neccesary simulation commands


![image](https://github.com/Sushma-Ravindra/Advanced_Physical_Design_using-_OpenLane/assets/141133883/5abfbaae-3c89-4eb6-b32b-a8aa2ba3c7bc)



Now all these 8 steps are fed in together as a configuration file to a characterization software called GUNA. This software generates timing, noise, power models. These .libs are classified as Timing characterization, power characterization and noise characterization.

![image](https://github.com/Sushma-Ravindra/Advanced_Physical_Design_using-_OpenLane/assets/141133883/db1941b4-a859-41c5-8973-e173d1d5b463)


</details>

<details>
  <summary> General Timing characterization Parameters</summary>


__Timing threshold definitions__



Timing characterization is essential for ensuring that digital circuits meet their timing requirements. It involves determining the timing behavior of various elements within the IC, such as logic gates, flip-flops, and interconnections, under different operating conditions.

Setup Time: The amount of time before the clock edge that data must be stable to be correctly captured by a flip-flop. Hold Time: The amount of time after the clock edge that data must remain stable to be correctly captured.

Delay Calculation: Characterize the delays of logic gates, flip-flops, and interconnects. Calculate the propagation delay from the input of a circuit element to its output.

Clock Skew Analysis: Analyze the variation in clock arrival times at different parts of the circuit. Clock skew can impact the synchronization of sequential elements and can be critical in high-speed designs.

Corner Case Characterization: Perform timing characterization under different process corners (fast corner and slow corner) to account for manufacturing variations. Analyze worst-case scenarios to ensure robust operation.

Post-Layout Timing Analysis: After physical design and layout, perform post-layout timing analysis to account for the actual interconnection delays and verify that the design still meets timing constraints.

```
Timing defintion 	  Value
slew_low_rise_thr 	20% value
slew_high_rise_thr 	80% value
slew_low_fall_thr 	20% value
slew_high_fall_thr 	80% value
in_rise_thr 	      50% value
in_fall_thr 	      50% value
out_rise_thr 	      50% value
out_fall_thr 	      50% value

```

__Propogation Delay and Transistion Time__

Propagation Delay The time difference between when the transitional input reaches 50% of its final value and when the output reaches 50% of its final value. Poor choice of threshold values lead to negative delay values. Even though we have taken good threshold values, sometimes depending upon how good or bad the slew might be, the delay might be still +ve or -ve.
This can happen if threshold somehow moves upwards or downwards. 

![image](https://github.com/Sushma-Ravindra/Advanced_Physical_Design_using-_OpenLane/assets/141133883/d56e74d3-edce-416f-aff3-f427e4cb4032)


```
Propagation delay = time(out_*_thr) - time(in_*_thr)

```

Transistion Time The time it takes the signal to move between states is the transition time , where the time is measured between 10% and 90% or 20% to 80% of the signal levels.


![image](https://github.com/Sushma-Ravindra/Advanced_Physical_Design_using-_OpenLane/assets/141133883/2f0514d7-f902-4ca6-a97f-98a8ef9a2b44)


```
Rise transition time = time(slew_high_rise_thr) - time (slew_low_rise_thr)

Low transition time = time(slew_high_fall_thr) - time (slew_low_fall_thr)

```

</details>

## Day3


<details>
  <summary> Labs for NGSPice simulations of CMOS Inverter</summary>

  
__Spice deck creation for CMOS Inverter__

Spice Deck: It is basically a netlist. It has information about components, interconnects etc.
Here a Spice Deck is created for the CMOS inverter.

Steps:

Component connectivity - Connectivity of the Vdd, Vss,Vin, substrate. Substrate tunes the threshold voltage of the MOS.
Component values - values of PMOS and NMOS, Output load, Input Gate Voltage, supply voltage.
Node Identification and naming - Nodes are required to define the SPICE Netlist For example M1 out in vdd vdd pmos w = 0.375u L = 0.25u , cload out 0 10f
Simulation commands
Model file - information of parameters related to transistors Simulation of CMOS using different width and lengths. From the waveform, irrespective of switching the shape of it are almost the same.


![image](https://github.com/Sushma-Ravindra/Advanced_Physical_Design_using-_OpenLane/assets/141133883/ac422e24-096d-4d9d-b797-2ff3f9798d2c)



Let us now simulate the inverter circuit in ngspice

![image](https://github.com/Sushma-Ravindra/Advanced_Physical_Design_using-_OpenLane/assets/141133883/b30a1447-ab92-4000-a393-616526967e82)



The model file for simulating this is as given below:

```
SPICE 3f5 Level 8, Star-HSPICE Level 49, UTMOST Level 8

.lib cmos_models 
* DATE: Feb 23/01
* LOT: T0BM                  WAF: 07
* Temperature_parameters=Default
.MODEL nmos  NMOS (                                LEVEL   = 49
+VERSION = 3.1            TNOM    = 27             TOX     = 5.8E-9
+XJ      = 1E-7           NCH     = 2.3549E17      VTH0    = 0.3907535
+K1      = 0.4376003      K2      = 8.265151E-3    K3      = 4.214601E-3
+K3B     = -3.7220937     W0      = 2.517345E-6    NLX     = 2.310668E-7
+DVT0W   = 0              DVT1W   = 0              DVT2W   = 0
+DVT0    = 0.2411602      DVT1    = 0.3707226      DVT2    = -0.5
+U0      = 316.5922683    UA      = -9.89493E-10   UB      = 2.154013E-18
+UC      = 2.474632E-11   VSAT    = 1.254499E5     A0      = 1.2735648
+AGS     = 0.2428704      B0      = 2.579719E-8    B1      = -1E-7
+KETA    = 4.87168E-4     A1      = 0              A2      = 0.5196633
+RDSW    = 120            PRWG    = 0.5            PRWB    = -0.2
+WR      = 1              WINT    = 2.357855E-8    LINT    = 1.210018E-9
+DWG     = 2.292632E-9
+DWB     = -9.94921E-10   VOFF    = -0.1039771     NFACTOR = 1.3905578
+CIT     = 0              CDSC    = 2.4E-4         CDSCD   = 0
+CDSCB   = 0              ETA0    = 3.894977E-3    ETAB    = 7.800632E-4
+DSUB    = 0.0307944      PCLM    = 1.7312397      PDIBLC1 = 0.999135
+PDIBLC2 = 4.850036E-3    PDIBLCB = -0.0866866     DROUT   = 0.8612131
+PSCBE1  = 7.995844E10    PSCBE2  = 1.457011E-8    PVAG    = 0.0099984
+DELTA   = 0.01           RSH     = 5              MOBMOD  = 1
+PRT     = 0              UTE     = -1.5           KT1     = -0.11
+KT1L    = 0              KT2     = 0.022          UA1     = 4.31E-9
+UB1     = -7.61E-18      UC1     = -5.6E-11       AT      = 3.3E4
+WL      = 0              WLN     = 1              WW      = -1.22182E-16
+WWN     = 1.2127         WWL     = 0              LL      = 0
+LLN     = 1              LW      = 0              LWN     = 1
+LWL     = 0              CAPMOD  = 2              XPART   = 0.4
+CGDO    = 3.11E-10       CGSO    = 3.11E-10       CGBO    = 1E-12
+CJ      = 1.741905E-3    PB      = 0.9876681      MJ      = 0.4679558
+CJSW    = 3.653429E-10   PBSW    = 0.99           MJSW    = 0.2943558
+CF      = 0              PVTH0   = -0.01          PRDSW   = 0
+PK2     = 2.589681E-3    WKETA   = -1.866069E-3   LKETA   = -0.0166961      )
*
.MODEL pmos  PMOS (                                LEVEL   = 49
+VERSION = 3.1            TNOM    = 27             TOX     = 5.8E-9
+XJ      = 1E-7           NCH     = 4.1589E17      VTH0    = -0.583228
+K1      = 0.5999865      K2      = 6.150203E-3    K3      = 0
+K3B     = 3.6314079      W0      = 1E-6           NLX     = 1E-9
+DVT0W   = 0              DVT1W   = 0              DVT2W   = 0
+DVT0    = 2.8749516      DVT1    = 0.7488605      DVT2    = -0.0917408
+U0      = 136.076212     UA      = 2.023988E-9    UB      = 1E-21
+UC      = -9.26638E-11   VSAT    = 2E5            A0      = 0.951197
+AGS     = 0.20963        B0      = 1.345599E-6    B1      = 5E-6
+KETA    = 0.0114727      A1      = 3.851541E-4    A2      = 0.614676
+RDSW    = 1.496983E3     PRWG    = -0.0440632     PRWB    = -0.2945454
+WR      = 1              WINT    = 7.879211E-9    LINT    = 2.894523E-8
+DWG     = -1.112097E-8
+DWB     = 9.815716E-9    VOFF    = -0.1204623     NFACTOR = 1.2259401
+CIT     = 0              CDSC    = 2.4E-4         CDSCD   = 0
+CDSCB   = 0              ETA0    = 0.3325261      ETAB    = -0.0623452
+DSUB    = 0.9206875      PCLM    = 0.833903       PDIBLC1 = 9.948506E-4
+PDIBLC2 = 0.0191187      PDIBLCB = -1E-3          DROUT   = 0.9938581
+PSCBE1  = 2.887413E10    PSCBE2  = 8.325891E-9    PVAG    = 0.8478443
+DELTA   = 0.01           RSH     = 3.6            MOBMOD  = 1
+PRT     = 0              UTE     = -1.5           KT1     = -0.11
+KT1L    = 0              KT2     = 0.022          UA1     = 4.31E-9
+UB1     = -7.61E-18      UC1     = -5.6E-11       AT      = 3.3E4
+WL      = 0              WLN     = 1              WW      = 0
+WWN     = 1              WWL     = 0              LL      = 0
+LLN     = 1              LW      = 0              LWN     = 1
+LWL     = 0              CAPMOD  = 2              XPART   = 0.4
+CGDO    = 2.68E-10       CGSO    = 2.68E-10       CGBO    = 1E-12
+CJ      = 1.864957E-3    PB      = 0.976468       MJ      = 0.4614408
+CJSW    = 3.118281E-10   PBSW    = 0.6870843      MJSW    = 0.3021929
+CF      = 0              PVTH0   = 6.397941E-3    PRDSW   = 30.410214
+PK2     = 2.100359E-3    WKETA   = 5.428923E-3    LKETA   = -0.0111599      )
*
.endl


```

To run the circuit in ngspice,
```
  source cmos.cir
  run
  setplot
  display
  plot out vs in

```

![image](https://github.com/Sushma-Ravindra/Advanced_Physical_Design_using-_OpenLane/assets/141133883/2eca6174-bc36-4679-b3c4-2e19a4237e0b)
![image](https://github.com/Sushma-Ravindra/Advanced_Physical_Design_using-_OpenLane/assets/141133883/f172fa8c-f49b-4382-a70f-e70770b0e7e1)
![Screenshot from 2023-09-10 23-01-19](https://github.com/Sushma-Ravindra/Advanced_Physical_Design_using-_OpenLane/assets/141133883/212317e9-8617-4b5f-825b-82bcc35f7ee1)
![image](https://github.com/Sushma-Ravindra/Advanced_Physical_Design_using-_OpenLane/assets/141133883/5b95daa9-1843-41cf-ab13-b30c269f3e41)



__Switching Threshold__

![image](https://github.com/Sushma-Ravindra/Advanced_Physical_Design_using-_OpenLane/assets/141133883/a70a3dfb-c84d-4d38-bcfb-aabf720956e6)

The sitching threshold of a CMOS inverter is the point on the transfer characteristic where Vin equals Vout (=Vm). At this point both PMOS and NMOS are in ON state.

![image](https://github.com/Sushma-Ravindra/Advanced_Physical_Design_using-_OpenLane/assets/141133883/15ea3478-ea97-4b03-85e7-4aacc31b4770)


__Static and Dynamic Simulation__

MOdify the above circuit file as below:

![image](https://github.com/Sushma-Ravindra/Advanced_Physical_Design_using-_OpenLane/assets/141133883/9f6d0e29-1cc3-4285-8f29-c5e03c062d39)


Now run the transient analysis:

![image](https://github.com/Sushma-Ravindra/Advanced_Physical_Design_using-_OpenLane/assets/141133883/d9fd644a-2b89-43b8-a5c3-5c00b950504e)


Now for this rise and fall delays can be calculated by finding difference between out and in curves, to find the difference between two graph points just drag with mouse and it will zoom and then you can just click on the graph. It gives the x and y coordinates.




__VSDSTDCelldesign Lab__


Steps:

```
git clone https://github.com/nickson-jose/vsdstdcelldesign
magic -T /home/sushma/.volare/sky130A/libs.tech/magic/sky130A.tech sky130_inv.mag &

```



![image](https://github.com/Sushma-Ravindra/Advanced_Physical_Design_using-_OpenLane/assets/141133883/92560906-6011-4889-865b-d3c8751cd7a2)

![image](https://github.com/Sushma-Ravindra/Advanced_Physical_Design_using-_OpenLane/assets/141133883/da545018-d088-4cff-be77-cc0880c13ec3)


</details>


<details>
  <summary>Inception of Layout</summary>


__Create Active Regions__

_16 Mask CMOS Fabrication_

1) Selecting a substrate: P type, dimensions, orientation.
   Doping and resisitivity.
2) Create active regions: Pockets in the substrate. Pockets need isolation between each and everyone of them.
   First, SiO2 layer is grown, then Si3N4, on top of these 1um of photoresist.
   
   ![image](https://github.com/Sushma-Ravindra/Advanced_Physical_Design_using-_OpenLane/assets/141133883/28b8a041-8c46-44f2-9f8d-675e9c0a1986)

    Now etching off and then put it into an oxidation furnace.


__Formation of wells__

   3) Formation of n well and p well : The formation of N-well and P-well regions in CMOS technology involves ion implantation using specific dopants. Boron is utilized for P-well formation, while Phosphorus is employed for N-well creation. These dopants are implanted into the silicon substrate to define the N-well and P-well regions, which are essential components for building complementary NMOS and PMOS transistors, respectively.

__Formation of gate__

  4)Formation of gate terminal: The gate terminals for NMOS (N-channel Metal-Oxide-Semiconductor) and PMOS (P-channel Metal-Oxide-Semiconductor) transistors are created through photolithography techniques. In this process, precise patterns are defined on the semiconductor substrate using masks and light exposure. These patterns correspond to the gate electrodes of the transistors, and they play a fundamental role in controlling the transistor's conductivity and operation. By carefully implementing photolithography, the gate terminals for both NMOS and PMOS transistors are formed with high precision, enabling the subsequent steps in transistor fabrication.

__Drain Formation__

  5)LDD (lightly doped drain) formation: Here, additional ion implantation steps are introduced after the formation of the main source and drain regions of the transistor. The key idea is to create lightly doped regions adjacent to the main source and drain regions. These lightly doped regions serve as a buffer between the channel and the heavily doped source and drain regions. The purpose of the LDD regions is to reduce the strength of the electric field near the drain, particularly in the region where the channel meets the drain. This helps to prevent the acceleration of electrons to high energies, which can lead to the hot electron effect. The hot electron effect can cause damage to the gate oxide and result in long-term reliability issues for the transistor.

__SOurce Formation__

  6)Source & drain formation: The formation of the source and drain regions in a semiconductor device is a critical step in the fabrication process. To ensure proper performance and avoid issues like channeling during ion implantation, several techniques are employed, including the use of a screen oxide layer, arsenic implantation, and annealing. Screen Oxide: Before performing the source and drain ion implantation, a thin layer of screen oxide is deposited or grown on the semiconductor wafer's surface. The screen oxide serves as a protective layer during the implantation process. It helps to disperse and slow down the implanted ions, reducing the likelihood of channeling. Arsenic Implantation: Arsenic (As) ions are implanted into the regions of the silicon substrate where the source and drain are to be formed. Arsenic is a common dopant used for N-type (electron-conducting) regions in CMOS technology. The implantation process introduces a controlled amount of arsenic atoms into the silicon lattice, creating N-type doping in the source and drain regions. Annealing: After the arsenic implantation, the wafer is subjected to an annealing process. Annealing involves heating the wafer to high temperatures for a specified duration. During annealing, the implanted arsenic ions are activated, and any damage to the silicon crystal lattice caused by the implantation process is repaired. Annealing helps to ensure that the source and drain regions have the desired electrical properties.

__Interconnects Formation__

  7)Local interconnect formation: Local interconnect formation is a important step in semiconductor device fabrication, enabling the creation of electrical connections between different components on a chip. Screen Oxide Removal (HF Etching): After various processing steps, including source and drain formation, a screen oxide layer is typically deposited or grown on the semiconductor wafer's surface. This screen oxide layer serves as a protective barrier during ion implantation. However, it needs to be removed to allow for the formation of local interconnects. HF Etching: Hydrofluoric acid (HF) is commonly used to selectively etch away the screen oxide. HF is highly effective at removing silicon dioxide (SiO2) while leaving other materials like silicon (Si) and metal layers unaffected. Deposition of Ti (Titanium): Once the screen oxide is removed, the next step involves depositing a layer of titanium (Ti) onto the wafer's surface. Titanium is chosen for its excellent adhesion properties and low electrical resistance. Low-Resistance Contacts: Titanium serves as the base layer for creating low-resistance electrical contacts or interconnects. It acts as an adhesion layer for subsequent metal layers (typically aluminum or copper) that will be deposited to form the actual interconnects. 8.Higher level metal formation: The higher-level metal formation in semiconductor device fabrication involves creating additional layers of metal interconnects to connect various components and ensure proper functionality. Chemical-Mechanical Polishing (CMP) for Planarization: After the initial layers of metal interconnects and insulating layers have been deposited and patterned, the surface of the wafer can become uneven due to the topography of the underlying structures. To ensure a flat and planar surface, CMP is employed. CMP: Chemical-Mechanical Polishing is a process that uses a combination of chemical etching and mechanical abrasion to remove excess material and achieve a smooth, flat surface. It is important for ensuring uniform layer thickness in subsequent metal layers. Top SiN (Silicon Nitride) Layer for Chip Protection: To protect the completed chip from environmental factors, moisture, and physical damage, a top layer of silicon nitride (SiN) is deposited. Silicon nitride is an excellent insulator and provides a robust protective barrier. Chip Protection: This top SiN layer acts as a passivation layer, shielding the underlying components from external influences. It also helps prevent contamination and ensures the long-term reliability of the integrated circuit.

__HIgh Level Metal Formation__ 

  8)Higher level metal formation: CMP for planarization followed by TiN and Tungsten deposition. Top SiN layer for chip protection.

  __Lab: BASIC LAYERS__
  

![image](https://github.com/Sushma-Ravindra/Advanced_Physical_Design_using-_OpenLane/assets/141133883/d27b673f-daed-441b-bfd4-d94e533e14c8)

![image](https://github.com/Sushma-Ravindra/Advanced_Physical_Design_using-_OpenLane/assets/141133883/4595b0f2-7f81-4686-9ba3-d1d34f447c76)

![image](https://github.com/Sushma-Ravindra/Advanced_Physical_Design_using-_OpenLane/assets/141133883/f4e8af8c-2cdc-4a05-bb7a-2704f75f64c9)

  
</details>


<details>
  <summary>Tech file Labs</summary>



  Once the parasitics are extraced into spice, it will look like this

```
    extract all
    ext2spice cthresh 0 rethresh 0
    ext2spice
```


![image](https://github.com/Sushma-Ravindra/Advanced_Physical_Design_using-_OpenLane/assets/141133883/32425498-49df-4c54-8bd2-185d295773bd)


__CHaracterize inverter__

PULSE(V1 V2 Tdelay Trise Tfall Ton Tperiod Ncycles)

![image](https://github.com/Sushma-Ravindra/Advanced_Physical_Design_using-_OpenLane/assets/141133883/88f504a4-2ae2-4db8-a9ed-47c70a156f36)

![image](https://github.com/Sushma-Ravindra/Advanced_Physical_Design_using-_OpenLane/assets/141133883/3352ae6e-a689-4d57-b7c7-db650b5ef42f)


![image](https://github.com/Sushma-Ravindra/Advanced_Physical_Design_using-_OpenLane/assets/141133883/a7daf01c-3cb0-49b5-8137-a5e010b568e0)


thus, X=1.2444408e-08 Y=2.58293


Four timing parameters are used to characterize the inverter standard cell:

    Rise transition: Time taken for the output to rise from 20% of max value to 80% of max value
    Fall transition- Time taken for the output to fall from 80% of max value to 20% of max value
    Cell rise delay = time(50% output rise) - time(50% input fall)
    Cell fall delay = time(50% output fall) - time(50% input rise)





__MAGIC and DRC Rules__

Go to this website:  ``` http://opencircuitdesign.com/magic/ ```

To be able to use its lab contents:

```
wget http://opencircuitdesign.com/open_pdks/archive/drc_tests.tgz
tar xfz drc_tests.tgz
cd drc_tests
magic -d XR met3.mag
```

![Screenshot from 2023-09-11 01-31-24](https://github.com/Sushma-Ravindra/Advanced_Physical_Design_using-_OpenLane/assets/141133883/59ce9d05-17e4-4b14-b44c-be4983d80b29)


Below is the periphery rules link :https://skywater-pdk.readthedocs.io/en/main/rules/periphery.html#m3

![image](https://github.com/Sushma-Ravindra/Advanced_Physical_Design_using-_OpenLane/assets/141133883/e5c76406-f82b-4751-85dd-947fc634203f)



Now create a box and hover over the metal3 on the sidepar and press 'p' on the keyboard (p for paint) , You can also undo by pressing the 'u' key :


You can also make a box on elements to see metalcuts.

Then type below command (to see metalcuts) in magic terminal :

```
cif see VIA2

```
![image](https://github.com/Sushma-Ravindra/Advanced_Physical_Design_using-_OpenLane/assets/141133883/d62185b6-4ce2-4f5b-ba6c-cce768a40596)




__Fix poly.9 error in sky.tech file__

```
load poly.mag

```

![image](https://github.com/Sushma-Ravindra/Advanced_Physical_Design_using-_OpenLane/assets/141133883/4daaf47a-1acb-421a-b0a6-c1268602de27)


Now to edit the sky.tech file. Open it in the text editor using below command:

```
gedit sky130A.tech
```
Now search for the poly.9, there are be 2-3 results: here is one of them :

```
spacing npres *nsd 480 touching_illegal \
	"poly.resistor spacing to N-tap < %d (poly.9)"
```
Modify it to this :
```
spacing npres allpolynonres 480 touching_illegal \
	"poly.resistor spacing to N-tap < %d (poly.9)"
```
![image](https://github.com/Sushma-Ravindra/Advanced_Physical_Design_using-_OpenLane/assets/141133883/0a40ff8d-5764-4398-ab18-30e836343026)

Now save it. You dont have to close the magic after the modifying the sky.tech file. Type the below command in magic's terminal:

tech load sky130A.tech In the warning click on yes.
Then type below command:

drc check


Modified layout :

![Screenshot from 2023-09-11 01-54-11](https://github.com/Sushma-Ravindra/Advanced_Physical_Design_using-_OpenLane/assets/141133883/86a4eb55-bdc1-4f1d-9fb0-facefc858a35)


__Challenge exercise to describe DRC error__

![image](https://github.com/Sushma-Ravindra/Advanced_Physical_Design_using-_OpenLane/assets/141133883/1a04880b-b022-4c24-b458-113013ecd7c3)

![image](https://github.com/Sushma-Ravindra/Advanced_Physical_Design_using-_OpenLane/assets/141133883/10a5d5fc-0174-4181-92c3-4868fc1ebfa9)



```
cif ostyle drc
cif see dnwell_shrink
feed clear
cif see nwell_missing
feed clear
```

![image](https://github.com/Sushma-Ravindra/Advanced_Physical_Design_using-_OpenLane/assets/141133883/a2c5edde-dc28-41bc-aa86-ce1948be8b22)




__Find missing or incorrect rules (creating magic DRC rule)__

Modification:

```
cifmaxwidth nwell_untapped 0 bend_illegal \

	"Nwell missing tap (nwell.4)"
```


TO check DRC 

```
tech load sky130A.tech drc check drc style drc(full) drc check

```

![image](https://github.com/Sushma-Ravindra/Advanced_Physical_Design_using-_OpenLane/assets/141133883/077b380d-6899-4706-bd67-6192bcdec869)



  
</details>

## Day-4

<details>
	<summary>Timing modelling using delay tables</summary>	
</details>

__Steps to convertgrid info to track info__

Ports as specified in tracks.info are required to be at intersection of horizontal and vertical tracks. The CMOS Inverter ports A and Y are on li1 layer. It needs to be ensured that they're on the intersection of horizontal and vertical tracks. 

![Screenshot from 2023-09-11 11-25-48](https://github.com/Sushma-Ravindra/Advanced_Physical_Design_using-_OpenLane/assets/141133883/9c8d24d3-6af3-47f3-b0e3-d83b232ed504)

Track info is now converted to grid info
```
In tkcon prompt: grid 0.46um 0.34um 0.23um 0.17um

```

![image](https://github.com/Sushma-Ravindra/Advanced_Physical_Design_using-_OpenLane/assets/141133883/cd78e20e-2855-456d-bcd2-6a3761118158)


__STeps to convert layout to lef__

Next step is extracting LEF file for the cell. However, certain properties and definitions need to be set to the pins of the cell which aid the placer and router tool. For LEF files, a cell that contains ports is written as a macro cell, and the ports are the declared PINs of the macro. Our objective is to extract LEF from a given layout (here of a simple CMOS inverter) in standard format. Defining port and setting correct class and use attributes to each port is the first step. Ports of the layout are the pins of lef file.

1)Select port->Edit->text and make the following changes

![image](https://github.com/Sushma-Ravindra/Advanced_Physical_Design_using-_OpenLane/assets/141133883/fcd4521f-e7c6-417a-a84b-2b6993d0ad0c)

This is followed for Y, VPWR, VGND

![Screenshot from 2023-09-11 11-44-33](https://github.com/Sushma-Ravindra/Advanced_Physical_Design_using-_OpenLane/assets/141133883/b04d941b-ea68-4a64-8f12-3b9105a6f1da)

![Screenshot from 2023-09-11 11-45-13](https://github.com/Sushma-Ravindra/Advanced_Physical_Design_using-_OpenLane/assets/141133883/fe7e03de-8876-44d7-9676-ef20d24e2a12)

![Screenshot from 2023-09-11 11-46-21](https://github.com/Sushma-Ravindra/Advanced_Physical_Design_using-_OpenLane/assets/141133883/b73e729f-4b92-4f68-9077-990d53069760)

![image](https://github.com/Sushma-Ravindra/Advanced_Physical_Design_using-_OpenLane/assets/141133883/ae8964da-cde7-445c-a7ab-ac3e830c90dc)



Now in the tkcon editor:

```
Select A area

port class input
port use signal

Select Y area

port class output
port use signal

Select VPWR area

port class inout
port use power

Select VGND area

port class inout
port use ground

```


Thus now, the changes of ports into pins can be viewed on the lef file: Lef file has port to pin conversions.

![Screenshot from 2023-09-11 12-02-17](https://github.com/Sushma-Ravindra/Advanced_Physical_Design_using-_OpenLane/assets/141133883/9c8ea8ad-1696-4a40-9f62-b3e87794e297)


![Screenshot from 2023-09-11 12-03-05](https://github.com/Sushma-Ravindra/Advanced_Physical_Design_using-_OpenLane/assets/141133883/0d4d15af-87d4-42e0-a77d-e5bd761eee98)



__Steps to include new cell in synthesis__

To include custom cell into syntheis:
1) Copy file to picorv32a location
2)In the config.json file, make the following changes.

```

{
    "DESIGN_NAME": "picorv32",
    "VERILOG_FILES": "dir::src/picorv32a.v",
    "CLOCK_PORT": "clk",
    "CLOCK_NET": "clk",
    "GLB_RESIZER_TIMING_OPTIMIZATIONS": true,
    "RUN_HEURISTIC_DIODE_INSERTION": true,
    "DIODE_ON_PORTS": "in",
    "GPL_CELL_PADDING": 2,
    "DPL_CELL_PADDING": 2,
    "CLOCK_PERIOD": 24,
    "FP_CORE_UTIL": 35,
    "PL_RANDOM_GLB_PLACEMENT": 1,
    "PL_TARGET_DENSITY": 0.5,
    "FP_SIZING": "relative",
    "LIB_SYNTH":"dir::src/sky130_fd_sc_hd__typical.lib",
    "LIB_FASTEST":"dir::src/sky130_fd_sc_hd__fast.lib",
    "LIB_SLOWEST":"dir::src/sky130_fd_sc_hd__slow.lib",
    "LIB_TYPICAL":"dir::src/sky130_fd_sc_hd__typical.lib",
    "TEST_EXTERNAL_GLOB":"dir::/src/*",
    "SYNTH_DRIVING_CELL":"sky130_vsdinv",
    "MAX_FANOUT_CONSTRAINT": 4,
    "pdk::sky130*": {
        "MAX_FANOUT_CONSTRAINT": 6,
        "scl::sky130_fd_sc_ms": {
            "FP_CORE_UTIL": 30
        }
    }
}

```


3)Now run openlane


```
prep -design picorv32a
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs
run_synthesis

```

SYnthesis report:

Since the custom standard cell has been plugged into the openLANE flow, it would be visible in the layout.

![image](https://github.com/Sushma-Ravindra/Advanced_Physical_Design_using-_OpenLane/assets/141133883/bab891bf-70cc-4d79-b5ac-96017abee44c)


__Delay Tables__ 

 We encounter several types of delays in ASIC design. They are as follows:Gate delay or Intrinsic delay,Net delay or Interconnect delay or Wire delay or Extrinsic delay or Flight time, Transition or Slew,Propagation delay,Contamination delay. Wire delays or extrinsic delays are calculated using output drive strength, input capacitance and wire load models. Other delays are intrinsic properties of each and every gate.
Delays are interdependent on different electrical properties.Input capacitance of the logic gate is a function of output state, output loads and input slew rate, Internal timing arcs and output slew rate is a function of switching inputs, Capacitance of the wire is dependent on frequency.
Lets say two scenarios, we have long wire and the cell(X1) is sitting at the end of the wire : the delay of this cell will be different because of the bad transition that caused due to the resistance and capcitances on the long wire. we have the same cell sitting at the end of the short wire: the delay of this will be different since the transistion is not that bad comapred. Eventhough both are same cells, depending upon the input tran, the delay got changed. Same goes with o/p load also.


<img width="1440" alt="Screenshot 2023-09-15 at 6 47 15 PM" src="https://github.com/Sushma-Ravindra/Advanced_Physical_Design_using-_OpenLane/assets/141133883/9bc5a04d-9e26-456c-99de-1b99e6384d26">





__Fix slack__


In the synthesis for picrov32a with custom cell, timing analysis log is viewed and is as below:

![image](https://github.com/Sushma-Ravindra/Advanced_Physical_Design_using-_OpenLane/assets/141133883/41490bd6-7cde-4c19-a2b0-f705ac2e7b07)

Since clock tree synthesis has not been performed yet, the analysis is with respect to ideal clocks and only setup time slack is taken into consideration. The slack value is the difference between data required time and data arrival time. The worst slack value must be greater than or equal to zero. If a negative slack is obtained, following steps may be followed:

Change synthesis strategy, synthesis buffering and synthesis sizing values
Review maximum fanout of cells and replace cells with high fanout


![image](https://github.com/Sushma-Ravindra/Advanced_Physical_Design_using-_OpenLane/assets/141133883/ef0e5dff-9f78-40bb-9197-d910748de3a3)


We perform synthesis and found that it has positive slack and met timing constraints.


Now to check if our floorplan contains our custom cell,  check .lef file in the tmp folder. 

```
run_floorplan
run_placement
```


![image](https://github.com/Sushma-Ravindra/Advanced_Physical_Design_using-_OpenLane/assets/141133883/fe9f85df-e808-467b-bf66-9182ca7c8b32)

After placement, we check for legality &To check the layout invoke magic from the results/placement directory:

![image](https://github.com/Sushma-Ravindra/Advanced_Physical_Design_using-_OpenLane/assets/141133883/982f8694-38cf-43a3-96cb-2ac67ed854fe)

```
magic -T /home/sushma/.volare/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.nom.lef def read picorv32.def &

```
![image](https://github.com/Sushma-Ravindra/Advanced_Physical_Design_using-_OpenLane/assets/141133883/14e817ac-0d87-4fdc-98ff-b8fca1ada511)



</details>

<details>
	<summary>Timing Analysis using OpenSTA </summary>


__Introduction to setup time__

Setup time is the required time duration that the input data MUST be stable before the triggering-edge of the clock. If data is changing within this setup time window, the input data might be lost and not stored in the flip-flop as metastability might occur. What is metastability? When setup and hold time requirements are violated, the flip-flop state becomes unstable, and after an unpredictable duration, the state of the flip-flop can settle either way (1 or 0). This scenario is known as metastability. As shown in the following diagram, output Q1 passes through the slow logic and arrives late at the input D2 of FF2, which leads to setup time violation and the loss of the new data.
Thus combinational delay must be less than clock frequency - setup time

<img width="1440" alt="Screenshot 2023-09-15 at 8 55 51 PM" src="https://github.com/Sushma-Ravindra/Advanced_Physical_Design_using-_OpenLane/assets/141133883/a19d6b61-801e-4657-91e8-f91bd121aa6e">

__Clock Jitter__
Clock jitter is a characteristic of the clock source and the clock signal environment. It can be defined as “deviation of a clock edge from its ideal location.” Clock jitter is typically caused by clock generator circuitry, noise, power supply variations, interference from nearby circuitry etc. Jitter is a contributing factor to the design margin specified for timing closure. 

![image](https://github.com/Sushma-Ravindra/Advanced_Physical_Design_using-_OpenLane/assets/141133883/b31bfd2f-0b16-4ebe-85db-6be459e92112)


__Configuring OpenSTA tool__

Timing analysis is carried out outside the openLANE flow using OpenSTA tool. For this, pre_sta.conf is required to carry out the STA analysis. Invoke OpenSTA outside the openLANE

```
sta pre_sta.conf

```

Slack can be observed in opensta environment also.
 
</details>
<details>
	<summary>Clock Tree Synthesis </summary>


The purpose of building a clock tree is enable the clock input to reach every element and to ensure a zero clock skew. H-tree is a common methodology followed in CTS. Before attempting a CTS run in TritonCTS tool, if the slack was attempted to be reduced in previous run, the netlist may have gotten modified by cell replacement techniques. Therefore, the verilog file needs to be modified using the write_verilog command. In this stage clock is propagated and make sure that clock reaches each and every clock pin from clock source with mininimum skew and insertion delay. Inorder to do this, we implement H-tree using mid point strategy. 

![image](https://github.com/Sushma-Ravindra/Advanced_Physical_Design_using-_OpenLane/assets/141133883/d46c0a6e-c8ee-4906-98a0-3eccb57c376b)

Balanced Tree CTS: In a balanced tree CTS, the clock signal is distributed in a balanced manner, often resembling a binary tree structure. This approach aims to provide roughly equal path lengths to all clock sinks (flip-flops) to minimize clock skew. It's relatively straightforward to implement and analyze but may not be the most power-efficient solution.

H-tree CTS: An H-tree CTS uses a hierarchical tree structure, resembling the letter "H." It is particularly effective for distributing clock signals across large chip areas. The hierarchical structure can help reduce clock skew and optimize power consumption.

Star CTS: In a star CTS, the clock signal is distributed from a single central point (like a star) to all the flip-flops. This approach simplifies clock distribution and minimizes clock skew but may require a higher number of buffers near the source.

Global-Local CTS: Global-Local CTS is a hybrid approach that combines elements of both star and tree topologies. The global clock tree distributes the clock signal to major clock domains, while local trees within each domain further distribute the clock. This approach balances between global and local optimization, addressing both chip-wide and domain-specific clocking requirements.

Mesh CTS: In a mesh CTS, clock wires are arranged in a mesh-like grid pattern, and each flip-flop is connected to the nearest available clock wire. It is often used in highly regular and structured designs, such as memory arrays. Mesh CTS can offer a balance between simplicity and skew minimization.

Adaptive CTS: Adaptive CTS techniques adjust the clock tree structure dynamically based on the timing and congestion constraints of the design. This approach allows for greater flexibility and adaptability in meeting design goals but may be more complex to implement.


__CrossTalk__
Crosstalk is a disturbance caused by the electric or magnetic fields of one telecommunication signal affecting a signal in an adjacent circuit.
Essentially, every electrical signal has a varying electromagnetic field. Whenever these fields overlap, unwanted signals -- capacitive, conductive or inductive coupling -- cause electromagnetic interference (EMI) that can create crosstalk.
Overlap can occur with structured cabling, integrated circuit design, audio electronics and other connectivity systems. For example, if there are two wires in close proximity that are carrying different signals, their currents will create magnetic fields that induce a weaker signal in the neighboring wire.
Impact: Crosstalk is a significant concern in VLSI design due to the high integration density of components on a chip. Uncontrolled crosstalk can lead to data corruption, timing violations, and increased power consumption. Mitigation: VLSI designers employ various techniques to mitigate crosstalk, such as optimizing layout and routing, using appropriate shielding, implementing proper clock distribution strategies, and utilizing clock gating to reduce dynamic power consumption when logic is idle


__Clock Net Shielding__


Shielding is done so as to prevent gltch. 
Shields are connected to VDD or GND. The shields do not switch.VLSI designers may use shielding techniques to isolate the clock network from other signals, reducing the risk of interference. This can include dedicated clock routing layers, clock tree synthesis algorithms, and buffer insertion to manage clock distribution more effectively. Clock Domain Isolation: VLSI designs often have multiple clock domains. Shielding and proper clock gating help ensure that clock signals do not propagate between domains, avoiding metastability issues and maintaining synchronization.

![image](https://github.com/Sushma-Ravindra/Advanced_Physical_Design_using-_OpenLane/assets/141133883/9417f075-f068-497c-a39a-8a62a65bf351)




__Lab__
Before attempting to run CTS in TritonCTS tool, if the slack was attempted to be reduced in previous run, the netlist may have gotten modified by cell replacement techniques. Therefore, the verilog file needs to be modified using the write_verilog command. Then, the synthesis, floorplan and placement is run again. To run CTS use the below command:

```
run_cts
```

![image](https://github.com/Sushma-Ravindra/Advanced_Physical_Design_using-_OpenLane/assets/141133883/aff4f916-ee18-419b-a6c7-28805f8629dc)

BOth values are positive and hence there is no violation.




Since, clock is propagated, from this stage, we do timing analysis with real clocks. From now post cts analysis is performed by operoad within the openlane flow



 
</details>













## Acknowledgements


