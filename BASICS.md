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

This module only drives a clock signal which can be used to test a digital system. You can compile and run this module with iverilog for example:

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

The system under test is very simple as our main focus at this stage is to study how parts of this setup can be written with C++.


## Building Systems with Verilator

The example above uses the square symbol (\#) to implement pulses. Verilog code that shall be translated to C++ (or "verilated" code) would not be able to process these delays in Verilog. Therefore let's remove the clock generator from Verilog.  It will be addressed later in C++.

To make the system under test a bit more interesting, we can add a counting action: To track the variables with digital waveforms, the Verilog dumpfile and dumpvar tasks are inserted. The resulting module is:

```
module hello(input clk, output [7:0] y);

  reg [7:0] n;

  initial begin
    $dumpfile("waves.vcd");
    $dumpvars();
    n = 8'h0;
  end

  always @(posedge clk) begin
    $display("tick");
    n <= n + 1;
  end

  assign y = n;

endmodule
```

The next step is to setup the C++ code that drives this device under test (or DUT).

In Verilator, clocks can be generated with C++ or with SystemC. One possiblity to drive the module is as follows:

```
#include "verilated.h"
#include "Vhello.h"

void wait_n_cycles(int n);

// Keep track of simulation time (64-bit unsigned)
vluint64_t main_time = 0;

// Called by $time in Verilog an needed to trace waveforms
double sc_time_stamp() {
    return main_time;  // Note does conversion to real, to match SystemC
}

int main(int argc, char ** argv) {

  Verilated::commandArgs(argc, argv);

  Verilated::traceEverOn(true);

  Vhello* top = new Vhello();

  // Simulate 20 clock cycles
  for (int n = 0; n < 20; n++) {
    top->clk = 1;
    wait_n_cycles(1000);
    top->eval(); 

    top->clk = 0;
    wait_n_cycles(1000);
    top->eval(); 

    main_time++;  
  }

  delete top;

  exit(0);

}

void wait_n_cycles(int n) {
  for (int i = 0; i < n; i++) { 
    main_time++;
  }
}

```


To compile with SystemC and C++, we can use CMake with this CMakeLists.txt build definition:

```
cmake_minimum_required(VERSION 3.15)

project(counter_tb)
find_package(verilator HINTS $ENV{VERILATOR_ROOT})

add_executable(Vhello hello.cc)

verilate(Vhello SOURCES src/hello.v)
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
tick
tick
...
tick
- src/hello.v:14: Verilog $finish
```

Besides the console output of clock ticks, you should have a waveform file which can be loaded with gitkwave:

```
$ gtkwave waves.vcd
```

You should be able to see 20 clock cycles and the resulting count value.

## A counter varation

We can make the system under test a bit more interesting if we add an asynchronous reset.

```
module counter(input clk, 
             input resetn,
             output [7:0] y);

  reg [7:0] n;

  always @(posedge clk) begin
    if (resetn == 1'b0)
      n <= 0;
    else
      n <= n + 1;

    if (n == 20)
      $finish();
  end

  assign y = n;




endmodule

```

Also, we can finish the simulation after we have reach a certain count value.



This reset could be driven from the testbench as follows

```
#include "verilated.h"
#include "Vhello.h"

void wait_n_cycles(int n);

// Current simulation time (64-bit unsigned)
vluint64_t main_time = 0;
// Called by $time in Verilog
double sc_time_stamp() {
    return main_time;  // Note does conversion to real, to match SystemC
}

int main(int argc, char ** argv) {

  Verilated::commandArgs(argc, argv);

  Verilated::traceEverOn(true);

  Vhello* top = new Vhello();

  top->resetn = 0;

  while (!Verilated::gotFinish()) { 

    // release reset
    if (main_time > 15) {
      top->resetn = 1;
    }


    top->clk = 1;
    wait_n_cycles(1000);
    top->eval(); 

    top->clk = 0;
    wait_n_cycles(1000);
    top->eval(); 

  }

  delete top;

  exit(0);

}

void wait_n_cycles(int n) {
  for (int i = 0; i < n; i++) { 
    main_time++;
  
  }
}
```
