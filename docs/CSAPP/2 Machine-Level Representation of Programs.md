# 2 Machine-Level Representation of Programs

The command-line option -Og instructs the compiler to apply a level of optimization that yields machine code that follows the overall structure of the original C code.

- *program counter*
- integer *register file*
- condition code registers
- vector registers

Example:

```c
// mstore.c

long mult2(long, long);

void multstore(long x, long y, long* dest) {
    long t = mult2(x, y);
    *dest = t;
}
```

```shell
# generate assembly code
gcc -Og -S mstore.c

# compile and assemble
gcc -Og -c mstore.c
```

```assembly
# mstore.s contains the following snippet
multstore:
.LFB0:
	.cfi_startproc
	endbr64
	pushq	%rbx
	.cfi_def_cfa_offset 16
	.cfi_offset 3, -16
	movq	%rdx, %rbx
	call	mult2@PLT
	movq	%rax, (%rbx)
	popq	%rbx
	.cfi_def_cfa_offset 8
	ret
	.cfi_endproc
```

```
# mstore.o contains the following sequence
53 48 89 d3 e8 00 00 00 00 48 89 03 5b c3
```

Use disassemblers to inspect the contents of machine-code files:

```shell
linux> objdump -d mstore.o
mstore.o:     file format elf64-x86-64


Disassembly of section .text:

0000000000000000 <multstore>:
   0:   f3 0f 1e fa             endbr64 
   4:   53                      push   %rbx
   5:   48 89 d3                mov    %rdx,%rbx
   8:   e8 00 00 00 00          callq  d <multstore+0xd>
   d:   48 89 03                mov    %rax,(%rbx)
  10:   5b                      pop    %rbx
  11:   c3                      retq
```

We can also use `gdb` to display the binary object code for a program:

```
linux> gdb mstore.o
(gdb) x/20xb multstore
0x0 <multstore>:        0xf3    0x0f    0x1e    0xfa    0x53    0x48    0x89    0xd3
0x8 <multstore+8>:      0xe8    0x00    0x00    0x00    0x00    0x48    0x89    0x03
0x10 <multstore+16>:    0x5b    0xc3    Cannot access memory at address 0x12
```

display (abbreviated ‘x’) 14 hex-formatted (also ‘x’) bytes (‘b’) starting at the address where function **multstore** is located

