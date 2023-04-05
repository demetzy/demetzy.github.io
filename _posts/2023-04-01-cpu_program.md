---
title: Computer Architecture and Program Layout
date: 2023-04-01 12:20:00 -0700
categories: [computer architecture]
tags: [computer]
img_path: /assets/img/pc/
---

## Introduction
{: style="text-align: center;"}

### 1. Computer Architecture:

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