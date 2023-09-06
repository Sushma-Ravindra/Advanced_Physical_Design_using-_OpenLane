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




  </details>


  </details>
  <details>
  <summary>Opensource EDA toosl</summary>

__OpenLane Directory Structure__





  

  </details>



## Acknowledgements


