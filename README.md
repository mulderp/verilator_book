# Verilator overview

## Welcome

Verilator is a tool to simulate and design digital circuits.

```
$ verilator
Usage:
        verilator --help
        verilator --version
        verilator --cc [options] [source_files.v]... [opt_c_files.cpp/c/cc/a/o/so]
        verilator --sc [options] [source_files.v]... [opt_c_files.cpp/c/cc/a/o/so]
        verilator --lint-only -Wall [source_files.v]...
```

{% hint style="info" %}
 Verilator runs on Unix and with Mingsys on Windows. 
{% endhint %}

Verilator will compile verilog files into C++ .

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



