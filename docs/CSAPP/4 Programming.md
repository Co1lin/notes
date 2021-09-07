# Assembly Language Programming

## Directives

```assembly
.text		# codes

.data		# data section of data having initial value
# type declaration required

.bss		# data section of data without initial value
# save memory; no type declaration required

# .p2align expr1, expr2, expr3
# align to 2^expr1, padding with expr2, max padding number of bytes expr3
.p2align 4, , 15
# If doing the alignment would require skipping more bytes than the specified maximum, then the alignment is not done at all.

.section .rodata # read-only code section
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

![Screen Shot 2021-08-25 at 8.51.24 PM](4%20Programming.assets/Screen%20Shot%202021-08-25%20at%208.51.24%20PM.png)

## System call

```shell
as -o myobj.o hello.s
# -gstabs with debugging info

ld myobj.o -o myobj
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
	
	movq	$60, %rax	# code of "exit"
	movq	$0, %rdi	# arg1 of exit, 0, exit status
	syscall
```

When executing `int $0x80`,

- `%rax` stores the function/service code of a system call.
- `%rdi, %rsi, %rdx, %r10, %r8, %r9` are arguments.
- More arguments are shored in a continuous area in memory which is pointed by `%rbx`.

After calling, `%eax` stores the return value.



## Command Line Arguments

`int argc`, `char* argv[]` are pushed on the the stack when the program starts.

```assembly
.text
.globl	_start
_start:
	popq %rsi	# argc
vnext:
	popq %rsi	# argv[?]
	testq	%rsi, %rsi
	jz	exit	# jump if zero
	movq	%rsi, %rbx
	xorq	%rdx, %rdx	# get 0
strlen:
	movb	(%rbx), %al	# get the last 8-bit as a byte
	incq	%rdx
	incq	%rbx
	testb	%al, %al
	jnz	strlen
	movb	$10, -1(%rbx)	# 10 is a new line
	movq	$1, %rax	# system call
	movq	$1, %rdi # write to stdout
	jmp vnext
exit:
	movq	$60, %rax
	movq	$0, %rdi
	syscall
```

## Call Functions in C Library

```assembly
.section	.rodata
.LC0:
	.string	"hello\n"

.text
.globl	_start

_start:
	movl	$.LC0, %edi
	call puts
	movl $0, %edi
	call exit
```

```shell
as -o hello.o hello.s
ld -lc -dynamic-linker /lib64/ld-linux-x86-64.so.2 -o hello hello.o
```





