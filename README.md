# RISCV

## Day 1 -Introduction to RISC-V ISA and GNU compiler toolchain

<details>
<summary>
Introduction to RISC-V basic keywords
</summary>


**Introduction:**

The "RISC" in RISC-V stands for Reduced Instruction Set Computer.It's designed to be simple, modular, and extensible, allowing for flexibility in implementing various types of processors, from microcontrollers to high-performance CPUs.It should accommodate all implementation technologies: Field-Programmable Gate
Arrays (**FPGAs**), Application-Specific Integrated Circuits (**ASICs**), full-custom chips,
and even future device technologies.It should be safe so the base ISA cannot be changed.

Function:
  Lets say if a c program has to run on a particular hardware or an interior chip, we have to pass the code to the layout which has certain flow.Firstly the c program is compiled to Assembly language(RISCV - set of instructions consisting hexadecimal numbers).This assembly language is converted into Machine language which is binary language program.
  The interface between RISCV and hardware is Hardware Description language.RTL implements the specifications of RISC.

  The flow for the above description:

  ![Riscv4](https://github.com/IIITB-ARUL/RISCV/assets/140998631/5930956e-2820-4714-87b7-227a1c0bdd66)
![rtl sinipptet](https://github.com/IIITB-ARUL/RISCV/assets/140998631/c802a949-4b67-43b3-bd3f-7fec674410e1)


 The operating system also takes a particular application and convert it into respective assembly language and the convert it to the binary.  

The instructions in RISCV are,

  > Pseudo instructions,
  > Base integer instruction (RV64I, RV32I),
  > Multiply extension (RV64M),
  > Single and double floating point instruction (RV64F, RV64D),
  > Application binary instruction,
  > Memory allocation and stack pointer.


</details>
<details>
  <summary>
  Labwork for RISC-V software toolchain
  </summary>



**Installation of RISCV tools**

Steps to Install

```
sudo apt-get install libboost-regex-dev
```
Spike RISCV ISA Simulator

```
git clone https://github.com/riscv-software-src/riscv-isa-sim
cd riscv-isa-sim
apt-get install device-tree-compiler
mkdir build
cd build
../configure --prefix=$RISCV
make
[sudo] make install
```

RISC-V Proxy Kernel and Boot Loader

```
git clone https://github.com/riscv-software-src/riscv-pk
cd riscv-pk
mkdir build
cd build
../configure --prefix=$RISCV --host=riscv64-unknown-elf
make
sudo make install
 ```





**GNU Compiler Toolchain**


   



Lets start with compiling a c program of Summing 1 t0 n (sum1ton),

Code


```
   
#include <stdio.h>

int main () 
{
	int i,sum = 0, n = 5;
	for (i = 1; i <=n; ++i) 
 	{
		sum += i;
	}
	printf("The sum of the number from 1 to %d is %d\n", n,sum);
	return 0;
}
 ```


The command for RISCV compilation,

```
    riscv64-unknown-elf-gcc <compiler option -O1 ; -Ofast> <ABI specifier -lp64; -lp32; -ilp32> <architecture specifier -RV64 ; RV32> -o <object filename> <C      filename>
```





Here -01 gives 15 instructions set while -0fast gives us 12 instructions set.


More details on compiler options can be obtained here

To view assembly code use the below command,


```
    riscv64-unknown-elf-objdump -d <object filename>
```

To see the executed file in text file format,

 
To use SPIKE simualtor to run risc-v obj file use the below command,

```
 riscv64-unknown-elf-objdump -d <object filename> | less
```


After RISCV compilation  the assembly code is shown in below image,

![Assembly](https://github.com/IIITB-ARUL/RISCV/assets/140998631/54c061f5-ba2d-4882-906b-213cd019b05a)

**Spike simulation and debugging**

To execute the assembly code,

```
    spike pk <object filename>
```


    
To use SPIKE as debugger

```
    spike -d pk <object Filename> with degub command as until pc 0 <pc of your choice>
```


To make program counter run manually from a particular instruction 

```
 until pc 0 <address>
```
To look into the content of the register

```
reg 0 <register>
```

> press enter to run next instruction 

![spike](https://github.com/IIITB-ARUL/RISCV/assets/140998631/0d475ebe-e00c-454e-be0a-b2a3c56fa3f4)


We can clearly see the content of register is changed after the execution of the instruction associated with that register.
  
</details>
