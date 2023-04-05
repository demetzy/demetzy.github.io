---
title: Assembly Reference
date: 2023-04-01 12:23:00 -0700
categories: [assembly]
tags: [assembly]
---

### 1. Registers:

#### General Purpose Registers:

| Register | Description                             | 64-bit | 32-bit | 16-bit | 8 high bits of lower 16 bits | 8-bit |
| -------- | --------------------------------------- | ------ | ------ | ------ | ---------------------------- | ----- |
| RAX      | Accumulator                             | RAX    | EAX    | AX     | AH                           | AL    |
| RBX      | Base                                    | RBX    | EBX    | BX     | BH                           | BL    |
| RCX      | Counter                                 | RCX    | ECX    | CX     | CH                           | CL    |
| RDX      | Data                                    | RDX    | EDX    | DX     | DH                           | DL    |
| RSI      | Source index for string operations      | RSI    | ESI    | SI     | N/A                          | SIL   |
| RDI      | Destination index for string operations | RDI    | EDI    | DI     | N/A                          | DIL   |
| RSP      | Stack Pointer                           | RSP    | ESP    | SP     | N/A                          | SPL   |
| RBP      | Base Pointer (meant for stack frames)   | RBP    | EBP    | BP     | N/A                          | BPL   |
| R8       | General purpose                         | R8     | R8D    | R8W    | N/A                          | R8B   |
| R9       | General purpose                         | R9     | R9D    | R9W    | N/A                          | R9B   |
| R10      | General purpose                         | R10    | R10D   | R10W   | N/A                          | R10B  |
| R11      | General purpose                         | R11    | R11D   | R11W   | N/A                          | R11B  |
| R12      | General purpose                         | R12    | R12D   | R12W   | N/A                          | R12B  |
| R13      | General purpose                         | R13    | R13D   | R13W   | N/A                          | R13B  |
| R14      | General purpose                         | R14    | R14D   | R14W   | N/A                          | R14B  |
| R15      | General purpose                         | R15    | R15D   | R15W   | N/A                          | R15B  |

#### Pointer Registers:

| Register | Description         | 64-bit | 32-bit | 16-bit |
| -------- | ------------------- | ------ | ------ | ------ |
| RIP      | Instruction Pointer | RIP    | EIP    | IP     |

---

### 2. Working with assembly:

- **Sample Layout**:

```nasm
; .text contains instructions
section .text
	global _start               ; this is for linker
_start:

	; write our string to stdout

	mov edx, len                ; third argument: message length
	mov ecx, msg	            ; second argument: pointer to message to write
	mov ebx, 1                  ; first argument: file handle (stdout)
	mov eax, 4                  ; system call number (sys_write)
	int 0x80                    ; call kernel

	; and exit

	mov ebx, 0                  ; first syscall argument: status code
	mov eax, 1                  ; system call number (sys_exit)
	int 0x80                    ; call kernel

; .data contains initialized data
section .data

	msg db "Hello, world!",0xa  ; the string to print + newline
	len equ $ -msg              ; length of the string

; section .bss for uninitialized values
```

- **Produce binary**:

```bash
nams -f elf32 -o hello.o hello.asm   # for 32-bit
nasm -f elf64 -o hello.o hello.asm   # for 64-bit

# Linker:
ld -m elf_i386 -o hello              # for 32-bit
ld -m elf_x86_64 -o hello hello.o    # for 64-bit
```

- **Display the disassembled executable sections in intel syntax**:

```bash
objdump -M intel -d hello
```
