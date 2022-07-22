# Assembly

## What is assembly?
Assembly language is a low level language that has a strong correspondence between its instructions and the machine code instructions. It first appeared in 1949. There are many instruction sets (types of assembly language) that can have different syntax depending on the CPU instruction set architecture (but now mainly x86-64 is used for AMD and Intel CPUs).

## What is an assembly language compiler called?

The assembly language compiler is called an `assembler`. It converts assembly language into machine code, which is basically zeros and ones.

## Why should I learn assembly?

Becuase it is used for a variety of cybersecurity tasks, such as inspecting and reverse engineering malware, ransomware or even changing the behaviour of other applications, such as games. 

## CPU Registers

Processor Registers is basically quickly accessible memory inside a CPU. The registers are almost always used for arithmetic and manipulation of values, to later be stored back to main memory (RAM). Registers sizes are measured by how much memory they can hold, for example an "8-bit register" or a "64-bit registers". x86_64 has registers that hold 64-bit registers. Registers with different bit sizes are distinquished by the letter behind that specific register (`RAX is 64-bit, EAX is 32-bit, AX is 16-bit and AH/AL are 8-bit`). Here is a simple table for register sizes and names:
<br>
<br>
<img src="https://i.stack.imgur.com/N0KnG.png"/>

## Definitions

Useful definitions for later reference:
- word: 2 bytes
- double word: 4 bytes
- quadword: 8 bytes
- double quadword: 16 bytes

### That register types does a CPU have?

It has multiple different registers for different uses, here are a couple of them:
- Data registers
    - Data registers hold integers and, in some architectures, floating point values, as well as characters and bit arrays.
- Address registers
    - Address registers hold addresses and are used by assembly instructions to indirectly access RAM data.
- General Purpose Registers (GPRs)
    - These registers can hold addresses and also data. There are 8 "named" GPRs, every register has a special purpose:
        - AX (Accumulator)
            - This is the primary register. It is used for `arithmetic` operations (the result of a couple of arithmetic operations). It is also used for selecting syscalls to invoke.
        - CX (Counter)
            - This register is used in shift/rotate operations and in loops.
        - DX (Data)
            - This register is used in i/o and arithmetic. This register is also used for `syscalls`, as it is the placeholder for the 4th argument (more on syscalls later).
        - DI (Destination Index)
            - This register is used as the 2nd argument for `syscalls`.
        - SI (Source Index)
            - This register is used as the 3rd agrument for syscalls.

## Syscall

A `syscall` is a instruction that requests a service from a kernel. All syscalls have a number associated with them, and you have to move the number to the primary (accumulator) register (that means RAX, EAX, AX or AH/AL). 

### The syscall input table:

```

ID:     RAX
Arg1:   RDI
Arg2:   RSI
Arg3:   RDX
Arg4:   R10
Arg5:   R8
Arg6:   R9

```

### Sections

Sections in assembly are independent memory sequences that help organize the code a little better.

#### The `.data` section

The .data section is used for declaring constants. An example would be:

```asm
	section .data
example_const	db	"Hello, I am a constant in assembly!", 10
```

The data initialization is divided into 3 parts: `<const name> <const datatype> <const value>`. Here, we have a constant that is called **example_const** which has the datatype **db (declare bytes)** and has the value **"Hello, I am a constant in assembly!\n"**.


The datatypes for constants are the following:
- `db` - "declare bytes", a size of 1 byte.
- `dw` - "declare word", size of 2 bytes.
- `dd` - "declare double word", size of 4 bytes.
- `dq` - "declare quad word", size of 8 bytes.

### %define \<NAME> \<VAL>

The `%define` works practically the same as #DEFINE in C++, that means that `%define` is also a preprocessor macro (the name is replaced with the value before compiling).

### The linux syscall list: 
- https://blog.rchapman.org/posts/Linux_System_Call_Table_for_x86_64/

### Flags <- Come back to it later

Flags are like registers, and hold data. 

They only hold 1 bit and they are part of a larger register. They are also used for results for comparisons, for example:

```
    cmp     a,b

    IF  a == b => ZF FLAG = 1
    IF  a != b => ZF FLAG = 0
```

### Pointers

Pointers are also registers that hold data. The data they are holding is an address to the specific data.

- rip (Instruction Pointer)
    - Points to next address to be executed in the control flow. It is incremented by 1 after each instruction, causing it to execute instructions one by one from top to bottom.
- rsp (Stack Pointer)
    - Points to top of stack.
- rbp (Base Pointer)
    - Points to bottom of stack.

### jmp and call

The difference between `jmp` and `call` is that `jmp` doesn't return to the original position, whereas `call` can be returned using the `ret` instruction. \<WRITE AN EXAMPLE

### Useful Resource Links
- https://cs.lmu.edu/~ray/notes/nasmtutorial/
- https://www.tutorialspoint.com/assembly_programming/assembly_system_calls.htm
