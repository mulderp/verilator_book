# Verilator overview

The aim of this book is to explore Verilator to learn and experiment with digital circuits. This book can also be read online at [gitbooks](https://mulder-patrick.gitbook.io/verilator/).

Verilator is a compiler that let you compile Verilog code into C++ to do simulations.
Using C++ can be helpful to introduce new abstractions and speed up simulation time.

Good places to start learning Verilator are also:
* [Verilator manual](https://www.veripool.org/wiki/verilator/Manual-verilator)
* [Verilator reference list](https://www.veripool.org/projects/verilator/wiki/Documentation)
* [UART example for ZipCPU](https://github.com/ZipCPU/wbuart32) 
* Also, the built-in help of Verilator with "verilator --help" can be a quick way to find what you need

## Verilator repository

The Verilator repository can be found [here](https://github.com/verilator/verilator). There is an examples folder and the issue list is a good way to report problems and ask questions.

{% hint style="info" %}
 Verilator runs on Linux, and with Mingsys on Windows. 
{% endhint %}

## Notes on this Gitbook

This book is Work in Progress. Feel free to add ideas and feedback by creating an issue on Github. 


## Learning Verilog

While Verilator let you write models in C++, you also need to know how to use Verilog.


### Verilog courses and tutorials

Here are some places where you can learn about Verilog such as:

* [Hardware Description Languages for FPGA Design](https://www.coursera.org/learn/fpga-hardware-description-languages) - This course introduces the languages, discusses a number of interesting code examples, and let you simulate these 
* [ChipVerify Verilog webpage](https://www.chipverify.com/verilog/)
* [ASIC world Verilog webpages](http://www.asic-world.com/verilog/veritut.html)
* [OpenCores](https://opencores.org/)

Also, the [Icarus Verilog](http://iverilog.icarus.com/) simulator is a good simulator to learn Verilog and it is free. In the Coursera course Hardware Description Languages for FPGA Design, the [Mentor Graphics ModelSim](https://www.mentor.com/company/higher_ed/modelsim-student-edition) simulator is used which is a tool also used by many companies.


### VHDL vs Verilog

Besides Verilog, VHDL can be useful to know when designing digital systems. We will not look into VHDL here, but if you are curious here are some places:

* [LED Displays Course](https://www.coursera.org/learn/enseignes-et-afficheurs-led/home/info) - Basic introduction to electronics and digital systems

