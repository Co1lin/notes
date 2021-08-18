# 3 Linking

![Screen Shot 2021-08-18 at 10.27.19 AM](3%20Linking.assets/Screen%20Shot%202021-08-18%20at%2010.27.19%20AM.png)

## Static Linking

Static linkers such as the Linux `ld`: a collection of relocatable object files → a fully linked executable object file that can be loaded and run

Symbol Resolution:

- Symbols: a function, a global variable, or a static variable
- associate each symbol reference with exactly one symbol definition

Relocation:

- Compilers and assemblers generate code and data sections that start at address 0.
- The linker relocates these sections by **associating** a memory location with each symbol definition, 
- and then modifying all of the references to those symbols so that <u>they point to this memory location</u>
- relocation entries

## Object Files

- Relocatable object file: compilers and assemblers (ELF files)
- Executable object file: linkers
- Shared object file: compilers and assemblers

## Symbols and Symbol Tables

- Global symbols #1
    - <u>defined by module m</u> 
    - can be referenced by other modules
    - nonstatic C functions and global variables
- Global symbols #2
    - referenced by module m
    - <u>defined by some other module</u>
    - externals
    - nonstatic C functions and global variables defined in other modules
- Local symbols
    - defined and referenced <u>exclusively</u> by module m
    - static C functions and global variables that are <u>defined with the static attribute</u>
    - visible anywhere within module m, <u>cannot be referenced by other modules</u>

## Symbol Resolution

### Resolve Duplicate Symbol Names

- Strong symbols: functions and initialized global variables
- Weak symbols: uninitialized global variables

Rule 1. Multiple strong symbols with the same name are not allowed.

Rule 2. Given a strong symbol and multiple weak symbols with the same name, choose the strong symbol.

Rule 3. Given multiple weak symbols with the same name, choose any of the weak symbols.

### Linking with Static Libraries

![Screen Shot 2021-08-18 at 2.35.40 PM](3%20Linking.assets/Screen%20Shot%202021-08-18%20at%202.35.40%20PM.png)

On Linux systems, static libraries are stored on disk in a particular file format known as an *archive*, which is a collection of concatenated relocatable object files.

To create a static library of these functions, we would use the `ar` tool as follows:

```shell
gcc -c addvec.c multvec.c
ar rcs libvector.a addvec.o multvec.o
# use libvector.a to build the executable
gcc -static -o prog2c main2.o ./libvector.a
```

During the symbol resolution phase, the linker scans the relocatable object files and archives **left to right** in the same sequential order that they appear on the compiler driver’s command line.

The general rule for libraries is to place them at the end of the command line.

## Relocation

- Relocating sections and symbol definitions
- Relocating symbol references within sections

![Screen Shot 2021-08-18 at 3.48.51 PM](3%20Linking.assets/Screen%20Shot%202021-08-18%20at%203.48.51%20PM.png)

![Screen Shot 2021-08-18 at 3.49.52 PM](3%20Linking.assets/Screen%20Shot%202021-08-18%20at%203.49.52%20PM.png)

- `R_X86_64_PC32`: Relocate a reference that uses a 32-bit PC-relative address.
- `R_X86_64_32`: Relocate a reference that uses a 32-bit absolute address.

![Screen Shot 2021-08-18 at 4.17.57 PM](3%20Linking.assets/Screen%20Shot%202021-08-18%20at%204.17.57%20PM.png)

## Loading

![Screen Shot 2021-08-18 at 5.04.44 PM](3%20Linking.assets/Screen%20Shot%202021-08-18%20at%205.04.44%20PM.png)

## Shared Libraries

### At Linking Time

A shared library is an object module that, at either run time or load time, can be loaded at an arbitrary memory address and linked with a program in memory.

`.so` on Linux and `.dll` on Windows.

- in any given **file system**, there is exactly one `.so` file for a particular library
- a single copy of the `.text` section of a shared library in **memory** can be shared by different running processes

Build a shared library:

```shell
# pic: position-independent code
gcc -shared -fpic -o libvector.so addvec.c multvec.c
# link into program
gcc -o prog2l main2.c ./libvector.so
```

![Screen Shot 2021-08-18 at 4.41.51 PM](3%20Linking.assets/Screen%20Shot%202021-08-18%20at%204.41.51%20PM.png)

### At Run Time

![Screen Shot 2021-08-18 at 5.19.26 PM](3%20Linking.assets/Screen%20Shot%202021-08-18%20at%205.19.26%20PM.png)

## PIC

![Screen Shot 2021-08-18 at 5.38.04 PM](3%20Linking.assets/Screen%20Shot%202021-08-18%20at%205.38.04%20PM.png)

