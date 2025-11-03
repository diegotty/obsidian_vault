---
related to:
created: 2025-03-02T17:41
updated: 2025-11-03T08:58
completed: false
---
# collective communication
collective functions are functions that involve all processes within a specified communicator. every process needs to call the function
- they are highly optimized by the MPI implementation for parallel computing, so it makes sense to use them over manual implementations
unlike point-to-point communcations (send-receive), collective calls are matched solely on the basis of the communicator and the order in which they are called (no tags !)
*colletive algorithms* are widely used in large-scale parallel applications from many domains, and often account for a large fraction of the total runtime
- especially relevant for distributed training of deep-learning models 
>[!info] met'a’s FSDP training system
![[Pasted image 20251103065523.png]]

for this reason, all the *big ballers* are designing their own collective communication libraries (NCCL for NVIDIA, RCCL for AMD, OnceCCL for intel,…)

the optimal way to perform a collective operation depends *heavily* on the specific conditions of the parallel run (message size, size of communicator, hardware topology, …), and no algorithm is fastest in all cases. 
OpenMPI does not make assumptions on the underlying hardware, and the algorithmic implementation of the collective function to run is chosen automatically through heuristics (problem solving technique that employs a pragmatic method that is not fully optimized, but is “good enough” as an approximation) (MPI uses pre-compiled decision that act as big `if-else`rules)
- proprietary libraries do make assumptions on the hardware and use that information as well as the details mentioned above to choose an implementation

### `MPI_Reduce()`
the `MPI_Reduce` function is a *collective communication function*, that combines values from *multiple processes* into a single result, and sends that result to the root
- as all collectives, it works as one call for all the processes
>[!info] illustration
![[Pasted image 20251028113049.png]]

>[!syntax] syntax
>```c
>MPI_Reduce(
>    void*        input_data_p,  // in
>    void*        output_data_p, // out
>    int          count,         // in
>    MPI_Datatype datatype,      // in
>    MPI_Op       operator,      // in
>    int          dest_process,  // in
>    MPI_Comm     comm           // in
>);
>```
>- `MPI_Op` defines the operator applied by the reduce function. the allowed operators are listed below (however, we can create custom operators with `MPI_Op_create()`)
>- it is possible to pass `NULL` to `output_data_p`, however `input_data_p` needs to always be valid and readable

| Operation value | Meaning                         |
| --------------- | ------------------------------- |
| `MPI_MAX`       | maximum                         |
| `MPI_MIN`       | minimum                         |
| `MPI_SUM`       | sum                             |
| `MPI_PROD`      | product                         |
| `MPI_LAND`      | logical and                     |
| `MPI_BAND`      | bitwise and                     |
| `MPI_LOR`       | logical or                      |
| `MPI_BOR`       | bitwise or                      |
| `MPI_LXOR`      | logical exclusive or            |
| `MPI_BXOR`      | bitwise exclusive or            |
| `MPI_MAXLOC`    | maximum and location of maximum |
| `MPI_MINLOC`    | minimum and location of minimum |
>[!warning] warning
>all the processes in the communicator must call *the same* collective function
### `MPI_Bcast()`
`MPI_Bcast` sends data belonging to a single process to all of the processes in the communicator
>[!syntax] syntax
>```c
>int MPI_Bcast(
>	void* data_p,          // in/out
>	int count,             // int
>	MPI_Datatype datatype, // in	
>	int source_proc,       // in
>	MPI_Comm comm,         // int
>);
>```

### `MPI_Allreduce()`
`MPI_Allreduce`, conceptually, is a `MPI_Reduce + MPI_Bcast`. a global sum is calculated the result and is distributed to all the processes
>[!info] illustration
![[Pasted image 20251030112641.png]]

>[!syntax] syntax
>```c
>int MPI_Allreduce(
>	void*        input_data_p,  // in
>	void*        output_data_p, // out
>	int          count,         // in
>	MPI_Datatype datatype,      // in
>	MPI_Op       operator,      // in
>	MPI_Comm     comm           // in
>);
>```
the argument is the same as `MPI_Bcast()`, except there is not `dest_process`as all the processes should get the information

>[!info] behind the scenes
![[Pasted image 20251103065251.png]]
