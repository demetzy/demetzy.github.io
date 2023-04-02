---
title: Assembly Reference
date: 2023-04-01 12:23:00 -0800
categories: [assembly, computer architecture]
tags: [assembly]
img_path: /assets/img/pc/
---

# Assembly reference
{: style="text-align: center;"}

### 1. CPU Architecture:

![CPU](cpu-arch.png)

* **Control Unit**: gets instructions to execute from RAM using a register - the *instruction pointer*, which stores the address of the instruction to execute.

```
Some functions of the CU:
  - Determine what/where the next instruction must go for processing
  - Send clock signals to all hardware to force synchronous operations
  - Send memory taskings if appropriate
```

* **Registers**: small memory storage areas built into the processor

```
Functions of a register:
  - Store temporary data for immediate processing by the ALU
  - Hold "flag" information if an operation
    results in overflow or triggers other flags
  - Hold the location of the next instruction to be processed by the CPU
```

* **ALU**: *Arithmetic Logic Unit* executes an instruction fetched from RAM and places the results in registers or memory. The process of fetching and executing instruction after instruction is repeated as a program runs.

```
Some ALU functions:
  - Addition & subtraction
  - Determining equality
  - AND/OR/XOR/NOR/NOT/NAND logic gates and more
```

---

### 2. Program Layout:

![Memory](program-layout.jpg)

* **Text Segment**: one of the sections of a program in an object file or in memory, which contains executable instructions.
As a memory region, a text segment may be placed below the heap or stack in order to prevent heaps and stack overflows from overwriting it.

* **Initialized Data Segment**: a portion of the virtual address space of a program, which contains the global variables and static variables that are initialized by the programmer.

* **Unintialized Data Segment**: data in this segment is initialized by the kernel to arithmetic 0 before the program starts executing uninitialized data starts at the end of the data segment and contains all global variables and static variables that are initialized to zero or do not have explicit initialization in source code.
For instance, a variable declared static int i; would be contained in the BSS segment.
For instance, a global variable declared int j; would be contained in the BSS segment.
* **Stack**: the stack area traditionally adjoined the heap area and grew in the opposite direction; when the stack pointer met the heap pointer, free memory was exhausted. (With modern large address spaces and virtual memory techniques they may be placed almost anywhere, but they still typically grow in opposite directions.)
The stack area contains the program stack, a LIFO structure, typically located in the higher parts of memory.

    A “stack pointer” register tracks the top of the stack; it is adjusted each time a value is “pushed” onto the stack. The set of values pushed for one function call is termed a “stack frame”; A stack frame consists at minimum of a return address.

* **Heap**: segment where dynamic memory allocation usually takes place. The heap area begins at the end of the BSS segment and grows to larger addresses from there. The Heap area is managed by malloc, realloc, and free, which may use the brk and sbrk system calls to adjust its size (note that the use of brk/sbrk and a single “heap area” is not required to fulfill the contract of malloc/realloc/free; they may also be implemented using mmap to reserve potentially non-contiguous regions of virtual memory into the process’ virtual address space). The Heap area is shared by all shared libraries and dynamically loaded modules in a process.

---

### 3. Registers:

#### General Purpose Registers:

| Register | Description | 64-bit | 32-bit | 16-bit | 8 high bits of lower 16 bits | 8-bit |
|----------|-------------|--------|--------|--------|------------------------------|-------|
| RAX      | Accumulator | RAX    | EAX    | AX     | AH                           | AL    |
| RBX      | Base        | RBX    | EBX    | BX     | BH                           | BL    |
| RCX      | Counter     | RCX    | ECX    | CX     | CH                           | CL    |
| RDX      | Data        | RDX    | EDX    | DX     | DH                           | DL    |
| RSI      | Source index for string operations | RSI | ESI | SI | N/A | SIL |
| RDI      | Destination index for string operations | RDI | EDI | DI | N/A | DIL |
| RSP      | Stack Pointer | RSP | ESP | SP | N/A | SPL |
| RBP      | Base Pointer (meant for stack frames) | RBP | EBP | BP | N/A | BPL |
| R8       | General purpose | R8 | R8D | R8W | N/A | R8B |
| R9       | General purpose | R9 | R9D | R9W | N/A | R9B |
| R10      | General purpose | R10 | R10D | R10W | N/A | R10B |
| R11      | General purpose | R11 | R11D | R11W | N/A | R11B |
| R12      | General purpose | R12 | R12D | R12W | N/A | R12B |
| R13      | General purpose | R13 | R13D | R13W | N/A | R13B |
| R14      | General purpose | R14 | R14D | R14W | N/A | R14B |
| R15      | General purpose | R15 | R15D | R15W | N/A | R15B |


#### Pointer Registers:

| Monikers | Description | 64-bit | 32-bit | 16-bit |
|----------|-------------|--------|--------|--------|
| RIP      | Instruction Pointer | RIP    | EIP    | IP     |

---

### 4. Compile and Debug Assembly Code:

* **Compile**:

```bash 
nams -f elf32 -o hello.o hello.asm   # for 32-bit
nasm -f elf64 -o hello.o hello.asm   # for 64-bit

# Linker:
ld -m elf_i386 -o hello              # for 32-bit
ld -m elf_x86_64 -o hello hello.o    # for 64-bit
```

* **Debug**:

```bash
objdump -M intel -d hello
```

* **Compile C file**:

```bash
gcc -m32 -o hello hello.c    # for 32-bit
gcc -S -o hello.asm hello.c  # produce an assembly file

gcc -fomit-frame-pointer -o hello hello.c    # avoid using the frame pointer register <rbp>

# not to use the standard system startup files when linking object files into an executable file
gcc -nostartfiles -o hello hello.c    

# the linker will include all the required libraries and object files into an executable itself, 
# so that the file doesn't depend on any external shared libraries
gcc -static -o hello hello.c 

gcc -nostdlib -o hello hello.c   # compile without the C standard library
```
