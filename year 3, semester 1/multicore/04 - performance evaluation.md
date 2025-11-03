---
related to:
created: 2025-11-03T08:26
updated: 2025-11-03T09:09
completed: false
---
we are interested in calculating the time elapsed since the start of the program until the end of it (which concides with the end of the last process)
>[!info] finishing at a different time
![[Pasted image 20251103084926.png]]
rank 0 is going to finish much later than rank 7, so we should report the maximum time across the ranks

however, it is not guaranteed that every rank is going to start at the same time. if not, the time we report might be longer because someone started later than someone else.
- to ensure all processes start at the same time, we use [[performance evaluation#`MPI_Barrier`|MPI_Barrier()]]
## `MPI_Barrier`
`MPI_Barrier` is a collective that *enforces synchronization* across all processes in a communicator. it acts like a wall that all participating processes must reach before any of them can proceed !
- when the first process reaches the barrier, it halts execution and waits until every process in the comm has also called the barrier collective
this function is not a guarantee, but a reasonable approximation of 