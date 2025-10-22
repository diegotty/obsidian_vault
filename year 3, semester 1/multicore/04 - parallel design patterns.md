---
related to:
created: 2025-03-02T17:41
updated: 2025-10-22T11:51
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
- *masters*:  one or more, is responsible for:
	- handing out pieces of work to workers
	- collecting the results of the work done by the workers
	- performing I/O duties on behalf of the workers (i.e. sending them the data they are supposed to process, accessing a file, ..)
	- interacting with the user
	clearly, the master can be a bottleneck ! however, we can use a hierarchy of masters
- *workers*: do the work given by the masters. the same function is applied to different data items
### map-reduce
a variation of the master-worker pattern, made popular by google’s search engine implementation
the master coordinates the whole operation.
workers run two typers of tasks:
- *map*: apply a function on data, resulting in a set of partial results
- *reduce*: collect the partial results and derive the complete one
maps and reduce workers can vary in number. the same function is applied to different parts of a single data item
## GSLP
 or **globally sequential, locally parallel**, it means that the application executes as a sequential program, with individual parts of it running in parallel when requested.
 patterns that fall into this category are:
### fork / join
### loop parallelism
 >[!info] img
 ![[Pasted image 20251022113348.png]]
