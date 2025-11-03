---
related to:
created: 2025-11-03T08:26
updated: 2025-11-03T09:24
completed: false
---
# performance evaluation
we are interested in calculating the time elapsed since the start of the program until the end of it, which concides with the end of the last process. we then must report the *maximum runtime across the rank* as the result of a run !
>[!info] finishing at a different time
![[Pasted image 20251103084926.png]]
rank 0 is going to finish much later than rank 7, so we should report the maximum time across the ranks

however, it is not guaranteed that every rank is going to start at the same time. if not, the time we report might be longer because someone started later than someone else.
- to ensure all processes start at the same time, we use [[performance evaluation#`MPI_Barrier`|MPI_Barrier()]]
## `MPI_Barrier`
`MPI_Barrier` is a collective that *enforces synchronization* across all processes in a communicator. it acts like a wall that all participating processes must reach before any of them can proceed !
- when the first process reaches the barrier, it halts execution and waits until every process in the comm has also called the barrier collective
this function is *not a guarantee, but a reasonable approximation* of synchronization !

## reporting performance
we must note that performance data is *non deterministic*: 100 runs of the same application will give 100 different results, as a result of interference from other application and/or the OS for the single compute node and interference on the network across multiple nodes
>[!info] bottlenecks
![[Pasted image 20251103091159.png]]

we *have to run the application multiple times*, and we report the entire distribution of timings
>[!info] graph showing distribution of timings
![[Pasted image 20251103091302.png]]

>[!info] the impact of noise
![[Pasted image 20251103091457.png]]
as the number of GPUs increases, the goodput gets worse. why ? because the more ranks you have, the more likely it is that at least one of them is affected by noise
## expectations
>[!info] run-times of serial and parallel matrix-vector multiplication
![[Pasted image 20251103091942.png]]
the runtime increases with problem size and decreases with the number of processes.
however, we can se that we measure the same runtime when doubling the processes from 8 to 16 in the first column. we must be able to have realistic expectations !

ideally, when running with $p$ processes, the program should be $p$ times faster than when running with 1 process. we define:
- $T_{serial}(n)$ as teh time of our sequential application on a problem of size $n$ (e.g. the dimension of the matrix)
- $T_{parall\ell}$