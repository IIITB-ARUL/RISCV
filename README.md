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
![signed1](https://github.com/IIITB-ARUL/RISCV/assets/140998631/2a55257d-bb89-4171-a8b2-cdd3bfb62f52)

</details>




## Day 2 - Introduction to ABI and basic verification flow

<details>
	<summary>
		Application Binary interface (ABI)
	</summary>

An Application Binary Interface (ABI) is a set of conventions and rules that dictate how different software components interact at the binary level. The ABI defines how function calls are made, how parameters are passed to functions, and how return values are retrieved. This includes aspects such as the order in which parameters are passed, the use of registers and the stack, and how the stack is managed during function calls.It defines how user-level applications interact with the operating system through system calls and other APIs.When software developers create compilers, libraries, and system software, they need to adhere to the ABI to ensure that their code can work seamlessly with other components in the system.



![ABI levels2](https://github.com/IIITB-ARUL/RISCV/assets/140998631/c8b22c54-4c2a-424b-a9ed-2aac3b5ff7fd)

**Memory allocation for double words**

**Registers**

In computer architecture and assembly language programming, registers are small, fast storage locations within the CPU (Central Processing Unit) that are used to hold data that is being actively operated on. These registers are an integral part of the processor and play a crucial role in the execution of instructions and data manipulation

Lets assume XLEN is 64 bit as we are going to with RV64 throughout this course.There are two ways to load data in the registers one is directly and the othter is loaded from the memory.Memory are byte addressable.

![memory'](https://github.com/IIITB-ARUL/RISCV/assets/140998631/ce4d1849-73b4-41ef-8e28-1eb9fc7374f2)

In the above image the least significant byte is stored at the bottom of memory and the most significant byte is stored in the top.This structure of memory addressing is known as **Little endian memory addressing system**.The RISCV architecture follows this memory addressing system.

There is another memory addressing system **Big endian** which is the reverse of little endian.


**Load,Add And Store Instructions**



Now lets explore some instructions,

```
ld x8, 16(x23)
```

here ld means **load doubleword** which loads the data from the memory **(address: 16(offest or immediate)+ x23(content of source register)** into x8 **destination register**.

The 32 bit representation of the instruction

![instruction ](https://github.com/IIITB-ARUL/RISCV/assets/140998631/f8d9812d-e34d-4531-8045-92bdd6cd03b4)



```
add x8, x24,x8
```

here **add** means addition operation of operands **x24,x8 (source registers)** and stored into destination register **x8**.

The 32 bit representation of the instruction

![add](https://github.com/IIITB-ARUL/RISCV/assets/140998631/2db55c90-c9c4-4fea-a055-fbf660e61fdd)


```
sd x8, 8(x23)
```
here sd means **store doubleword** which stores back the **content of register x8**  into memory **(address: 8(offest or immediate)+ x23(content of source register)**

The 32 bit representation of the instruction


![sd](https://github.com/IIITB-ARUL/RISCV/assets/140998631/2e37b991-691a-452f-9793-fdcf4ad28322)





 Now these instructions operate on the integers so these are called **Base Integer Instructions-RV64I**.There are totally 47 base integer instructions out which we have seen only three.

 This RV64I is divided into three types:

 **I-type**: Instructions operate on destination registers and immediate.

 **R-type**: Instructions operate only on registers.
 
 **S-type**: Instructions operatae on immediate and source registers.

 

You can see from these images that 5 bits are used to represent registers.Because RISCV has only 32 registers(x0 to x31).Application Binary interface (ABI) system calls through these registers by some internal names assigned by RISCV which are listed below

![32reg](https://github.com/IIITB-ARUL/RISCV/assets/140998631/f80f3dcb-75d0-431a-a495-9c9c4463b6c9)


</details>
<details>
	<summary>
	 Lab work using ABI function calls
	</summary>


 In this lab we are going to modify the c prgram and make function call through c program to; assembly language program.i.e.,arguments are passed through registers and after execution the return value is retrieved through registers.


Lets take a simple Assembly language program of consecutive sum of 1 to 9 by using C function call,
 

 The **Algorithm** for the same is shown below

![algo](https://github.com/IIITB-ARUL/RISCV/assets/140998631/7c92cb78-c39e-4c9c-a225-cb93f55881d1)

C Code

```
#include <stdio.h>
extern int load(int x,int y);
int main()
 {
 	int result = 0;
	int count =9;
 	result = load(0x0,count+1);
 	printf("Sum of numbers from 1 to %d is %d\n",count,result);
 }
```
Assembly language Code 

```
.section .text
.global load
.type load, @function

load: 
     add   a4,a0,zero    //initialize sum register a4 with 0x0
     add   a2,a0,a1      //store count of 10 in reg a. reg a1 is loaded with 0xa(decimal 10) from main
     add   a3,a0,zero    //initialize intermediate sum reg a3 by 0x0

loop:
 add   a4,a3,a4     // Incremental addition
     addi  a3,a3,1      // Increment intermediate register by 1
     blt   a3,a2,loop   // If a3 is less than a2,branch to label <loop> 
     add   a0,a4,zero   // store final result to reg a0 so that it can be read by main pgm
     ret
```
The steps to simulate the above written code,


```
riscv64-unknown-elf-gcc -Ofast -mabi=lp64 -march=rv64i -o custom1_to9.o custom1_to_9.c load.S
spike pk custom1_to9.o
riscv64-unknown-elf-objdump -d custom1_to9.o | less
```


 
</details>
