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
![compilation1](https://github.com/IIITB-ARUL/RISCV/assets/140998631/60b58aef-e9c1-4008-a5a4-3871b7823521)
![spike1to9](https://github.com/IIITB-ARUL/RISCV/assets/140998631/372c9fd2-2728-4a36-91e8-10dc4cd40d93)


 
</details>




<details>
	<summary>
		Basic verification flow using iverilog
	</summary>

**Lab To Run C-Program On RISC-V CPU**

![labtorun con riscv](https://github.com/IIITB-ARUL/RISCV/assets/140998631/51a1a542-98eb-49c8-8f62-eed00176b8b6)

 Demo lab for the above shown flow,

```
cd ~/riscv_workshop_collaterals/labs/
chmod 777 rv32im.sh # this command gives the permission 
./rv32im.sh
```

The rv32im.sh is the shell script which runs the following command to convert the c program into hex file bitstream and simulate and load the hex file into picorv.32 using iverilog

![rvsim](https://github.com/IIITB-ARUL/RISCV/assets/140998631/0aa4ab39-29b8-4413-a39d-c2806f7e8fb3)

After running the shell script,

![op](https://github.com/IIITB-ARUL/RISCV/assets/140998631/f5ce1eee-f460-4040-a929-dbeb053d478c)


firmware.hex:

![hex](https://github.com/IIITB-ARUL/RISCV/assets/140998631/f2ca3205-66fb-4d62-8978-e4fe65928c0e)

firmware32.hex:

![32hex](https://github.com/IIITB-ARUL/RISCV/assets/140998631/d83ff03a-2bde-4402-bd7e-01b123d4ed60)

</details>

# Day 3- Digital Logic with TL-Verilog and Makerchip

<details>
	<summary>
		Introduction to logic gates
	</summary>

Logic gates are fundamental building blocks in digital circuitry and electronics. They are used to perform logical operations on input signals, which are typically binary (0 or 1). Logic gates are combined to create more complex circuits and systems, ultimately enabling the creation of digital devices like computers, smartphones, and other electronic device.

![logicgate](https://github.com/IIITB-ARUL/RISCV/assets/140998631/7a97a6c1-0b37-4894-8d45-88b7469597cd)

Here **NAND** and **NOR** are universal logic gates.Using these gates any kind of logic can be realized.

 
</details>

<details>
	<summary>
		Labs on combinational circuit
	</summary>


**Makerchip**

Makerchip" refers to an online Integrated Development Environment (IDE) that enables users to design, simulate, and verify digital circuits and systems.
The IDE allows users to simulate their designs, providing a way to test and debug their circuits before implementation.Users can visualize the behavior of their designs using waveform viewers, which display the changing signal values over time.
Makerchip  can be used for educational purposes to teach digital logic design and related concepts.

**Combinational circutis**

A combinational circuit is an electronic circuit that performs a specific logical operation based on its input values. It produces output(s) solely dependent on the current input values, without any memory or feedback. In other words, the output is a direct function of the input, and there is no internal state or memory elements like flip-flops.

**Inverter**

![Inverter](https://github.com/IIITB-ARUL/RISCV/assets/140998631/157d3697-1638-4fa2-8bac-fbc564bf5df4)

**Exor**

![EXOR](https://github.com/IIITB-ARUL/RISCV/assets/140998631/e0e7c859-0cc1-43e3-ace5-7d7e61e4b6bf)

**And**

![And](https://github.com/IIITB-ARUL/RISCV/assets/140998631/f59ff6e7-8a1b-49e6-a476-541b38e3dca6)

**2:1 Mux**

![mux](https://github.com/IIITB-ARUL/RISCV/assets/140998631/9fb25b5b-c6d4-47a3-8fdc-0d2bf581b732)

**Vectors**

![muxvectors](https://github.com/IIITB-ARUL/RISCV/assets/140998631/fe5edc03-700d-485f-a692-93704c6cd8c5)



**Calculator**


![calc1](https://github.com/IIITB-ARUL/RISCV/assets/140998631/8db11946-ddf3-44f7-846a-3721c6441e7d)



TL verilog code for the above shown calculator

```
   
$val1[31:0] = $rand1[3:0];
$val2[31:0] = $rand2[3:0];
   
$sum[31:0] = $val1 + $val2;
$diff[31:0] = $val1 - $val2;
$prod[31:0] = $val1 * $val2;
$quot[31:0] = $val1 / $val2;
   
$out[31:0] = $op[0] ? ( $op[1] ? $quot : $diff ) : ( $op[1] ? $prod : $sum ); 
```
![calc2](https://github.com/IIITB-ARUL/RISCV/assets/140998631/8d09b701-9e72-43c4-9c30-757aa404ef96)

</details>


<details>
	<summary>
		Labs on Sequential logic
	</summary>


A sequential circuit is an electronic circuit that uses memory elements to store information, allowing it to have an internal state and exhibit behavior that depends not only on the current inputs but also on the previous inputs and internal state. Unlike combinational circuits, which produce outputs based only on current inputs, sequential circuits have feedback loops and can maintain state over time.

![sequentialckt](https://github.com/IIITB-ARUL/RISCV/assets/140998631/02f2f1ba-06a2-496a-9d7d-b69833198d0f)


**Example - Fibonacci series**

Fibonacci series is a number is the sum of its previous two numbers, ie. 1,1,2,3,5,8,13,

 ![fibonacci 1](https://github.com/IIITB-ARUL/RISCV/assets/140998631/92a0a78d-bb24-45df-b159-013a008368df)


**Free running counter**


![counter1](https://github.com/IIITB-ARUL/RISCV/assets/140998631/a0a85087-f06a-4048-ac01-c31dfc2f1205)

The above chip is implemented using  makerchip.

![counter2](https://github.com/IIITB-ARUL/RISCV/assets/140998631/083f839c-167b-4635-bfce-1e2c6f8ad7c5)



**Sequential calculator**

![sequentialcounter](https://github.com/IIITB-ARUL/RISCV/assets/140998631/4e314cee-0dc5-40b7-858d-426a84336c52)


The above circuit is implemented in the Makerchip.

TL verilog code 

```
   $reset = *reset;

   $val1[31:0] = >>1$out[31:0];
   $val2[31:0] = $rand2[3:0];
   
   $sum[31:0] = $val1 + $val2;
   $diff[31:0] = $val1 - $val2;
   $prod[31:0] = $val1 * $val2;
   $quot[31:0] = $val1 / $val2;
   
   $out[31:0] = $reset ? 0 : ($op[0] ? ( $op[1] ? $quot : $diff ) : ( $op[1] ? $prod : $sum ));
   
```


 ![sequentialcalc](https://github.com/IIITB-ARUL/RISCV/assets/140998631/33d1e5e0-7dc7-4414-a899-fa80f2e176b1)


</details>


<details>
	<summary>
		Pipeling
	</summary>

Pipelining is a technique used in computer architecture to improve the overall throughput and efficiency of instruction processing. It involves breaking down the execution of instructions into a series of stages, where each stage performs a specific operation on an instruction. By overlapping the execution of multiple instructions in different stages, pipelining can increase the overall instruction throughput and reduce the time it takes to complete a sequence of instructions.

The stages in a typical instruction pipeline might include:

1.Instruction Fetch (IF): Fetches the instruction from memory.
    
2.Instruction Decode (ID): Decodes the fetched instruction to determine the necessary operations.
    
3.Execute (EX): Performs the actual computation or operation specified by the instruction.
    
4.Memory Access (MEM): If needed, accesses memory to read or write data.
    
5.Write Back (WB): Writes the results of the execution back to registers.




 
</details>



## References
- https://github.com/kunalg123
- https://github.com/stevehoover/RISC-V_MYTH_Workshop
- https://www.vsdiat.com/
- https://makerchip.com/
- https://github.com/riscv/riscv-gnu-toolchain
- https://github.com/alwinshaju08/RISCV
- https://github.com/mrdunker/RISC-V_based_MYTH_IIITB
- https://github.com/KanishR1
