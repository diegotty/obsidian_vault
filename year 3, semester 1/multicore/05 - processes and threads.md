---
related to:
created: 2025-11-03, 17:38
updated: 2025-11-03T17:54
completed: false
---
# threads
*threads* are analogous to a “light-weight” process. it is the smallest unit of processing that can be scheduled and executed by an OS.
it is fundamental for *parallelism*, as each thread can be assignied to a different CPU core.
>[!info] rember
a CPU core 
cores can indipendently run a separate thread or process at the same time

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

### threading levels in MPI