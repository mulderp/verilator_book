# Getting started

## A Basic Verilog testbench

Before introducing Verilator let`s first describe a simple testbench with Verilog.

In a new file src/hello_tb.v enter the following:

```
module hello_tb;

  reg clk;

  parameter N_CYCLES = 20;
  parameter PULSE_WIDTH = 5;
  integer j;

  initial begin
   clk <= 0;
   for (j=0; j<N_CYCLES; j = j + 1)
      #PULSE_WIDTH clk <= ~clk;
  end

  always @(posedge clk) begin
    $display("tick");
  end

endmodule
```

You can compile and run this module with iverilog for example:

```
$ iverilog -o hello_tb src/hello_tb.v
$ vvp hello_tb
```

You should see the following output:

```
tick
tick
...
```

The system under test is very simple. Parts of this setup can be done with C++, but Verilator doesn't have the same ways to generate events. Clocks are driven in from the SystemC or C++ wrapper, or derived with logic from clocks from SystemC or C++.


To compile with SystemC and C++, we can use this CMakeLists.txt build definition:

```
cmake_minimum_required(VERSION 3.15)

project(counter_tb)
find_package(verilator HINTS $ENV{VERILATOR_ROOT})

include_directories("/opt/systemc/include")

add_executable(Vhello hello.cc)

verilate(Vhello SOURCES src/hello.v)
verilator_link_systemc(Vhello)
```

With the following Verilog:

```
module hello(input clk);

  integer i;

  always @(posedge clk) begin
    $display("tick");
  end

  initial begin
  for (i = 0; i < 20; i = i + 1)
    $display("tick0");
  $finish();
  end

endmodule
```

Last we need a C++ testbench:

```
#include "verilated.h"

#include <sysc/communication/sc_clock.h>
#include <sysc/kernel/sc_externs.h>

int sc_main(int argc, char ** argv) {

  Verilated::commandArgs(argc, argv);

  sc_core::sc_clock clk("clk", 10, 0.6, 3, true);

  Vhello* top = new Vhello();

  while (!Verilated::gotFinish()) { top->eval(); }

  delete top;

  exit(0);

}
```

Now compile all with:

```
  $ mkdir build
  $ cd build
  $ cmake -G Ninja ..
  $ ninja all
```

You should be able to run the example and see:

```
> ./Vhello

        SystemC 2.3.4_pub_rev_20191203-Accellera --- Aug 29 2020 13:00:14
        Copyright (c) 1996-2019 by all Contributors,
        ALL RIGHTS RESERVED

Info: (I804) /IEEE_Std_1666/deprecated:
    sc_clock(const char*, double, double, double, bool)
    is deprecated use a form that includes sc_time or
    sc_time_unit
tick
tick
...
tick
- src/hello.v:14: Verilog $finish
```

## A counter example

We can make the system under test a bit more interesting if we add a counter with an asynchronous reset.

```
module counter0 ( input   clk,
                  input   rstn,
                  input   en,
                  output  reg [3:0] out);

  always @ (posedge clk, negedge rstn) begin
    if (!rstn) begin
      out <= 0;
    end else begin
      if (en)
          out <= out + 1;
      else
         out <= out;
    end
  end
endmodule
```

To test this counter you can take the following testbench:

```
module counter_tb;

  reg clk;

  parameter N_CYCLES = 20;
  parameter PULSE_WIDTH = 5;

  integer j;

  initial begin
   clk <= 0;
   for (j=0; j<N_CYCLES; j = j + 1)
      #PULSE_WIDTH clk <= ~clk;
  end

  initial begin
    $dumpfile("waves.vcd");
    $dumpvars();

  end

  // counter setup
  reg counter_rstn_tb;
  reg counter_en_tb;
  wire [3:0] counter_n_tb;

  counter0 u0(
     .clk(clk),
     .rstn(counter_rstn_tb),
     .en(counter_en_tb),
     .out(counter_n_tb)
  );

 initial begin
     counter_en_tb <= 0;
     counter_rstn_tb <= 0;
     #4 counter_rstn_tb <= 1;
     #8 counter_en_tb <= 0;
     #10 counter_en_tb <= 1;
     #20 counter_en_tb <= 1;
  end

endmodule
```

When you elaborate the design and look at the waveforms you should be able to see waveforms like this:

[image](images/count_image.png)

## A first look at Verilator

Verilator will compile Verilog files into C++.  This means we can load the counter module into C++. To do this, you have to "verilate" the counter.

```
$ verilator -cc src/counter0.v 

```

```
$ tree obj_dir/
obj_dir/
├── Vcounter0_classes.mk
├── Vcounter0.cpp
├── Vcounter0.h
├── Vcounter0.mk
├── Vcounter0__Syms.cpp
├── Vcounter0__Syms.h
├── Vcounter0__ver.d
└── Vcounter0__verFiles.dat
```


The Verilog code can now be imported into a C++ testbench:

```
include            "Vour.h"#
include            "verilated.h"
int            main(int            argc,            char**            argv,            char**            env)            {
  Verilated::commandArgs(argc,            argv);
  Vour*            top            =            new            Vour;



{% code title="hello.sh" %}
```bash
# This will be a simple wrapper script.

```
{% endcode %}

## Testbench
The test bench provided for Verilator would need to be in C++, or a FSM in Verilog. 

## Compiling a design

Modules are the basic building blocks of a Verilog design. You can put multiple modules per file.

You'd write this in C code, for example in = 0; 

then make time pass and call 

{% code title="tb.cc" %}
```
topp->eval(), 
```
{% endcode %}
     
then print the value.


## Simulating a design



