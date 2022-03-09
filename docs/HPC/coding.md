# Coding

## OpenMP

Compile with OpenMP:

```makefile
openmp_pow: openmp_pow.cpp
	g++ $^ -O3 -std=c++11 -fopenmp -o $@
```

omp directive:

[Ref from IBM](https://www.ibm.com/docs/en/xl-c-and-cpp-linux/13.1.5?topic=parallelization-pragma-omp-parallel)

[Multithreading with OpenMP](https://ppc.cs.aalto.fi/ch3/)

```cpp
// explicitly instructs the compiler to parallelize the chosen block of code.
// #pragma omp parallel clause
// clause can be: num_threads(int_exp)
#pragma omp parallel
{
    // identifies a section of code that must be run only by the master thread.
    #pragma omp master
    thread_count = omp_get_num_threads();
}

#pragma omp parallel for // env NUM_THREADS works here; or use num_threads(expr)
for (int i = 0; i < n; i++) {}

#pragma omp parallel for collapse(2)
for (int i = 0; i < n; i++) {
    for (int j = 0; i < n; j++) {
        ;
    }
}
```

## MPI

### Basis

```cpp
// init MPI system
MPI_Init(nullptr, nullptr);

// get num of processes
int comm_sz;
MPI_Comm_size(MPI_COMM_WORLD, &comm_sz);

// get current process id/rank
int my_rank;
MPI_Comm_rank(MPI_COMM_WORLD, &my_rank);

if (my_rank == 0) {
    printf("mpi_pow: n = %d, m = %d, process_count = %d\n", n, m, comm_sz);
    fflush(stdout);
}

auto start = std::chrono::system_clock::now();
//
auto end = std::chrono::system_clock::now();

MPI_Finalize();

long long duration = std::chrono::duration_cast<std::chrono::microseconds>(end - start).count());
```

### P2P

```cpp
```



### Collective

#### Scatter and Gather

```cpp
// MPI_Barrier blocks all MPI processes in the given communicator
// until they all call this routine.
MPI_Barrier(MPI_COMM_WORLD);

// Process i get data of 
// root_a[my_rank * (n / comm_sz) : (my_rank + 1) * (n / comm_sz))
// in node 0 and stores them to a
MPI_Scatter(
    root_a, n / comm_sz, MPI_INT, // send_buf_p, send_count, send_type
    a, n / comm_sz, MPI_INT,      // recv_buf_p, recv_count, recv_type
    0, MPI_COMM_WORLD             // src_process, comm
);

// do something with local data
pow_a(a, b, n, m, comm_sz);

// Process 0 gathers the data from all processes
MPI_Gather(
    b, n / comm_sz, MPI_INT,      // send_buf_p, send_count, send_type
    root_b, n / comm_sz, MPI_INT, // recv_buf_p, recv_count, recv_type
    0, MPI_COMM_WORLD             // src_process, comm
);

MPI_Barrier(MPI_COMM_WORLD);
```

