# Parallel Computing Model

flop: floating point operation

1 KFlops/s = 10^3 Flops/s

## Hardware

### Flynn's taxonomy

- SIMD: one controller, multiple processing unit working on multiple data
    - vector extensions of Intel CPU
    - Single Instr. Multiple Threads of NVIDIA: 1 Warp contains 32 Cores

### Mem: shared or distributed

Shared mem. : same address space; direct access

- Unified Mem. Access
    - limited scalability

- Non-Unified Mem. Access
    - remote mem. access cost is higher
    - e.g. multi-way CPU server


Distributed mem. : independent address spaces; cannot directly access

![Screen Shot 2022-03-03 at 3.03.17 PM](computing_model.assets/Screen%20Shot%202022-03-03%20at%203.03.17%20PM.png)

### Processor's perspective

- SMP: Symmetric Multiprocessor: shared mem.
- CMP: Chip Multiprocessor: apply techniques in SMP into a single processor; e.g. Intel i7 (multi-core)

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

CPU <-> GPU


### Data Parallel

- compatible hardware:
    - SIMD
    - Distributed mem.

SIMD: intel vector extensions

MapReduce: map, shuffle, reduce

## Performance Index

Strong scaling

Weak scaling







