# Notes of MiniDecaf

> These are some notes when doing the experiment [MiniDecaf](https://decaf-lang.github.io/minidecaf-tutorial/).

## Basis

Steps for compiling:

1. Lexical (relating to **words**) analysis (词法分析) → tokens
2. Syntactic Analysis (according to **syntax**) / Parsing (句法分析) → Abstract Syntax Tree
    - `frontend/ast/tree.py`: defines the nodes of AST
3. Semantic (relating to **meaning**) Analysis (go through the AST, checking the type correctness, etc.)
    - `frontend/typecheck/namer.py`: the namer phase; resolve all symbols defined in the AST and store them in symbol tables
4. Generate intermediate code (e.g. **three-address code**) for code generation and optimization
    - `utils/tac/tacop.py`: define operations in TAC
    - `frontend/tacgen/tacgen.py`: generate TAC
5. Generate object code
    - `utils/riscv.py`: Convert TAC into RISCV code



## Stage 1

### Step 2

In step 2, we need only modify some codes related to TAC generation.

1. `frontend/tacgen/tacgen.py` : 
    - `expr.setattr("val", ...)` sets the value of child to the temporary value of this node.
    - `expr.op` which is `node.XXXop`needs to be projected to `tacop.XXXop`
2. `utils/tac/tacinstr.py` :
    - Complement the `__str__` attribute of related TAC operations.

Overflow example: `-(~2147483647)`.

### Step 3

Modify `frontend/tacgen/tacgen.py` for all binary operations.

```c
#include <stdio.h>

int main() {
  int a = 0x80000000;
  int b = -1;
  printf("%d\n", a / b);
  return 0;
}
```

```shell
❯ gcc -O0 test.c -o test
❯ ./test
[1]    72874 floating point exception  ./test
# same as divided by zero

❯ riscv64-unknown-elf-gcc -O0 test.c -march=rv32im -mabi=ilp32 -o test
❯ spike --isa=RV32G /usr/local/bin/pk test
bbl loader
-2147483648
```

After enabling optimization:

- `gcc` on macOS gives random values.
- qemu of RISCV gives `-2147483648`.

