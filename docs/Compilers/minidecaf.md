# Notes of MiniDecaf

> These are some notes when doing the experiment [MiniDecaf](https://decaf-lang.github.io/minidecaf-tutorial/).

## Basis

Steps for compiling:

1. Lexical (relating to **words**) analysis (词法分析) → tokens
2. Syntactic Analysis (according to **syntax**) / Parsing (句法分析) → Abstract Syntax Tree
3. Semantic (relating to **meaning**) Analysis (go through the AST, checking the type correctness, etc.)
4. Generate intermediate code (e.g. three-address code) for code generation and optimization
5. Generate object code

## Step 2

In step 2, we need only modify some codes related to TAC generation.

1. `frontend/tacgen/tacgen.py` : 

    - `expr.setattr("val", ...)` sets the value of child to the temporary value of this node.
    - `expr.op` which is `node.XXXop`needs to be projected to `tacop.XXXop`
2. `utils/tac/tacinstr.py` :
    - Complement the `__str__` attribute of related TAC operations.

Overflow example: `-(~2147483647)`.

