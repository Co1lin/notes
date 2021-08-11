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

`x/20xb multstore`: display (abbreviated ‘x’) 14 hex-formatted (also ‘x’) bytes (‘b’) starting at the address where function **multstore** is located

- x86-64 instructions can range in length from 1 to 15 bytes.

Add `main.c`:

```c
#include <stdio.h>

void multstore(long, long, long *);
int main() {
    long d;
    multstore(2, 3, &d);
    printf("2 * 3 --> %ld\n", d);
    return 0;
}

long mult2(long a, long b) {
    long s = a * b;
    return s;
}
```

```shell
gcc -Og -o prog main.c mstore.c
objdump -d prog
```

The outputs contains:

```assembly
00000000000011d5 <multstore>:
    11d5:       f3 0f 1e fa             endbr64 
    11d9:       53                      push   %rbx
    11da:       48 89 d3                mov    %rdx,%rbx
    11dd:       e8 e7 ff ff ff          callq  11c9 <mult2>
    11e2:       48 89 03                mov    %rax,(%rbx)
    11e5:       5b                      pop    %rbx
    11e6:       c3                      retq   
    11e7:       66 0f 1f 84 00 00 00    nopw   0x0(%rax,%rax,1)
    11ee:       00 00
```

The addresses listed along the left are different— the linker has shifted the location of this code to a different range of addresses.

Combining assembly code with C programs

## Data Formats

![Screen Shot 2021-08-11 at 1.27.33 AM](2%20Machine-Level%20Representation%20of%20Programs.assets/Screen%20Shot%202021-08-11%20at%201.27.33%20AM.png)

- word: 2 Bytes
- l (long words) for int: 4 Bytes; for double: 8 Bytes
- s for float: 4 Bytes
- q (quad words) for long or char\*: 8 Bytes

## Accessing Info.

a set of 16 *general-purpose registers* storing 64-bit values

![Screen Shot 2021-08-11 at 1.35.51 AM](2%20Machine-Level%20Representation%20of%20Programs.assets/Screen%20Shot%202021-08-11%20at%201.35.51%20AM.png)

### Operand Speciﬁers

![image-20210811014731793](2%20Machine-Level%20Representation%20of%20Programs.assets/image-20210811014731793.png)

### Data Movement Instructions

The instructions in a class perform the same operation but with different operand sizes.

#### Simple

![image-20210811020313509](2%20Machine-Level%20Representation%20of%20Programs.assets/image-20210811020313509.png)

- A move instruction **cannot** have **both** operands refer to **memory** locations!
    - Copying a value from one memory location to another requires two instructions—the ﬁrst to load the source value into a register, and the second to write this register value to the destination.
- The size of the register must match the size designated by the last character of the instruction!
- For most cases, the `mov` instructions will only update the speciﬁc register bytes or memory locations indicated by the destination operand (**with the remaining bytes unchanged**).
    - Except for `movl` (long word is 4 Bytes, half of the 64-bit register), it will also set the high-order 4 bytes of the register to **0**.

- The regular `movq` instruction can only have **immediate** source operands that can be represented as **32**-bit two’s-complement numbers. (This value is then sign extended to produce the 64-bit value for the destination.)
    - The `movabsq` instruction can have an arbitrary **64**-bit **immediate** value as its source operand and can only have a **register** as a destination. (I → R)

#### Zero-extending

![Screen Shot 2021-08-11 at 2.35.18 AM](2%20Machine-Level%20Representation%20of%20Programs.assets/Screen%20Shot%202021-08-11%20at%202.35.18%20AM.png)

- size designators as its ﬁnal two characters—the ﬁrst specifying the source size, and the second specifying the destination size

#### Sign-extending

![Screen Shot 2021-08-11 at 2.38.52 AM](2%20Machine-Level%20Representation%20of%20Programs.assets/Screen%20Shot%202021-08-11%20at%202.38.52%20AM.png)

- Note the absence of an explicit instruction to zero-extend a 4-byte source value to an 8-byte destination in Figure 3.5. Such an instruction would logically be named movzlq, but this instruction does not exist. Instead, this type of data movement can be implemented using a movl instruction having a register as the destination. This technique takes advantage of the property that an instruction generating a 4-byte value with a register as the destination will ﬁll the upper 4 bytes with zeros.

- `cltq` has no operands—it always uses register %eax as its source and %rax as the destination for the sign-extended result. It therefore has the exact same effect as the instruction movslq %eax, %rax, but it has a more compact encoding.

Data Movement Example:

![image-20210811102633515](2%20Machine-Level%20Representation%20of%20Programs.assets/image-20210811102633515.png)

Two features:

- Dereferencing a pointer involves copying that pointer (addr) into a register (`%rdi`), and then using this register in a memory reference (`(%rdi)`).
- Local variables such as x are often kept in registers rather than stored in memory locations.

#### Stack Operation

![Screen Shot 2021-08-11 at 10.34.36 AM](2%20Machine-Level%20Representation%20of%20Programs.assets/Screen%20Shot%202021-08-11%20at%2010.34.36%20AM.png)

The stack pointer `%rsp` holds the address of the top stack element.

`pushq %rax` is equivalent to:

```assembly
subq $8, %rsp			# Decrement stack pointer Store
movq %rax, (%rsp)	# Store %rbp on stack
```

`popq %rax` is equivalent to:

```assembly
movq (%rsp), %rax		# Read %rax from stack
addq $8, (%rsp)			# Increment stack pointer
```

