# MIPS Basis

## Introduction

CISC: **complex** instruction set computer; e.g. x86-64

RISC: **reduced** instruction set computers

MIPS: Microprocessor without Interlocked Pipeline Stages; a typical RISC

5-Stage Pipeline

- Fetch instruction
- Read registers
- Arithmetic operation
- Memory access
- Write back

Compare to x86-64

![Screen Shot 2021-08-25 at 10.16.32 PM](5%20MIPS%20Basis.assets/Screen%20Shot%202021-08-25%20at%2010.16.32%20PM.png)

![Screen Shot 2021-08-25 at 10.19.22 PM](5%20MIPS%20Basis.assets/Screen%20Shot%202021-08-25%20at%2010.19.22%20PM.png)

![Screen Shot 2021-08-25 at 10.20.20 PM](5%20MIPS%20Basis.assets/Screen%20Shot%202021-08-25%20at%2010.20.20%20PM.png)

Branch Delay Slot is visible too software to provide the possibility of optimization at upper-level:

![Screen Shot 2021-08-25 at 10.23.26 PM](5%20MIPS%20Basis.assets/Screen%20Shot%202021-08-25%20at%2010.23.26%20PM.png)

**No branch prediction!**

## Registers

There are 32 general-purpose registers for your program to use: \$0 to \$31.

- \$0 Always returns zero, no matter what you store in it.
- \$31 Is always used by the normal subroutine-calling instruction `jal` for the **return address**.
    - `jalr` can use any register for the return address

The ﬂoating-point math coprocessor (ﬂoating-point accelerator, or FPU), if available, adds 32 ﬂoating-point registers; in simple assembly language, they are called \$f0 to \$f31.

![Screen Shot 2021-08-26 at 12.49.31 AM](5%20MIPS%20Basis.assets/Screen%20Shot%202021-08-26%20at%2012.49.31%20AM.png)

![Screen Shot 2021-08-26 at 10.35.57 AM](5%20MIPS%20Basis.assets/Screen%20Shot%202021-08-26%20at%2010.35.57%20AM.png)

## Data Types

![Screen Shot 2021-08-26 at 9.58.08 AM](5%20MIPS%20Basis.assets/Screen%20Shot%202021-08-26%20at%209.58.08%20AM.png)

## Coprocessor 0

```assembly
mtc0	<register s of CPU>, <n_th register of CP0>	# Move to coprocessor 0
mfc0	d, $n	# d is loaded with the values from CPU control register number n
```

```assembly
mfc0	t0, SR	# Status Register
and		t0, <complement of bits to clear>
or		t0, <bits to set>
mtc0	t0, SR
```

## Instructions

```assembly
lw $1, offset($2)

lb $1, offset($2)	# load byte with sign extension
lbu $1, offset($2)	# load byte with zero extension
```

Add

![Screen Shot 2021-08-26 at 11.24.56 AM](5%20MIPS%20Basis.assets/Screen%20Shot%202021-08-26%20at%2011.24.56%20AM.png)

Count leading zeros / ones

![Screen Shot 2021-08-26 at 11.25.14 AM](5%20MIPS%20Basis.assets/Screen%20Shot%202021-08-26%20at%2011.25.14%20AM.png)

Multiply / Divide

![Screen Shot 2021-08-26 at 11.28.19 AM](5%20MIPS%20Basis.assets/Screen%20Shot%202021-08-26%20at%2011.28.19%20AM.png)

Multiply and Add / Subtract

![Screen Shot 2021-08-26 at 11.31.31 AM](5%20MIPS%20Basis.assets/Screen%20Shot%202021-08-26%20at%2011.31.31%20AM.png)

Unsigned / Signed compare, less than

![Screen Shot 2021-08-26 at 11.32.18 AM](5%20MIPS%20Basis.assets/Screen%20Shot%202021-08-26%20at%2011.32.18%20AM.png)

Branch

![Screen Shot 2021-08-26 at 11.50.25 AM](5%20MIPS%20Basis.assets/Screen%20Shot%202021-08-26%20at%2011.50.25%20AM.png)

![Screen Shot 2021-08-26 at 11.57.36 AM](5%20MIPS%20Basis.assets/Screen%20Shot%202021-08-26%20at%2011.57.36%20AM.png)

Load / Save

![Screen Shot 2021-08-26 at 12.00.33 PM](5%20MIPS%20Basis.assets/Screen%20Shot%202021-08-26%20at%2012.00.33%20PM.png)

Load Link / Store Conditional

![Screen Shot 2021-08-26 at 12.11.42 PM](5%20MIPS%20Basis.assets/Screen%20Shot%202021-08-26%20at%2012.11.42%20PM.png)

Logical

![Screen Shot 2021-08-26 at 12.14.00 PM](5%20MIPS%20Basis.assets/Screen%20Shot%202021-08-26%20at%2012.14.00%20PM.png)

### Directives

![Screen Shot 2021-08-26 at 12.42.07 PM](5%20MIPS%20Basis.assets/Screen%20Shot%202021-08-26%20at%2012.42.07%20PM.png)

![Screen Shot 2021-08-26 at 12.42.18 PM](5%20MIPS%20Basis.assets/Screen%20Shot%202021-08-26%20at%2012.42.18%20PM.png)

![Screen Shot 2021-08-26 at 12.42.36 PM](5%20MIPS%20Basis.assets/Screen%20Shot%202021-08-26%20at%2012.42.36%20PM.png)







