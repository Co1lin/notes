# CUDA

*Compute Unified Device Architecture* is a parallel computing platform and application programming interfac

## Graphics Processing Unit

- SIMD
- low frequency (1.4 GHz), but many cores (8k+)
- powerful: 29.7 TFlops
- high speed of DRAM: 760 GB/s (total)
- mem. hierarchy:
    - regester
    - shared mem. : PBSM: per-block shared mem.
    - global mem.
    - const mem. ?



GPU

- Streaming Multiprocessor
    - Warp: locked step for all cores
        - Core



Host: CPU + CPU Mem.

Device: GPU + GPU Mem.



Kernel function

kernel = grid -> thread blocks -> threads

- thread blocks should be independent; they are parallel
- thread blocks are mapped to SMs

grid? how it combined with hardware



- limited number of threads in each thread block: 1024; use multiple blocks

- limited total number of threads



- `__device__` can only be **called** on GPU
- `__global__` called on host and runs on GPU
- 





Compilation Process (by nvcc)

1. seperate
2. generic: generate PTX
3. specialized



coalesced mem. access

