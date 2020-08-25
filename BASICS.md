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

## A first look at Verilator

```
$ verilator
Usage:
        verilator --help
        verilator --version
        verilator --cc [options] [source_files.v]... [opt_c_files.cpp/c/cc/a/o/so]
        verilator --sc [options] [source_files.v]... [opt_c_files.cpp/c/cc/a/o/so]
        verilator --lint-only -Wall [source_files.v]...
```


Verilator will compile Verilog files into C++.

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



