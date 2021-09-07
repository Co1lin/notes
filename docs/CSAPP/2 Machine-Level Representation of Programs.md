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

x86-64 has a set of 16 *general-purpose registers* storing 64-bit values. (While x86-32 has only 8.)

![Screen Shot 2021-08-11 at 1.35.51 AM](2%20Machine-Level%20Representation%20of%20Programs.assets/Screen%20Shot%202021-08-11%20at%201.35.51%20AM.png)

**`%rip` : program counter**

### Operand Speciﬁers

![image-20210811014731793](2%20Machine-Level%20Representation%20of%20Programs.assets/image-20210811014731793.png)

**NOTE:** $s = 1,2,4,8$

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
movq %rax, (%rsp)	# Store %rax on stack
```

`popq %rax` is equivalent to:

```assembly
movq (%rsp), %rax		# Read %rax from stack
addq $8, (%rsp)			# Increment stack pointer
```

### Arithmetic and Logical Operations

![Screen Shot 2021-08-11 at 11.29.52 AM](2%20Machine-Level%20Representation%20of%20Programs.assets/Screen%20Shot%202021-08-11%20at%2011.29.52%20AM.png)

- `leaq (%rdx), %rax` copies the effective address (but not the value stored in the address) to the destination.
- Shift
    - `<shift instruction> <imm value / single-byte register %cl>` (only allowing this speciﬁc register `%cl` as the operand)
    - a shift instruction operating on data values that are $w$ bits long determines the shift amount from the low-order $m$ bits of register `%cl`, where $2^m = w$​. The higher-order bits are ignored.
        - So, for example, when register `%cl` has hexadecimal value `0xFF`, then instruction `salb` would shift by 7, while `salw` would shift by 15, `sall` would shift by 31, and `salq` would shift by 63.
    - Different right shift instructions: 
        - `SAR` performs an arithmetic shift (ﬁll with copies of the **sign bit**)
        - `SHR` performs a logical shift (ﬁll with **zeros**)

**Special Arithmetic Operations:**

oct word: 8 words, 16-byte

![Screen Shot 2021-08-11 at 12.49.05 PM](2%20Machine-Level%20Representation%20of%20Programs.assets/Screen%20Shot%202021-08-11%20at%2012.49.05%20PM.png)

!!! danger "Byte Order"
    ![Screen Shot 2021-08-11 at 2.01.55 PM](2%20Machine-Level%20Representation%20of%20Programs.assets/Screen%20Shot%202021-08-11%20at%202.01.55%20PM.png)
    
    The code above is generated for little-endian machine, on which the high-order (more significant) bytes are stored at higher addresses.

## Control

### Condition Codes

a set of single-bit condition code registers

- CF: Carry ﬂag. The most recent operation generated **a carry out of the most signiﬁcant bit**. Used to <u>detect overﬂow for unsigned operations</u>.
- ZF: Zero ﬂag. The most recent operation yielded zero.
- SF: Sign ﬂag. The most recent operation yielded a negative value.
- OF: Overﬂow ﬂag. The most recent operation caused a two’s-complement overﬂow—either negative or positive.
- `leaq` instruction does not alter any condition codes, since it is intended to be used in address computations.
- **Otherwise, all of the instructions listed in Figure 3.10 cause the condition codes to be set.**

![Screen Shot 2021-08-11 at 2.49.09 PM](2%20Machine-Level%20Representation%20of%20Programs.assets/Screen%20Shot%202021-08-11%20at%202.49.09%20PM.png)

### Accessing the Condition Codes

![Screen Shot 2021-08-11 at 2.53.28 PM](2%20Machine-Level%20Representation%20of%20Programs.assets/Screen%20Shot%202021-08-11%20at%202.53.28%20PM.png)

e.g. Compute `a < b ` with data type `long`:

![Screen Shot 2021-08-11 at 4.09.42 PM](2%20Machine-Level%20Representation%20of%20Programs.assets/Screen%20Shot%202021-08-11%20at%204.09.42%20PM.png)

- `cmpq %rsi %rdi` ~ `%rdi - %rsi` ~ `a - b`
- `a < b` ~ `a - b < 0` ~ `SF=1 && OF=0 || SF=0 && OF=1`

### Jump Instructions

![Screen Shot 2021-08-11 at 4.37.58 PM](2%20Machine-Level%20Representation%20of%20Programs.assets/Screen%20Shot%202021-08-11%20at%204.37.58%20PM.png)

Different encodings for target instruction of jumps:

- PC relative: the difference between the address of the target instruction and the address of the instruction immediately following the jump
    - the instructions can be compactly encoded (requiring just 2 bytes)
    - the object code can be shifted to different positions in memory without alteration
- Absolute address: using 4 bytes to directly specify the target

```c
if (test_expr)
  then_statement;
else
  else_statement;
```

is equivalent to assembly code written in C:

```c
t = test_expr;
if (!t)
	goto false;
then_statement;
goto done;
false:
	else_statement;
done:
```

### Implementing Conditional Branches with Conditional Moves

![Screen Shot 2021-08-11 at 5.56.18 PM](2%20Machine-Level%20Representation%20of%20Programs.assets/Screen%20Shot%202021-08-11%20at%205.56.18%20PM.png)

- The source and destination values can be 16, 32, or 64 bits long. Single-byte conditional moves are not supported.
- The assembler can infer the operand length of a conditional move instruction from the name of the destination register, and so the same instruction name can be used for all operand lengths.

e.g. 

![Screen Shot 2021-08-11 at 5.55.22 PM](2%20Machine-Level%20Representation%20of%20Programs.assets/Screen%20Shot%202021-08-11%20at%205.55.22%20PM.png)

### Loop

Loop gets turned into a lower-level combination of tests and conditional jumps.

#### do-while

```c
loop:
	body_statement;
	t = test_expr;
	if (t)
  	goto loop;
```

#### while

the 1st way: jump to middle (`-Og`)

```c
	goto test;
loop:
	body_statement;
test:
	t = test_expr;
	if (t)
    goto loop;
```

the 2nd way: guarded do (`-O1`)

```c
	t = test_expr;
	if (!t)
    goto done;
	do
    body_statement;
	while (test_expr);
done:
```

Using this implementation strategy, the compiler can often optimize the initial test, for example, determining that the test condition will always hold.

#### for

```c
for (init-expr; test-expr; update-expr)
	body-statement;

// is equivalent to

init-expr;
while (test-expr) {
  body-statement;
  update-expr;
}
```

### switch

jump table `jt`

**The use of a jump table allows a very efficient way to implement a multiway branch!**

The authors of gcc created a new operator `&&` to create a pointer for a code location.

```c
void switch_eg(long x, long n,
               long *dest)
{
    long val = x;
    switch (n)
    {
    case 100:
        val *= 13;
        break;
    case 102:
        val += 10;
    /* Fall through */
    case 103:
        val += 11;
        break;
    case 104:
    case 106:
        val *= val;
        break;
    default:
        val = 0;
    }
    *dest = val;
}

// is translated to

void switch_eg_impl(long x, long n,
                    long *dest)
{
    /* Table of code pointers */
    static void *jt[7] = {
        &&loc_A, &&loc_def, &&loc_B,
        &&loc_C, &&loc_D, &&loc_def,
        &&loc_D};
    unsigned long index = n - 100;
    long val;

    if (index > 6)
        goto loc_def;
    /* Multiway branch */
    goto *jt[index];

loc_A: /* Case 100 */
    val = x * 13;
    goto done;
loc_B: /* Case 102 */
    x = x + 10;
/* Fall through */
loc_C: /* Case 103 */
    val = x + 11;
    goto done;
loc_D: /* Cases 104, 106 */
    val = x * x;
    goto done;
loc_def: /* Default case */
    val = 0;
done:
    *dest = val;
}
```

![Screen Shot 2021-08-11 at 11.02.32 PM](2%20Machine-Level%20Representation%20of%20Programs.assets/Screen%20Shot%202021-08-11%20at%2011.02.32%20PM.png)

Declarations of jump table:

![Screen Shot 2021-08-11 at 11.02.56 PM](2%20Machine-Level%20Representation%20of%20Programs.assets/Screen%20Shot%202021-08-11%20at%2011.02.56%20PM.png)

## Procedures

![Screen Shot 2021-08-16 at 6.59.06 PM](2%20Machine-Level%20Representation%20of%20Programs.assets/Screen%20Shot%202021-08-16%20at%206.59.06%20PM.png)

Features:

- x86-64 stack grows toward **lower** addresses
- stack pointer `%rsp` points to the top element of the stack
- stack frame structure
- When procedure P calls procedure Q, it will <u>push the return address onto the stack</u>, indicating <u>where</u> within P the program <u>should resume</u> execution once Q returns

### Control Transfer

call: 

- <u>**pushes**</u> an address A (**return address**) onto the stack and <u>**sets**</u> the PC to the beginning of Q
- return address is computed as the address of the instruction immediately <u>following</u> the call instruction

ret:

- <u>pops</u> an address A off the stack and <u>sets</u> the PC to A

![Screen Shot 2021-08-16 at 5.45.49 PM](2%20Machine-Level%20Representation%20of%20Programs.assets/Screen%20Shot%202021-08-16%20at%205.45.49%20PM.png)

### Data Transfer

![Screen Shot 2021-08-16 at 6.29.49 PM](2%20Machine-Level%20Representation%20of%20Programs.assets/Screen%20Shot%202021-08-16%20at%206.29.49%20PM.png)

When passing parameters on the stack, all data sizes are rounded up to be multiples of eight.

With the arguments in place, the program can then execute a call instruction to transfer control to procedure Q.

![Screen Shot 2021-08-16 at 6.34.42 PM](2%20Machine-Level%20Representation%20of%20Programs.assets/Screen%20Shot%202021-08-16%20at%206.34.42%20PM.png)

Except the first 6 args, others are allocated within the stack frame (with higher addresses than the stack pointer `%rsp` ).

### Local Storage on the Stack

At times, however, local data must be stored in memory:

- There are not enough registers to hold
- The address operator `&` is applied to a local variable (must generate an address)
- arrays or structures

A procedure allocates space on the stack frame by **decrementing** the stack pointer.

### Local Storage in Registers

By convention, registers `%rbx`, `%rbp`, and `%r12`–`%r15` are classified as **callee-saved registers**. When procedure P calls procedure Q, Q must **preserve** the values of these registers.

All other registers, except for the stack pointer `%rsp`, are classified as **caller-saved registers**. (<u>They can be modified by any function!</u>)

`push` and `pop` decrement and increment the stack pointer, respectively.

## Array Allocation and Access

![Screen Shot 2021-08-16 at 7.46.26 PM](2%20Machine-Level%20Representation%20of%20Programs.assets/Screen%20Shot%202021-08-16%20at%207.46.26%20PM.png)

### Pointer Arithmetic

$p$ is a pointer to data of type T , and the value of $p$ is $x_p$, then the expression $p+i$ has value $x_p + L \cdot i$, where $L$ is the size of data type T.

The array reference `A[i]` is identical to the expression `*(A+i)`.

### Nested Arrays

![Screen Shot 2021-08-16 at 8.00.00 PM](2%20Machine-Level%20Representation%20of%20Programs.assets/Screen%20Shot%202021-08-16%20at%208.00.00%20PM.png)

### Array Access Optimization

Fix-sized:

![Screen Shot 2021-08-19 at 3.27.51 PM](2%20Machine-Level%20Representation%20of%20Programs.assets/Screen%20Shot%202021-08-19%20at%203.27.51%20PM.png)

Variable-sized:

![Screen Shot 2021-08-16 at 8.20.17 PM](2%20Machine-Level%20Representation%20of%20Programs.assets/Screen%20Shot%202021-08-16%20at%208.20.17%20PM.png)

### Variable-Size Stack Frames

**Variable-sized arrays** needs the support of variable-size stack frames.

![Screen Shot 2021-08-19 at 4.55.40 PM](2%20Machine-Level%20Representation%20of%20Programs.assets/Screen%20Shot%202021-08-19%20at%204.55.40%20PM.png)

![Screen Shot 2021-08-19 at 4.56.28 PM](2%20Machine-Level%20Representation%20of%20Programs.assets/Screen%20Shot%202021-08-19%20at%204.56.28%20PM.png)

## Heterogeneous Data Structures

All of the components of a structure are stored in a contiguous region of memory and <u>a pointer to a structure is the address of its first byte</u>.

![Screen Shot 2021-08-16 at 9.46.05 PM](2%20Machine-Level%20Representation%20of%20Programs.assets/Screen%20Shot%202021-08-16%20at%209.46.05%20PM.png)

The selection of the different fields of a structure is handled completely at compile time. (The machine code contains no information about the field declarations or the names of the fields.)

### Data Alignment

The x86-64 hardware will work correctly regardless of the alignment of data. However, Intel recommends that data be aligned to improve memory system performance.

- Any primitive object of K bytes must have an **address that is a multiple of K**.
- For struct:
    - Each structure element should satisfy its alignment requirement.
    - Any pointer of the struct satisfies a K-byte alignment. (K is the maximum size of its members.) (The total size of the struct should be a multiple of K.)

## Combining Control and Data in Machine-Level Programs

The value of a function pointer is the address of the first instruction in the machine-code representation of the function.

### Buffer Overflow

Thwart:

- Stack randomization (a kind of **ASLR**, address-space layout randomization)

    - allocating a random amount of space between 0 and n bytes on the stack **at the**

        **start of a program**

- Stack protector

    - Store a special **canary value** (guard value) in the stack frame between any local buffer and the rest of the stack state.
    - Before restoring the register state and returning from the function, the program checks if the canary has been altered. (compare with the initial value)
    - `-fno-stack-protector` prevent gcc from inserting stack protector.
    - ![Screen Shot 2021-08-19 at 11.45.36 PM](2%20Machine-Level%20Representation%20of%20Programs.assets/Screen%20Shot%202021-08-19%20at%2011.45.36%20PM.png)
      
        ![Screen Shot 2021-08-19 at 4.27.10 PM](2%20Machine-Level%20Representation%20of%20Programs.assets/Screen%20Shot%202021-08-19%20at%204.27.10%20PM.png)

Attack:

- nop (no operation) sequence (nop sled, i.e. **slides** through the sequence)





