---
related to:
created: 2025-03-02T17:41
updated: 2025-11-03T08:55
completed: false
---
we are interested in calculating the time elapsed since the start of the program until the end of it (which concides with the end of the last process)
>[!info] finishing at a different time
![[Pasted image 20251103084926.png]]
rank 0 is going to finish much later than rank 7, so we should report the maximum time across the ranks

however, it is not guaranteed that every rank is going to start at the same time. if not, the time we report might be longer because someone started later than someone else.
- to ensure all processes start at the same time, we use `MPI_Barrier`, a collective that *enforces synchronization* across all processes in a communicator.
## `MPI_Barrier`
- it provides an approximation 