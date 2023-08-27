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
		Pipelining
	</summary>

Pipelining is a technique used in computer architecture to improve the overall throughput and efficiency of instruction processing. It involves breaking down the execution of instructions into a series of stages, where each stage performs a specific operation on an instruction. By overlapping the execution of multiple instructions in different stages, pipelining can increase the overall instruction throughput and reduce the time it takes to complete a sequence of instructions.

The stages in a typical instruction pipeline might include:

1.Instruction Fetch (IF): Fetches the instruction from memory.
    
2.Instruction Decode (ID): Decodes the fetched instruction to determine the necessary operations.
    
3.Execute (EX): Performs the actual computation or operation specified by the instruction.
    
4.Memory Access (MEM): If needed, accesses memory to read or write data.
    
5.Write Back (WB): Writes the results of the execution back to registers.


![pipe1](https://github.com/IIITB-ARUL/RISCV/assets/140998631/be0ed871-af45-4989-9f6d-945b950dfa16)


![pipe2](https://github.com/IIITB-ARUL/RISCV/assets/140998631/0fa9f70e-c94b-4a8a-88b6-afb67ca21831)




![pipe4](https://github.com/IIITB-ARUL/RISCV/assets/140998631/39b5dcbe-a9be-498a-87c0-289d42f7fa9c)


The stage of operation of the function can be modified without impacting the behaviour of that function.Only the timing in  the implementation may vary but it helps us to get a high throughput.



![pipe5](https://github.com/IIITB-ARUL/RISCV/assets/140998631/5cd79ce2-9e15-4931-913c-1f7347ef95cb)

We can clearly see from the above image that **TL verilog** prominently helps us in code reduction.


 **Pipelined Pythogorean implementaion**


![pipelined2](https://github.com/IIITB-ARUL/RISCV/assets/140998631/8f496b36-0f2f-407f-8145-9c7100f0db8e)



![identifiers](https://github.com/IIITB-ARUL/RISCV/assets/140998631/e94c4aac-f03c-4edb-b2a2-fd797c8b4aa6)


**Fibonacci series in pipeline**

![fibopipe](https://github.com/IIITB-ARUL/RISCV/assets/140998631/92b70257-5999-45ca-84b8-1a9b5f2e82bf)

Makerchip implementation of the same

![makerchipfibo](https://github.com/IIITB-ARUL/RISCV/assets/140998631/071a59d9-dee2-4ce3-8033-68fb67d55a3c)



**Implementation of given pipelined structure**

![givenpippe](https://github.com/IIITB-ARUL/RISCV/assets/140998631/8fc74590-c73f-4bde-9b79-a9085cb2f26c)

**Lab 1 : Calculator**

![lab1calc](https://github.com/IIITB-ARUL/RISCV/assets/140998631/d3984008-a95b-43d3-aada-86d5d30842a4)

Implementation in makerchip

TL verilog code

```
@1
         $reset = *reset;
         $val1[31:0] = >>1$out[31:0];
         $val2[31:0] = $rand2[3:0];
         $sum[31:0] = $val1 + $val2;
         $diff[31:0] = $val1 - $val2;
         $prod[31:0] = $val1 * $val2;
         $quot[31:0] = $val1 / $val2;
         $out[31:0] = $reset ? 0 : ($op[0] ? ( $op[1] ? $quot : $diff ) : ( $op[1] ? $prod : $sum ));
         
         $count[31:0] =$reset ? 0 : (>>1$count + 1);
```

![lab1calc](https://github.com/IIITB-ARUL/RISCV/assets/140998631/a6c66f6a-5569-404c-b992-249cbe019e8d)


**Lab 2 : 2 Stage Calculator**

 ![lab2calc](https://github.com/IIITB-ARUL/RISCV/assets/140998631/a08b6ab9-e047-4905-af6b-dd1f02f73234)

 Implementation in makerchip

TL verilog code

```
|calc
      @1
         $reset = *reset;
         $val1[31:0] = >>1$out[3:0];
         $val2[31:0] = $rand2[3:0];
         $sum[31:0] = $val1 + $val2;
         $diff[31:0] = $val1 - $val2;
         $prod[31:0] = $val1 * $val2;
         $quot[31:0] = $val1 / $val2;
         $valid[31:0] = $reset ? 0 : (>>1$valid + 1);
         
      @2
         $out[31:0] = ($reset | ~($valid)) ? 0 : ($op[0] ? ( $op[1] ? $quot : $diff ) : ( $op[1] ? $prod : $sum ));
         
```

 ![lab2calc](https://github.com/IIITB-ARUL/RISCV/assets/140998631/7bd47505-41c5-4094-8ec8-741aa19da9de)




</details>

<details>
	<summary>
		Valitidy
	</summary>


  In Transaction-Level Verilog (TL-Verilog), which is an extension of the Verilog hardware description language (HDL), "validity" refers to the concept of indicating whether a piece of data is valid or not. TL-Verilog is designed to facilitate high-level modeling and rapid design entry, particularly for transaction-level modeling.

  ![validity](https://github.com/IIITB-ARUL/RISCV/assets/140998631/dcc2e5b4-0d65-4f02-a7cc-6723d9ad36e8)





**Clock gating**

Clock gating is a power-saving technique used in digital circuit design to reduce dynamic power consumption by selectively controlling the clock signal to specific circuit elements or modules. The primary goal of clock gating is to save power by stopping the clock signal from reaching parts of the circuit that are not currently active or performing useful computation

The basic idea behind clock gating is to insert logic gates (typically AND or OR gates) between the clock source and the destination registers or logic elements. These gates act as switches that allow the clock signal to pass through only when a certain condition is met. If the condition is not satisfied, the clock signal is effectively "gated" or blocked from reaching the destination, preventing unnecessary clock cycles and power consumption.


  However, clock gating isn't always straightforward. If implemented incorrectly, it can introduce additional delay into the circuit, impacting performance. Additionally, managing clock domains and ensuring proper synchronization between clock-gated and non-clock-gated regions can be complex.


 ![validity2](https://github.com/IIITB-ARUL/RISCV/assets/140998631/de436a30-cd38-4dc3-8b98-ad59a227a1e2)





  **Lab Distance calculator**


TL veriog code


  ```
    |calc
      @1
         $reset = *reset;
      ?$valid
         @1
            $aa_sq[31:0] = $aa[3:0] * $aa;
            $bb_sq[31:0] = $bb[3:0] * $bb;
         @2
            $cc_sq[31:0] = $aa_sq + $bb_sq;
         @3
            $out[31:0] = sqrt($cc_sq);
       @4
          $tot_dist[31:0] = $reset ? '0 : ($valid ? (>>1$tot_dist + $out) : $RETAIN);

```

![distance](https://github.com/IIITB-ARUL/RISCV/assets/140998631/3bfe12e6-2166-41fa-a623-825fb842a017)




**Lab 2Cycle calculator**

![2cyle](https://github.com/IIITB-ARUL/RISCV/assets/140998631/30d3c537-4bdd-4a9e-a617-f1e2a1b03207)

![2cycle1](https://github.com/IIITB-ARUL/RISCV/assets/140998631/ab9fd170-1d8f-4b3c-87bb-96be45a6b349)




**Lab Calculator with Single value memory**


![singlevaluememory](https://github.com/IIITB-ARUL/RISCV/assets/140998631/77f6e3b2-e8bb-4719-b3d4-62a7a9970118)




TL verilog code

```
   |calc
      @0
         $reset = *reset;
         
      @1
         $val1 [31:0] = >>2$out;
         $val2 [31:0] = $rand2[3:0];
         
         $valid = $reset ? 1'b0 : >>1$valid + 1'b1 ;
         $valid_or_reset = $valid || $reset;
         
      ?$vaild_or_reset
         @1   
            $sum [31:0] = $val1 + $val2;
            $diff[31:0] = $val1 - $val2;
            $prod[31:0] = $val1 * $val2;
            $div[31:0] = $val1 / $val2;
            
         @2   
            $mem[31:0] = $reset ? 32`b0 :
                         ($op[2:0] == 3'b101) ? $val1 : >>2$mem ;
            
            $out [31:0] = $reset ? 32'b0 :
                          ($op[2:0] == 3'b000) ? $sum :
                          ($op[2:0] == 3'b001) ? $diff :
                          ($op[2:0] == 3'b010) ? $prod :
                          ($op[2:0] == 3'b011) ? $quot :
                          ($op[2:0] == 3'b100) ? >>2$mem : >>2$out ;

```



![value memory](https://github.com/IIITB-ARUL/RISCV/assets/140998631/02d2b986-3d1d-475f-a9b9-b4fd993049e4)

</details>


<details>
	<summary>
		Wrap-up
	</summary>


 **Convay's Game of life**

![Convays game](https://github.com/IIITB-ARUL/RISCV/assets/140998631/5a6e74de-5fc4-4585-8dc6-b73dd5c7ff96)


 **Pythagorean theorem**

 ![Pythagoreantheorem](https://github.com/IIITB-ARUL/RISCV/assets/140998631/a5bda9bd-8084-4def-96d1-8d439c4811c0)

![Pythagoreantheorem1](https://github.com/IIITB-ARUL/RISCV/assets/140998631/0349b36f-272c-46f1-a032-229d06f8bac1)

 

</details>

## Day 4


<details> <summary>
	Micro-architecture Of Single Cycle RISC-V CPU
</summary>



![0](https://github.com/IIITB-ARUL/RISCV/assets/140998631/d95ecb61-134b-4a7b-a5f1-a5a12c7db179)



**1.Program Counter (PC):**

The Program Counter is a special register that keeps track of the memory address of the next instruction to be executed. During normal operation, it is automatically incremented after each instruction fetch, pointing to the address of the next instruction in memory.

**2.Imem-Rd (Instruction Memory Read):** 

This block is responsible for fetching instructions from memory (typically from RAM or cache) based on the address provided by the Program Counter.The fetched instruction is sent to the Instruction Decoder for further processing.

**3.Instruction Decoder:**

The Instruction Decoder is responsible for interpreting the fetched instruction. It determines the operation to be performed, the operands involved, and the control signals required for subsequent stages.It decodes the instruction opcode and generates control signals to control other components of the processor accordingly.

**4.Register File Read:**

In most microprocessors, a Register File is used to store a set of general-purpose registers. The Register File Read stage retrieves the values from registers specified by the source operand fields in the instruction. These values are typically sent to the ALU for computation or used in other operations.

**5.Arithmetic Logic Unit (ALU):**

The Arithmetic Logic Unit is the component responsible for performing arithmetic and logical operations on data. It takes input from the Register File Read stage and performs operations such as addition, subtraction, multiplication, division, bitwise AND/OR/XOR, and more, depending on the instruction.

**6.Register File Write:**

After the ALU or other processing stages have computed a result, the Register File Write stage writes the result back to a destination register specified by the instruction.This stage updates the register values, making the results available for future instructions or for reading by the CPU.

**7.Branch:**

The Branch unit handles conditional branching operations in the processor, including conditional jumps (branches) based on the evaluation of certain conditions.It calculates the target address for a branch instruction and determines whether the branch should be taken or not, typically based on the result of a comparison operation.

</details>

<details>
	<summary>
		Fetch and Decode
	</summary>


**Program counter**


The program counter (PC) is a fundamental component of a computer's central processing unit (CPU) that keeps track of the address of the next instruction to be fetched and executed. The PC logic manages the updating of the program counter as instructions are fetched and executed in a program.

![PC2](https://github.com/IIITB-ARUL/RISCV/assets/140998631/edba7904-ccce-4e20-8022-9682d724f940)


**Fetch**

Pipeline structure of Fetch

![fetch1](https://github.com/IIITB-ARUL/RISCV/assets/140998631/4f39bf34-bdde-4ab6-b844-e301a40907d9)

This implementation has some errors so we go for the below logic


![fetch2](https://github.com/IIITB-ARUL/RISCV/assets/140998631/9f08eb29-7239-4fb6-bcf8-c6a682a8e3db)


Implementation of fetch on the makerchip



![fetch3](https://github.com/IIITB-ARUL/RISCV/assets/140998631/3f185861-0944-496f-a8a2-4afc08202982)



**Decode**

Pipelined structure of decode

![decode](https://github.com/IIITB-ARUL/RISCV/assets/140998631/713e5f6c-1e95-4501-aa1e-1e1faa052224)



![Instruction-2](https://github.com/IIITB-ARUL/RISCV/assets/140998631/88c10a5a-203c-4f0b-a635-e822300c6783)




**Instruction Immediate decode**


<img width="1055" alt="Instructiondecode" src="https://github.com/IIITB-ARUL/RISCV/assets/140998631/1173c6e6-9b3f-4100-a21b-806d64f31549">


![instructionimm-1](https://github.com/IIITB-ARUL/RISCV/assets/140998631/96bcc329-2f18-47ca-a8db-d1c175c29f3a)



**Instruction decode**


<img width="1156" alt="Instructiondecodenew" src="https://github.com/IIITB-ARUL/RISCV/assets/140998631/efb156c5-47bf-49db-bba7-46ef583d3222">


![Instructiondecodenew-1](https://github.com/IIITB-ARUL/RISCV/assets/140998631/a09b3ac6-9d00-4f16-9162-55144630a131)


**Instruction field decode**




<img width="1240" alt="Instruction-field" src="https://github.com/IIITB-ARUL/RISCV/assets/140998631/92f2293c-8c3b-4da6-9e06-6fcec2779cae">

Implementation on makerchip

![Instruction-field01](https://github.com/IIITB-ARUL/RISCV/assets/140998631/0f58a327-26f3-4334-9c2c-c5e67310bb0b)


![Instruction-field2](https://github.com/IIITB-ARUL/RISCV/assets/140998631/5c5895e4-f6f5-4457-a4d2-d967744b9a6f)


**Individual Instruction decode**

![instructionind1](https://github.com/IIITB-ARUL/RISCV/assets/140998631/b1ed852d-517b-4706-b248-f28600bf829b)

Implementation on makerchip

![instructionind2](https://github.com/IIITB-ARUL/RISCV/assets/140998631/91f108e6-3cfe-43f4-bfb1-63da0ef72c72)

</details>

<details>
	<summary>
		RISCV control logic
	</summary>



 **Lab Register file read**


![Registerfile](https://github.com/IIITB-ARUL/RISCV/assets/140998631/e17d7b9d-3f1f-4f19-bda4-a6822c0f1fdb)
![Registerfile01](https://github.com/IIITB-ARUL/RISCV/assets/140998631/7e159518-27f3-4052-b8e4-6d59f830cf1a)
![Registerfile2](https://github.com/IIITB-ARUL/RISCV/assets/140998631/d29ec90f-6e04-4e8e-99a4-a7ef30c83a9c)



 **Lab ALU**


![ALU01](https://github.com/IIITB-ARUL/RISCV/assets/140998631/c1a2c724-dbe3-4ca1-99e0-d75240558515)

![ALU2](https://github.com/IIITB-ARUL/RISCV/assets/140998631/c7f0c060-7c90-41dc-8b53-3e5648a109ba)


 **Lab Register file write**

![Registerfilewrite](https://github.com/IIITB-ARUL/RISCV/assets/140998631/9f750f3a-2f9f-4f4b-bce0-89ff5d7c9613)

![Registerfilewrite1](https://github.com/IIITB-ARUL/RISCV/assets/140998631/02dab6e9-eda4-4a5e-9d7c-2fd989cf230b)


**Arrays**


![Arrays](https://github.com/IIITB-ARUL/RISCV/assets/140998631/d2d82f04-f9d7-4233-a5d0-1ce651c8a52e)

![Arrays01](https://github.com/IIITB-ARUL/RISCV/assets/140998631/44534b20-eb25-401d-82c8-031b6ab35cc3)


 **Lab Branches**


![branches01](https://github.com/IIITB-ARUL/RISCV/assets/140998631/cfd9c5b5-5417-429b-bb4c-42038724327e)




 1.BEQ (Branch if Equal): The BEQ instruction compares two registers and branches to the target address if they are equal. It checks if the contents of two specified registers are the same, and if they are, the program counter is updated to the target address.

2.BNE (Branch if Not Equal): The BNE instruction also compares two registers but branches to the target address if they are not equal. If the contents of the two registers are different, the program counter is updated to the target address.

3.BLT (Branch if Less Than): The BLT instruction compares two signed integer values in registers and branches to the target address if the first value is less than the second value. It checks the signed comparison between two registers and jumps if the first register's value is less than the second's.

4.BGE (Branch if Greater Than or Equal): The BGE instruction compares two signed integer values in registers and branches to the target address if the first value is greater than or equal to the second value. It checks the signed comparison and jumps if the first register's value is greater than or equal to the second's.

5.BLTU (Branch if Less Than, Unsigned): The BLTU instruction compares two unsigned integer values in registers and branches to the target address if the first value is less than the second value. It performs an unsigned comparison and jumps if the first register's value is less than the second's.

6.BGEU (Branch if Greater Than or Equal, Unsigned): The BGEU instruction compares two unsigned integer values in registers and branches to the target address if the first value is greater than or equal to the second value. It performs an unsigned comparison and jumps if the first register's value is greater than or equal to the second's.

 TL verilog code

 ```
\m4_TLV_version 1d: tl-x.org
\SV
   // This code can be found in: https://github.com/stevehoover/RISC-V_MYTH_Workshop
   
   m4_include_lib(['https://raw.githubusercontent.com/BalaDhinesh/RISC-V_MYTH_Workshop/master/tlv_lib/risc-v_shell_lib.tlv'])

\SV
   m4_makerchip_module   // (Expanded in Nav-TLV pane.)
\TLV

   // /====================\
   // | Sum 1 to 9 Program |
   // \====================/
   //
   // Program for MYTH Workshop to test RV32I
   // Add 1,2,3,...,9 (in that order).
   //
   // Regs:
   //  r10 (a0): In: 0, Out: final sum
   //  r12 (a2): 10
   //  r13 (a3): 1..10
   //  r14 (a4): Sum
   // 
   // External to function:
   m4_asm(ADD, r10, r0, r0)             // Initialize r10 (a0) to 0.
   // Function:
   m4_asm(ADD, r14, r10, r0)            // Initialize sum register a4 with 0x0
   m4_asm(ADDI, r12, r10, 1010)         // Store count of 10 in register a2.
   m4_asm(ADD, r13, r10, r0)            // Initialize intermediate sum register a3 with 0
   // Loop:
   m4_asm(ADD, r14, r13, r14)           // Incremental addition
   m4_asm(ADDI, r13, r13, 1)            // Increment intermediate register by 1
   m4_asm(BLT, r13, r12, 1111111111000) // If a3 is less than a2, branch to label named <loop>
   m4_asm(ADD, r10, r14, r0)            // Store final result to register a0 so that it can be read by main program
   
   // Optional:
   // m4_asm(JAL, r7, 00000000000000000000) // Done. Jump to itself (infinite loop). (Up to 20-bit signed immediate plus implicit 0 bit (unlike JALR) provides byte address; last immediate bit should also be 0)
   m4_define_hier(['M4_IMEM'], M4_NUM_INSTRS)

   |cpu
      @0
         $reset = *reset;



      // YOUR CODE HERE
      // ...
      @0
         $pc[31:0] = >>1$reset ? 32'd0 : (>>1$taken_branch ? >>1$br_tgt_pc :  (>>1$pc+32'd4));
      @1
         //Instruction Fetch
         $imem_rd_en = !$reset;
         $imem_rd_addr[M4_IMEM_INDEX_CNT-1:0] = $pc[M4_IMEM_INDEX_CNT+1:2];
         $instr[31:0] = $imem_rd_data[31:0];
      ?$imem_rd_en
         @1
            $imem_rd_data[31:0] = /imem[$imem_rd_addr]$instr;
      @1
         //Instruction Decode
         $is_i_instr = $instr[6:2] ==? 5'b0000x ||
                       $instr[6:2] ==? 5'b001x0 ||
                       $instr[6:2] ==? 5'b11001 ||
                       $instr[6:2] ==? 5'b11100;
         
         $is_u_instr = $instr[6:2] ==? 5'b0x101;
         
         $is_r_instr = $instr[6:2] ==? 5'b01011 ||
                       $instr[6:2] ==? 5'b011x0 ||
                       $instr[6:2] ==? 5'b10100;
         
         $is_b_instr = $instr[6:2] ==? 5'b11000;
         
         $is_j_instr = $instr[6:2] ==? 5'b11011;
         
         $is_s_instr = $instr[6:2] ==? 5'b0100x;
         
         $imm[31:0] = $is_i_instr ? {{21{$instr[31]}}, $instr[30:20]} :
                      $is_s_instr ? {{21{$instr[31]}}, $instr[30:25], $instr[11:7]} :
                      $is_b_instr ? {{20{$instr[31]}}, $instr[7], $instr[30:25], $instr[11:8], 1'b0} :
                      $is_u_instr ? {$instr[31:12], 12'b0} :
                      $is_j_instr ? {{12{$instr[31]}}, $instr[19:12], $instr[20], $instr[30:21], 1'b0} :
                                    32'b0;
         $opcode[6:0] = $instr[6:0];
         
         $rs2_valid = $is_r_instr || $is_s_instr || $is_b_instr;
         ?$rs2_valid
            $rs2[4:0] = $instr[24:20];
            
         $rs1_valid = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
         ?$rs1_valid
            $rs1[4:0] = $instr[19:15];
         
         $funct3_valid = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
         ?$funct3_valid
            $funct3[2:0] = $instr[14:12];
            
         $funct7_valid = $is_r_instr ;
         ?$funct7_valid
            $funct7[6:0] = $instr[31:25];
            
         $rd_valid = $is_r_instr || $is_i_instr || $is_u_instr || $is_j_instr;
         ?$rd_valid 
            $rd[4:0] = $instr[11:7]; //rd - Destination Register
            
         $dec_bits [10:0] = {$funct7[5], $funct3, $opcode};
         $is_beq = $dec_bits ==? 11'bx_000_1100011;
         $is_bne = $dec_bits ==? 11'bx_001_1100011;
         $is_blt = $dec_bits ==? 11'bx_100_1100011;
         $is_bge = $dec_bits ==? 11'bx_101_1100011;
         $is_bltu = $dec_bits ==? 11'bx_110_1100011;
         $is_bgeu = $dec_bits ==? 11'bx_111_1100011;
         $is_addi = $dec_bits ==? 11'bx_000_0010011;
         $is_add = $dec_bits ==? 11'b0_000_0110011;
         
      @1
         //Register File Read
         $rf_wr_en = 1'b0;
         $rf_wr_index[4:0] = 5'b0;
         $rf_wr_data[31:0] = 32'b0;
         
         $rf_rd_en1 = $rs1_valid;
         $rf_rd_index1[4:0] = $rs1;
         
         $rf_rd_en2 = $rs2_valid;
         $rf_rd_index2[4:0] = $rs2;
         
         $src1_value[31:0] = $rf_rd_data1;
         $src2_value[31:0] = $rf_rd_data2;
         
      @1
         //ALU
         $result[31:0] = $is_addi ? $src1_value + $imm :
                         $is_add ? $src1_value + $src2_value :
                         32'bx ;
      @1
         //Register File Write
         $rf_wr_en = $rd_valid && $rd != 5'b0;
         $rf_wr_index[4:0] = $rd;
         $rf_wr_data[31:0] = $result;
         
      @1
         //Branch Instructions
         $taken_branch = $is_beq ? ($src1_value == $src2_value):
                         $is_bne ? ($src1_value != $src2_value):
                         $is_blt ? (($src1_value < $src2_value) ^ ($src1_value[31] != $src2_value[31])):
                         $is_bge ? (($src1_value >= $src2_value) ^ ($src1_value[31] != $src2_value[31])):
                         $is_bltu ? ($src1_value < $src2_value):
                         $is_bgeu ? ($src1_value >= $src2_value):
                                    1'b0;
         `BOGUS_USE($taken_branch)
         $br_tgt_pc[31:0] = $pc + $imm;
      // Note: Because of the magic we are using for visualisation, if visualisation is enabled below,
      //       be sure to avoid having unassigned signals (which you might be using for random inputs)
      //       other than those specifically expected in the labs. You'll get strange errors for these.

   
   // Assert these to end simulation (before Makerchip cycle limit).
   *passed = *cyc_cnt > 40;
   *failed = 1'b0;
   
   // Macro instantiations for:
   //  o instruction memory
   //  o register file
   //  o data memory
   //  o CPU visualization
   |cpu
      m4+imem(@1)    // Args: (read stage)
      m4+rf(@1, @1)  // Args: (read stage, write stage) - if equal, no register bypass is required
      //m4+dmem(@4)    // Args: (read/write stage)
      //m4+myth_fpga(@0)  // Uncomment to run on fpga

   m4+cpu_viz(@4)    // For visualisation, argument should be at least equal to the last stage of CPU logic. @4 would work for all labs.
\SV
   endmodule
```

![branches2](https://github.com/IIITB-ARUL/RISCV/assets/140998631/4b105585-dbdc-4e5a-aa22-d96bd5a90f79)




**Testbench**

A testbench in Verilog is a simulation environment that is created to verify the correctness and functionality of a digital design (usually described in another Verilog module).


![Testbench](https://github.com/IIITB-ARUL/RISCV/assets/140998631/97be42fb-cc36-4974-be4b-dc0a96786447)

</details>




## Days 5

<details>
	<summary>
		Pipelining the CPU
	</summary>
</details>


 <details>
	 <summary>
		 Solutions to pipeline hazards
	 </summary>
 </details>

<details>
	<summary>
		Load/Store Instructions and completing the RISCV CPU
	</summary>
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
