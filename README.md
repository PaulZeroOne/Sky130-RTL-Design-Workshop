# Sky130-RTL-Design-Workshop
This is a doccumentation of the five day workshop on "RTL DESIGN USING VERILOG USING SKY130 TECHNOLOGY"
This repository includes the design and synthesis of RTL codes with the use of tools such as iverilog, yosys and gtkwave.

![github-small](https://github.com/srimathiramasamy/Sky130-RTL-Design-and-Synthesis/raw/main/Capture.PNG)

## Table of contents
[DAY 1](#DAY-1)
* [Interaction with Chipspirit founder - Mohan Jindal](#Chipspirit)
* [Lab Setup](#Lab-Setup)
* [Simulator -  iverilog](#Simulator-iverilog)
* [Waveform Viewer - gtkwave](#Waveform-Viewer-gtkwave)
* [Synthesis - yosys](#Synthesis-yosys)
* [Library Files](#Library-Files)

[DAY 2](#DAY-2)
* [Hierarchical and Flat Synthesis](#Hierarchical-And-Flat-Synthesis)
* [Flip Flop Coding Styles](#Flip-Flop-Coding-Styles)

## DAY 1
The first day of the RTL Design workshop covers the simaulation part of the RTL design along with a brief description about the usage of design tools such as iverilog, gtkwave and yosys. It also provides a detailed descriptive note on the SkyWater130 library used. 

### Chipspirit
Chipspirit has collaborated with Indian Navy to work on cutting edge security applications under the leadership of Mohan Jindal who has an extensive and rich experience in VLSI design, verification and implementation.

### Lab Setup
 
 * create a directory 'VLSI' in the home path. 
```
 mkdir VLSI/;cd VLSI/;ls
```
 
 * clone the sky130RTLDesignAndSynthesisWorksop repository from Github.
 ```
 git clone https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorksop.git
 ```
 
* files present in the directory

 
 ![github-small](https://github.com/PaulZeroOne/Sky130-RTL-Design-Workshop/blob/main/Images/view.PNG)
 
 ### Simulator iverilog
 RTL Design is the set of verilog codes which intends to meet the specifications. The testbench is the setup appied to the desing with a set of test vector to check working of the designed module.
 Simulator plays an important role in verifying the design by providing the different input stimulus to the design at different times to check the proper fuunctioning of the RTL code as mentioned in the specifications.  THere are various verilog simulators availabe such as:
* iverilog
* Modelsim
* Questa Sim
* Xilinx Simulator (XSIM)
* MPSim
* ISE Simulator

In this workshop, we have used *iverilog*, which is a free open source simulator.

#### Working
It mainly relays on the change in the input. If there are is no change in the input test vector then there will be no change in the output, the simulator will not eveluate the output in this condition.

* We can view and edit verilog files using vim editor as 
```
vim design.v 
```
 
 ##### Vim Editor Important Shortcuts 
| Shortcut  | Description |
| ------------- | ------------- |
| :  | enter command mode  |
| i  | enter insert mode  |
| exit  | exit file  |
|  wq | write and exit  |
|  G | end of buffer  |
|  gg | beignning of buffer  |

 
![github-small](https://github.com/PaulZeroOne/Sky130-RTL-Design-Workshop/blob/main/Images/tb_goodmux.PNG)
 
The primary inputs given to the testbench where the design is instantiated from the stimulus generator and the outputs are verified by the stimulus observer, the testbench doesnot have primary input and primary output
 
 #### Simulation Flow
 
The output of the simulator is a .vcd file where vcd stands for value change dump. The simulator looks for changes in the input testvector hence the format .vcd occurs.

![github-small](https://user-images.githubusercontent.com/89927660/131877209-116b4362-b56d-46ed-9f42-c7981a027158.png)

#### Syntax
  * source the iverilog tool and mention the design file and testbench file.
 ```
 iverilog design.v tb_design.
 ```
 * the .vcd file dumped can be viewed using the following command.
 ```
 ./a.out
 ```
![github-small](https://github.com/PaulZeroOne/Sky130-RTL-Design-Workshop/blob/main/Images/vcd.PNG)

A *design.vcd* file will be dumped, which is going to be used to loaded into gtkwave for visualization. Value Change Dump (VCD) is an ASCII-based format for dumpfiles generated by EDA logic simulation tools.

 
 ### Waveform Viewer gtkwave
 
 The tool for viewing the waveform is gtkwaveform viewer, where the window pops up containing the stimulus from where the functionality of the RTL design can be verified.
 
 * launch the tool.
 ```
 gtkwave design.vcd 
 ``` 
 
![github-small](https://github.com/PaulZeroOne/Sky130-RTL-Design-Workshop/blob/main/Images/gtkwave.PNG)

 
### Synthesis yosys

Synthesizer is the tool which converts the RTL to the netlist. In this flow we use yosys as the synthesizer.

![github-small](https://github.com/srimathiramasamy/Sky130-RTL-Design-and-Synthesis/blob/main/Y1.PNG)

The design along with the library file is given to the synthesizer tool and the netlist is generated from it.
Netlist is the representation of the design in terms of standard cell in the .lib, the design is made into gates and the connections are made into gates


commands -
* invoke yosys
 ```
 yosys
```
![github-small](https://github.com/PaulZeroOne/Sky130-RTL-Design-Workshop/blob/main/Images/yosys.PNG)
* read the library with the relative path
```
read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd_tt_025C_1v80.lib
```
* read the design (successfully done should come)
````
read_verilog designmodule.v
````
![github-small](https://github.com/PaulZeroOne/Sky130-RTL-Design-Workshop/blob/main/Images/yosysss.PNG)
* synth -top moduletosynthesis
````
synth -top modulename
````
![github-small](https://github.com/PaulZeroOne/Sky130-RTL-Design-Workshop/blob/main/Images/stats.PNG)
* generate the netlist (abc -converts the RTL to gate specified in the library)
````
abc -liberty ../my_lib/lib/sky130_fd_sc_hd_tt_025C_1v80.lib
````
* show - graphical version of the logic realized
```
show
```
![github-small](https://github.com/PaulZeroOne/Sky130-RTL-Design-Workshop/blob/main/Images/show.PNG)
* write_verilog - text version of the logic realized
```
write_verilog modulename_netlist.v
!gvim modulename_netlist.v
```
![github-small](https://github.com/PaulZeroOne/Sky130-RTL-Design-Workshop/blob/main/Images/netlist.PNG)
* exit
```
exit
```
To verify the synthesis output - provide the netlist along with the testbench to the iverilog (verilog simulator) and check the .vcd file with the gtkwaveform viewer to monitor the functionality of the design, the functionality should remain the same as the ouput RTL simulation.
The primary inputs and the ouput remains same thus the same testbench can be used.

![github-small](https://github.com/srimathiramasamy/Sky130-RTL-Design-and-Synthesis/blob/main/2.1.PNG)



### Library Files

.lib is the collection of all the standard cell. contains various versions of the same gates (slow, fast, medium speed). It will be rich enough to implement any logic function. The requirement of different flavors of gates is due to the combinational delay of the logic circuit determines the speed of the circuit.

![github-small](https://github.com/srimathiramasamy/Sky130-RTL-Design-and-Synthesis/blob/main/2.2.PNG)

tclk > the propagation of flop a + combinational delay + setup time of flop b
fclk=1/tclk

tclk is the minimum clk period needed and fclk will be the maximum clk frequency. For the maximum performance the delay should be minimum, this can be acheived by faster cells which eliminates the requirement of the medium and slow cells.

#### Naming Syntax

```sky130_fd_hd_sc__tt_025C_1v80.lib```
fd-foundary
tt - typical
025C-temperature
1v80-voltage
PVT - Process Voltage Temperature

#### Requirement of Slow cells 

To ensure the absence of hold related time issues in the flop b there is requirement of slow cells, where the delay should be minimum. Thus we require fast cells to meet the performance and slow cells to meet the hold. The load in the circuits is the capacitor, the faster the charging and the discharging the faster the circuit operates.
* wider transistors provide low delay - more area and power consumption
* narrow transistors provide more delay - less area and power consumption

more use of slow cells - sluggish performance
more use of fast cells - hold violations


## DAY 2
### Hierarchical and FLat Synthesis 

* submodule2

![github-small](https://github.com/srimathiramasamy/Sky130-RTL-Design-and-Synthesis/blob/main/31.PNG)
![github-small](https://github.com/srimathiramasamy/Sky130-RTL-Design-and-Synthesis/blob/main/40.PNG)
```
gedit multiple_module.v
```
submodule1 - and gate
submodule2 - or gate
multiple module -instantiation of submodules
* multiple module
![github-small](https://github.com/srimathiramasamy/Sky130-RTL-Design-and-Synthesis/blob/main/32.PNG)
![github-small](https://github.com/srimathiramasamy/Sky130-RTL-Design-and-Synthesis/blob/main/33.PNG)

```
yosys
read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd_tt_025C_1v80.lib
read_verilog multiple_module.v
synth -top multiple_module
abc -liberty ../my_lib/lib/sky130_fd_sc_hd_tt_025C_1v80.lib
show
exit
```
```
write_verilog -noattr multiple_module_hier.v
gedit multiple_module_hier.v
```
![github-small](https://github.com/srimathiramasamy/Sky130-RTL-Design-and-Synthesis/blob/main/34.PNG)
![github-small](https://github.com/srimathiramasamy/Sky130-RTL-Design-and-Synthesis/blob/main/35.PNG)

The hierachail desing is preserved.
![github-small](https://github.com/srimathiramasamy/Sky130-RTL-Design-and-Synthesis/blob/main/hie.PNG)
![github-small](https://github.com/srimathiramasamy/Sky130-RTL-Design-and-Synthesis/blob/main/37.PNG)
![github-small](https://github.com/srimathiramasamy/Sky130-RTL-Design-and-Synthesis/blob/main/38.PNG)
![github-small](https://github.com/srimathiramasamy/Sky130-RTL-Design-and-Synthesis/blob/main/39.PNG)


```
flaten
```

![github-small](https://github.com/srimathiramasamy/Sky130-RTL-Design-and-Synthesis/blob/main/36.PNG)


### Flip Flop Coding Styles
![github-small](https://github.com/srimathiramasamy/Sky130-RTL-Design-and-Synthesis/blob/main/3331.PNG)
![github-small](https://github.com/srimathiramasamy/Sky130-RTL-Design-and-Synthesis/blob/main/3.1.PNG)
![github-small](https://github.com/srimathiramasamy/Sky130-RTL-Design-and-Synthesis/blob/main/3.2.PNG)
![github-small](https://github.com/srimathiramasamy/Sky130-RTL-Design-and-Synthesis/blob/main/3.3.PNG)
![github-small](https://github.com/srimathiramasamy/Sky130-RTL-Design-and-Synthesis/blob/main/3.4.PNG)
![github-small](https://github.com/srimathiramasamy/Sky130-RTL-Design-and-Synthesis/blob/main/3.5.PNG)
![github-small](https://github.com/srimathiramasamy/Sky130-RTL-Design-and-Synthesis/blob/main/3.6.PNG)
![github-small](https://github.com/srimathiramasamy/Sky130-RTL-Design-and-Synthesis/blob/main/3.7.PNG)
