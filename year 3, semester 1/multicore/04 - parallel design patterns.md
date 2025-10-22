---
related to:
created: 2025-03-02T17:41
updated: 2025-10-22T11:40
completed: false
---
# parallel program structure patterns
we can distinguish the *parallel program structure patterns* into two major categories:
## GPLS 
or **globally parallel, locally sequential**, it means that the application is able to *perform multiple tasks concurrently*, with each task running sequentially
patterns that fall into this category are:
###  SPMD
SPMD keeps all the application logic in a single program, and its structure typically involves:
- program inizialization
- obtaining a unique identifier: numbered from 0, IDs enumerate the threads/processes used. some systems use *vector identifiers*
- running the program: the execution path is diversified based on the ID
- shutting down the program: clean-up, saving results, etc.
it fails when:
- memory requirements are too high for all nodes
- heterogeneous platforms are involved
### MPMD
MPMD’s structure is identical to SPMD, but deployment involves different programs
>[!info] both SPMD and MPMP are supported by MPI
### master-worker
the processes are divided into two kinds of components:
- *masters*:  one or more, is responsible for handing out pieces of work to workers, collecting the results of the work
- *workers*: 
### map-reduce
## GSLP
 or **globally sequential, locally parallel**, it means that the application executes as a sequential program, with individual parts of it running in parallel when requested.
 patterns that fall into this category are:
### fork / join
### loop parallelism
 >[!info] img
 ![[Pasted image 20251022113348.png]]
