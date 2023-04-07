# MCS-85+ Sample Code
This repository contains sample Intel 8085 code meant for the MCS-85+ single board computer [available from Bits of the Golden Age](https://www.tindie.com/products/helloworld/mcs-85-bare-board-v10-tin/). Detailed information can also be found in the [MCS-85+ User Manual](https://bitsofthegoldenage.org/download/mcs-85-users-manual/).

## MCS-85 Resident Monitor Requirements
All code herein is built for the `MCS-85_Monitor v0.0.7`. It may work for other version, but since this code is calling function in the monitor directly by address, if the bytecode layout changes ia a different version of the MCS-85 Monitor, this code might fail.

The current version of the MCS-85 Resident Monitor may be obtained at [the Bits of the Golden Age website](https://bitsofthegoldenage.org/download/mcs-85-resident-monitor/).

## Using BespokeASM
[BespokeASM](https://github.com/michaelkamprath/bespokeasm) is an assembler that can work with any instruction set as long as a configuration for that instruction set has been produced. A configuration for the Intel 8085 tailored for the MCS-85+ [is available in this repository](./bespokeasm/). The MCS-85+ monitor can accept programs via the serial connection using Intel Hex format. BespokeASM will ompile a program to the Intel Hex format suitable for copy & pasting into you terminal program with this command (as executded from the root directory of this repository):
```sh
bespokeasm compile -c bespokeasm/mcs-85-plus.yaml -p -t intel_hex -n -I common fibonacci.a85
```
Where `fibonacci.a85` is replaced by the program you wish to compile

Note that while all the 8085 mnemonics are identical with this BespokeASM confirguration, there are some differences with assembler directives from legacy 8085 assemblers. [These differences are documented here](https://github.com/michaelkamprath/bespokeasm/tree/main/examples/intel-8085#instruction-set). As a result, minor modifications of legacy 8085 assembly code is required in order to compile it with BespokeASM.

## MCS-85+ "Kernel"
A very lightweight kernel for creating program for the MCS-85+ is available in the [`common` directory](./common) of this repository. It provides 32-bit math and memory management functions, plus defines some constants for calling functions in the monitor, notably serial printing functions. To use this kernel, your program's code would start with the following:
```asm
#include "kernel.a85"

.org 0 "program"
start:
    ... your code here ...
```
This includes the kernel code found in the `common` directory (the path to which is included in teh compilation command with the `-I` option), then sets to the start of your program code to the start of the memory zone named "program" (see [BespokeASM documentation on memory zones](https://github.com/michaelkamprath/bespokeasm/wiki/Assembly-Language-Syntax#memory-zone-scope)). To run a program using this kernel, in the MCS-85+ Resident Monitor, first upload the Intel Hex representation of the program using the `A` monitor command, then start exectution at the `0x2800` address with the `G2800` monitor command.
