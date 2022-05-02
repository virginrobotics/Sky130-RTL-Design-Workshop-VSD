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
![picture](https://github.com/virginrobotics/Sky130-RTL-Design-Workshop-VSD/blob/main/images/verilogfiles.png)
