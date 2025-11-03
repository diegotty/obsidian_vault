---
related to:
created: 2025-11-03, 17:38
updated: 2025-11-03T18:09
completed: false
---
with the introduction of openMP and pthreads, we study how to parallelize effectively between cores of a single CPU
# threads
*threads* are analogous to a “light-weight” process. it is the smallest unit of processing that can be scheduled and executed by an OS.
it is fundamental for *parallelism*, as each thread can be assignied to a different CPU core.
>[!info] rember
a CPU core is the fundamental execution unit, comprised by ALU, registers (with PC, IR, SP, ….), CU, and L1 cache
cores can indipendently run a separate thread or process at the same time, making for true (and almost linear) speedup

having a core that can handle 2 threads means that, with the same ALU, there are resources to save 2 process contexts. that way, context switching is faster (and context switching is very expensive !)

in a shared memory program, a single process may have multiple threads of control.
[[09 - threads#multithreading]]
## openMP
*openMP* provides a higher abstraction interface. it is more portable than pthreads as it can be run on POSIX and non-POSIX system
- on POSIX systems, it is implemented on top of pthreads, on non-POSIX systems it is implemented on top of some other abstraction !
>[!info] POSIX
>*POSIX* (*portable operating system interface*) are APIs made for software compatibility between variants of unix systems and other operating systems. 

## pthreads
*pthreads* provides you with much more fine-grained control (the usual ease-of-use / performance tradeoff !)

>[!warning] while possible, it is not recommended to mix openMP and pthreads in the same code .

### multithreading in MPI
## `MPU_Init_thread()`
### threading levels
`MPI_THREAD_SINGLE` → rank that calls the function is not allowed to use threads (which is basically equivalent to calling `MPI_Init`)
`MPI_THREAD_FUNNELED` → rank can be multi-threaded but only the main thread may call MPI functions. ideal for fork-join parallelism such as used in `#pragma` omp parallel, where all MPI calls are outside the openMP regions
`MPI_THREAD_SERIALIZED` → rank can be multi-threaded but only one thread at a time may call MPI functions. the rank must ensure that MPI is used in a thread-safe way. one approach is to ensure that MPI usage is mutally excluded by all the threads
`MPI_THREAD_MULTIPLE` → rank can be multi-threaded and any thread may call MPI functions. *the MPI library ensures that this access is safe across threads*. note that this makes all MPI operations less efficient, even if only one thread makes MPI calls, so should be used only when necessary
>[!warning] not all the threading levels are supported by all the MPI implementations

>[!info] representation of threading levels
single:
![[Pasted image 20251103180811.png]]
>
>funneled:
![[Pasted image 20251103180843.png]]
>
>serialized:
![[Pasted image 20251103180905.png]]