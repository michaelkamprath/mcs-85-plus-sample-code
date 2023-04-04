# MCS-85+ Configuration for BespokeASM
In order to assemble code provided in this repository, [the BespokeASM custom assembler](https://github.com/michaelkamprath/bespokeasm) is used with the `mcs-85-plus.yaml` configuration found in this directory. Assuming BespokeASM [is installed correctly](https://github.com/michaelkamprath/bespokeasm/wiki/Installation-and-Usage#installation), the command to copile code would be (if exectured fromthe root directory of this repository):
```sh
bespokeasm compile -c bespokeasm/mcs-85-plus.yaml -p -n -I common fibonacci.a85
```
Where `fibonacci.a85` is replaced with teh program you wish to compile. The above command will emit a human readable listing. To generate the Intel Hex version of the machine code suitable for uploading to the MCS-85+ via a terminal connection, add the following flag tot the command: `-t intel_hex`.
