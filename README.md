# Advanced_Physical_Design_using-_OpenLane


## Table of Contents

- [Day1-Introduction](#Day1-introduction)
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











</details>













## Acknowledgements


