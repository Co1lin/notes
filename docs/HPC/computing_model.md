# Parallel Computing Model

flop: floating point operation

1 KFlops/s = 10^3 Flops/s

## Hardware

PU: Flynn's taxonomy

MEM: shared / distributed mem.

Shared mem. : same address space; direct access

- Unified Mem. Access
- Non-Unified Mem. Access

Distributed mem. : independent address spaces; cannot directly access

From processor's perspective:

- SMP: Symmetric Multiprocessor: shared mem.
- CMP: Chip Multi Processor: apply techniques in SMP into a single processor; e.g. Intel i7 (multi-core)

## Programming Models

Threads: 
- shared data, indep. instructions
- unit for scheduling

Processes: 
- indep. data
- unit for allocating resources

### Shared mem.

- shared var.
- threads
- models
    - pthread
    - OpenMP



### Message Passing

- distributed mem.
- multi-processing
- Message Passing Interface




### Data Parallel

- compatible hardware:
    - SIMD
    - Distributed mem.

SIMD

MapReduce: map, shuffle, reduce

## Performance Index

Strong scaling

Weak scaling







