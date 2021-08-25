# Assembly Language Programming

## Directives

```assembly
.text		# codes

# .p2align expr1, expr2, expr3
# align to 2^expr1, padding with expr2, max padding number of bytes expr3
.p2align 4, , 15
# If doing the alignment would require skipping more bytes than the specified maximum, then the alignment is not done at all.

.section .rdata # read-only code section
LC0:	# local
	.ascii	"Hello World\12\0"
```

Example:

```assembly
.globl	_c

.data	# data section
.align 4
_c:	# start address symbol
	.long 1
# int c = 1;
```

```shell
gcc -O2 \
-mpreferred-stack-boundary=2 \	# align to 2^2=4
-fomit-frame-pointer \	# omit the frame pointer %rbp
helloworld.c -S
```

Frame Pointer:

![Picture1](4%20Programming.assets/Picture1.png)

Memory Layout:

![Screen Shot 2021-08-25 at 4.43.23 PM](4%20Programming.assets/Screen%20Shot%202021-08-25%20at%204.43.23%20PM.png)

## Assembly Programming

```shell
as -o myobj.o hello.s
# -gstabs with debugging info

ld -o myobj.o
```

```assembly
.data	# data section
msg:
	.ascii "Hello World\n"
	len = . - msg	# . means the current location
.text	# code section
.globl _start	# entry point
_start:
	movq	$len, %rdx	# 3th arg. of sys. call, how many to write
	movq	$msg, %rsi	# 2th arg. of sys. call, starting address of what to write
	movq	$1, %rax	# code of "write"
	movq	$1, %rdi	# write to 1(stdout)
	# int	$0x80		# interrupt instruction in i386
	syscall
	
	movl	$60, %rax	# code of "exit"
	movl	$0, %rdi	# arg1 of exit, 0, exit status
	syscall
```

When executing `int $0x80`,

- `%rax` stores the function/service code of a system call.
- `%rdi, %rsi, %rdx, %r10, %r8, %r9` are arguments.
- More arguments are shored in a continuous area in memory which is pointed by `%rbx`.

After calling, `%eax` stores the return value.







