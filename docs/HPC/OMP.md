# OpenMP



- Multi-threading
- Shared mem.
- **Incremental** parallelization



fork-join model



Task distribution structure:

- do / for
- section
- single



for loop scheduling:

- static: suitable for balanced workload
- dynamic
- guided: k tasks for the 1st time; k/2 tasks for the 2nd time; ...
- - better working balance but more cost than static
    - not balanced very well as dynamic

sections

```cpp
#pragma omp parallel num_threads(2)
{
  #pragma omp sections
  {
    #pragma omp section
    {
      // section1
    }
    #pragma omp section
    {
      // section2
    }
  }
}
```



Sync

critical

atomic



Loosen Consistency

- value of a shared variable is not guaranteed to be the latest

- need "flush"

- auto flushed commands:

    - in/out: parallel, critical, ordered
    - out: for, sections, single

- manually flush: 

    ````cpp
    #pragma omp flush [acq_rel / release / acquire] [var1, ...]
    ````
    
- 









