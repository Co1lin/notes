# MPI Design

SPMD: Single Program Multi. Data



## P2P Communication



## Collective Communication

### Bcast and Reduce

Broadcast A from P1 to all of the processes. (1-to-all)

![Screen Shot 2022-03-09 at 5.22.36 PM](MPI.assets/Screen%20Shot%202022-03-09%20at%205.22.36%20PM.png)

Reduce data from all of the processes to P2 with sum op. (all-to-1)

![Screen Shot 2022-03-09 at 5.32.30 PM](MPI.assets/Screen%20Shot%202022-03-09%20at%205.32.30%20PM.png)

### Scatter and Gather

Scatter: Distribute different data to different processes. (1-to-all)

- The root node has all of the data at the beginning.
- The root node sends different parts of the data to different processes.

![Screen Shot 2022-03-09 at 5.34.51 PM](MPI.assets/Screen%20Shot%202022-03-09%20at%205.34.51%20PM.png)

Gather: The root node gathers data from all of the processes. (all-to-1)

![Screen Shot 2022-03-09 at 5.36.26 PM](MPI.assets/Screen%20Shot%202022-03-09%20at%205.36.26%20PM.png)

### All-X

Allgather = Gather + Bcast

![Screen Shot 2022-03-09 at 5.38.17 PM](MPI.assets/Screen%20Shot%202022-03-09%20at%205.38.17%20PM.png)

Allreduce = Reduce + Bcast

![Screen Shot 2022-03-09 at 5.39.20 PM](MPI.assets/Screen%20Shot%202022-03-09%20at%205.39.20%20PM.png)

**Alltoall**: Each process gathers different data from all of the processes. (n ✖️ Gather)

![Screen Shot 2022-03-09 at 5.41.32 PM](MPI.assets/Screen%20Shot%202022-03-09%20at%205.41.32%20PM.png)

## Communicator and Group

`MPI::COMM_WORLD` : Pre-defined communicator for all of the processes

- Group: within which processes can communicate to each other
- Communicator: each one binds to a group; to provide communication functions

![Screen Shot 2022-03-09 at 5.47.14 PM](MPI.assets/Screen%20Shot%202022-03-09%20at%205.47.14%20PM.png)

## MPI-IO

POSIX (Portable Operating System Interface) v.s. MPI:

![image-20220309180303178](MPI.assets/image-20220309180303178.png)

MPI-IO can:

- Open the file only once
- R/W simultaneously

Independent IO (traditional?)

- Each process has a request
- Serial when the same file is requested by different processes

Collective IO

- Shared R/W buffer: combine many queries into a single one -> reduce IO request
- sync needed

![Screen Shot 2022-03-09 at 5.59.19 PM](MPI.assets/Screen%20Shot%202022-03-09%20at%205.59.19%20PM.png)



