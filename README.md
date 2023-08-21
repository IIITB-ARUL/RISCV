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




<details>
	<summary>
		Integer number representation 
	</summary>
This section suummarizes the representation of integer number in the processor.Humans understands the decimal numbers where as computers understand binary numbers.The interface between the conversion has to be addressed.By undrstanding this we come to know how the data instruction is getting arranged in the memory.

![number representation ](https://github.com/IIITB-ARUL/RISCV/assets/140998631/449e414d-39f7-4c30-817e-ad00f33a391d)



**Unsigned integer**

Unsigned integers are a type of data representation used in computer programming to store whole numbers that are non-negative (i.e., greater than or equal to zero).

The largest value you can represent with a 64-bit unsigned integer is 2^64 - 1, which is equal to 18,446,744,073,709,551,615. This is because you have 64 bits, and when all bits are set to 1, you get the maximum possible value.


**Signed integer**
A signed integer is a data type used in computer programming to represent whole numbers that can include both positive and negative values. 

Sign Bit (Most Significant Bit, MSB): The leftmost bit.

 Magnitude Bits: The remaining 63 bits (from bit 1 to bit 63)

Minimum Value: The smallest representable value is -2^63, which is -9,223,372,036,854,775,808.

Maximum Value: The largest representable value is 2^63 - 1, which is 9,223,372,036,854,775,807.

**Lab on unsigned integer**

Code

```
#include <stdio.h>
#include <math.h>
int main() {
unsigned long long int max = (unsigned long long int) (pow(2,64) -1);
printf("highest number represented by unsigned long long int is %llu\n", max);
return 0;
}

```
![unsigned](https://github.com/IIITB-ARUL/RISCV/assets/140998631/f046a7ae-cda9-400c-8a58-657686c0ca4e)


**Lab on signed integer**

Code

```
#include <stdio.h>
#include <math.h>
int main()
{
    long long int max = (long long int)(pow(2,63)-1);        
    long long int min = (long long int)(pow(2,63) * -1);     
    printf("highest number represented by long long int is %lld\n",max);
    printf("lowest number represented by long long int is %lld\n",min);
    return 0;
}
```
![signed](https://github.com/IIITB-ARUL/RISCV/assets/140998631/897acd31-bc0a-405a-9dc0-74dc6a43e1c0)


</details>




## Day 2 - Introduction to ABI and basic verification flow

<details>
	<summary>
		Application Binary interface (ABI)
	</summary>




 
</details>
