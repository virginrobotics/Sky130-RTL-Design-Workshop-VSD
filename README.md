![picture](https://github.com/virginrobotics/Sky130-RTL-Design-Workshop-VSD/blob/main/images/Verilog-flyer.png)

# Sky130-RTL-Design-Workshop-VSD
------------------------------------------------
This repository is a documentation of the 5 day workshop on RTL Design using iverilog and YOSYS for simulation and synthesis with Sky130 PDK. 
The workshop spanned 5 days from 26th April 2022 to 1 May 2022 with attendees provided remote access to machines with pre installed software and course modules over different topics along with assessments.

# Navigation
-------
* ## [ DAY 1 - Introduction to Verilog RTL Design and Synthesis]()
  * ### [Icarus Verilog - iverilog]()
  * ### [Simulation using iverilog and gtkwave]()
  * ### [Yosys and Logic Synthesis]()
  * ### [Yosys and Sky130 PDK]()


* ## [ DAY 2 - Timing Libs, Hirearchial vs Flat synthesis and efficient flop coding styles]()
  * ### [Timing .libs]()
  * ### [Hierarchial vs Flat Synthesis]()
  * ### [Flop coding styles and optimization]()


* ## [ DAY 3 - Combinational and Sequential Optimizations]()
  * ### [Introduction to Optimizations]()
  * ### [Combinational Logic Optimizations]()
  * ### [Sequential Logic Optimizations]()
  * ### [SLO unused outputs]()


* ## [ DAY 4 - GLS, Blocking vs Non-Blocking and Synthesis-Simulation mismatch]()
  * ### [Gate Level Simulation]()
  * ### [Synthesis Simulation Mismatch]()
  * ### [Blocking and Non-Blocking Statements]()
  * ### [Blocking Statements Caveats]()


* ## [ DAY 5 - If, Case, For loop and For Generate]()
  * ### [If Case constructs]()
  * ### [Incomplete Overlapping Case]()
  * ### [For Loop and For Generate]()
--------------

## Day 1 Inroduction to Verilog RTL Design and Synthesis

First day is about learning what verilog and RTL is, difference between design and synthesis, how they're tied together using testbenches, broad picture of YOSYS, iverilog, Netlists , Cell libraries etc.

### Simulation using iverilog and gtkwave 

Folder verilog_files has all the digital designs in verilog along with testbenches for the trainee to compile and run. Let's see how one uses iverilog to simulate and view waveforms of an upcounter ,called upcntr.v and it's testbench tb_upcntr.v

#### 1. Navigate to verilog_files
Use the command ``` ls ``` to view the contents of each directory and navigate to where you cloned the workshop repo.
![picture](https://github.com/virginrobotics/Sky130-RTL-Design-Workshop-VSD/blob/main/images/verilogfiles.png)

#### 2. See the verilog behind a module
Use your faviourite text editor to open upcntr.v and tb_upcntr.v , the source and testbench for the upcounter.

``` nano upcntr.v tb_upcntr.v ```

Use ``` alt + . ```to toggle between files.

> Behavioural Description

![picture](https://github.com/virginrobotics/Sky130-RTL-Design-Workshop-VSD/blob/main/images/upcntrnew.png)

> Testbench 

![picture](https://github.com/virginrobotics/Sky130-RTL-Design-Workshop-VSD/blob/main/images/tbupcntrnew.png)

#### 3. Compile verilog module and testbench
Following is the syntax for compiling the upcounter verilog module and it's testbench.

``` iverilog <module_name.v> <testbench_module_name.v> ```

Example:

``` iverilog upcntr.v tb_upcntr.v ```

The command creates a binary called 

> a.out 

We can specify our own name for the binary using

``` iverilog -o among_us.out upcntr.v tb_upcntr.v ```

![picture](https://github.com/virginrobotics/Sky130-RTL-Design-Workshop-VSD/blob/main/images/amongusout.png)

Let's see what's inside our binary file among_us.out

![picture](https://github.com/virginrobotics/Sky130-RTL-Design-Workshop-VSD/blob/main/images/binaryfile.png)

### 4. .vcd File and GTK wave

Execute the binary file created after compiling using

``` ./<binaryfilename>.out ```

In our case

``` ./among_us.out ```

The value change dump file will contain the simulation output, let's see what's inside.

> nano tb_upcntr.vcd

![picture](https://github.com/virginrobotics/Sky130-RTL-Design-Workshop-VSD/blob/main/images/vcdfile.png)

Open the .vcd file using gtkwave

``` gtkwave tb_upcntr.vcd ```

![picture](https://github.com/virginrobotics/Sky130-RTL-Design-Workshop-VSD/blob/main/images/gtkoutput.png)


### YOSYS and Logic synthesis

Yosys is an opensource framework for RTL synthesis tools. To realize your HDL in hardware , an important step is to convert that behavioural description of the design to a low level description like a #### Gate Level Netlist ####, which describes the design in terms of standard logic cells like AND, NOR, XOR, FLOPS from a device library. 

To syhtesize a gate level netlist, you would need a standard cell library, in our case the sky130 PDK and a verilog module. 

Following are the steps to synthesize a verilog module.

#### 1. Call YOSYS

Call the yosys tool 

``` yosys```

![picture](https://github.com/virginrobotics/Sky130-RTL-Design-Workshop-VSD/blob/main/images/yosys.png)

#### 2. Read the liberty file (Cell libraries)

> If synthesis is realizing HDL to a gate level netlist, how does one know what gates to use, what parameters for the gates like area, power , delay, number of inputs, outputs, risetime, leakage current and much more. It comes from a .lib file that follows the Liberty syntax ( Library Timing file ) that contains all the information of standard logic cells of a particular technology node with its timing model like setup time, hold time, cell delay , cell transition etc. The .lib file is usually provided by a third party vendor or the foundry itself, if it supports a standard cell library.

Command to import library.

``` read_liberty -lib <path to .lib file> ```

In our case

``` read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib ```

![picture](https://github.com/virginrobotics/Sky130-RTL-Design-Workshop-VSD/blob/main/images/readliberty.png)

#### 3. Read the verilog module

Read the verilog module you want to synthesise, lets use the module good_mux.v

``` read_verilog good_mux.v ```

![picture](https://github.com/virginrobotics/Sky130-RTL-Design-Workshop-VSD/blob/main/images/readverilog.png)

#### 4. Synthesize top level module - Default Synthesis script

Use the ```synth``` command to run the default synthesis command on specified verilog module.

``` synth -top <modulename> ```

Example 

``` synth -top good_mux ```

On running ```synth``` , the follwoing report is presented by YOSYS.

The ABC pass maps YOSYS's internal gate library to a target architecture, in our case the sky130 lib.

``` abc [option] [selection] ```

We use

``` abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib ```

![picture](https://github.com/virginrobotics/Sky130-RTL-Design-Workshop-VSD/blob/main/images/abcpass.png)

The report form the pass

![picture](https://github.com/virginrobotics/Sky130-RTL-Design-Workshop-VSD/blob/main/images/abcresult.png)

#### 6. Graphical representation of Netlist 

> Create a graphviz DOT file for the selected part of the design and compile it to a graphics file (usually SVG or PostScript).

Using the command ```show``` gives us 

![picture](https://github.com/virginrobotics/Sky130-RTL-Design-Workshop-VSD/blob/main/images/showterminal.png)


![picture](https://github.com/virginrobotics/Sky130-RTL-Design-Workshop-VSD/blob/main/images/showmux.png)

A standard cell ```sky130_fd_sc_hd__mux2_1``` is called with ports i0,i1,sel and y mapped suitably.

-----------

## DAY 2 - Timing Libs, Hierarchial vs Flat synthesis and efficient flop coding styles

### Timing Libs

We are using devices from the Skywater130nm PDK technology node for constructing our logic cells and the properties of the cells depend on the device parameters too. 
The cells come in different flavours to optimize application specific needs. 

The library name ```sky130_fd_sc_hd_tt_025C_1v80.lib``` has the following information encoded in it's name.

> fd = foundry,
> sc = standard cell,
> hd = high density,
> tt = typical corner,
> 025C = 25 degrees Celcius,
> 1v80 = 1.8 Volts

Different flavours of an and gate for example may have the following specs in terms of leakage power and dimensions among other.

![picture](https://github.com/virginrobotics/Sky130-RTL-Design-Workshop-VSD/blob/main/images/and3.png)

![picture](https://github.com/virginrobotics/Sky130-RTL-Design-Workshop-VSD/blob/main/images/and2.png)

![picture](https://github.com/virginrobotics/Sky130-RTL-Design-Workshop-VSD/blob/main/images/and1.png)


### Hierarchial vs FLat Synthesis

A Hierarchial architecture has submodules inside the verilog program, example ```multiple_modules.v``` in verilog_files.

![picture](https://github.com/virginrobotics/Sky130-RTL-Design-Workshop-VSD/blob/main/images/multiplemodules.png)

submodule 1 corresponds to an and module and submodule two is a or gate. 

Let's see what happens when we synthesize the verilog program

![picture](https://github.com/virginrobotics/Sky130-RTL-Design-Workshop-VSD/blob/main/images/multiplemodulessynth.png)

after technology mapping through ```abc```

we run ```show```

![picture](https://github.com/virginrobotics/Sky130-RTL-Design-Workshop-VSD/blob/main/images/showmultiplemodules.png)


### Flop coding styles and optimization
----------

## DAY 3 - Combinational and Sequential Optimization

### Introduction to optimization

### Combinational Logic Optimization

### Sequential Logic Optimization

### SLO unused outputs

-------------

## Day 4 GLS, Blocking vs Non Blocking and Synthesis-Simulation mismatch

### Gate Level Simulation

### Synthesis Simulation Mismatch

### Blocking and Non-Blocking Statements 

### Blocking statements caveats
![picture](https://github.com/virginrobotics/Sky130-RTL-Design-Workshop-VSD/blob/main/images/blockingcaveatgtkwave.png)

---------------

## Day 5 - If, Case, For loop and For Generate

### If case constructs
![picture](https://github.com/virginrobotics/Sky130-RTL-Design-Workshop-VSD/blob/main/images/incompifverilog.png)

![picture](https://github.com/virginrobotics/Sky130-RTL-Design-Workshop-VSD/blob/main/images/incompifsynth.png)

![picture](https://github.com/virginrobotics/Sky130-RTL-Design-Workshop-VSD/blob/main/images/showincompif.png)

![picture](https://github.com/virginrobotics/Sky130-RTL-Design-Workshop-VSD/blob/main/images/incompifgtkwave.png)

![picture](https://github.com/virginrobotics/Sky130-RTL-Design-Workshop-VSD/blob/main/images/incompif2synth.png)

![picture](https://github.com/virginrobotics/Sky130-RTL-Design-Workshop-VSD/blob/main/images/showincompif2.png)

![picture](https://github.com/virginrobotics/Sky130-RTL-Design-Workshop-VSD/blob/main/images/incompif2gtkwavebad.png)

### Incomplete Overlapping Constructs 

![picture](https://github.com/virginrobotics/Sky130-RTL-Design-Workshop-VSD/blob/main/images/compcaseverilog.png)

![picture](https://github.com/virginrobotics/Sky130-RTL-Design-Workshop-VSD/blob/main/images/compcasesynth.png)

![picture](https://github.com/virginrobotics/Sky130-RTL-Design-Workshop-VSD/blob/main/images/showcompcase.png)

![picture](https://github.com/virginrobotics/Sky130-RTL-Design-Workshop-VSD/blob/main/images/compcasegtkwave.png)


### For loop and For generate

![picture](https://github.com/virginrobotics/Sky130-RTL-Design-Workshop-VSD/blob/main/images/muxgenerateverilog.png)

![picture](https://github.com/virginrobotics/Sky130-RTL-Design-Workshop-VSD/blob/main/images/muxgeneratesynth.png)

![picture](https://github.com/virginrobotics/Sky130-RTL-Design-Workshop-VSD/blob/main/images/muxgenerate.png)

#### Ripple Carry Adder

![picture](https://github.com/virginrobotics/Sky130-RTL-Design-Workshop-VSD/blob/main/images/rcagenerate.png)

![picture](https://github.com/virginrobotics/Sky130-RTL-Design-Workshop-VSD/blob/main/images/rcasynth.png)

![picture](https://github.com/virginrobotics/Sky130-RTL-Design-Workshop-VSD/blob/main/images/showrca.png)

![picture](https://github.com/virginrobotics/Sky130-RTL-Design-Workshop-VSD/blob/main/images/rcagtkwave.png)

![picture](https://github.com/virginrobotics/Sky130-RTL-Design-Workshop-VSD/blob/main/images/rcaveriloggtkwave.png)

----------

## BIG THANKS TO 
* ### [Kunal Ghosh Co-Founder VSD](https://www.vlsisystemdesign.com/)
* ### [Shon Taware - Chipspirit]()
