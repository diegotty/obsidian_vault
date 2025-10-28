---
related to:
created: 2025-03-02T17:41
updated: 2025-10-28T11:03
completed: false
---
# parallel program structure patterns
we can distinguish the *parallel program structure patterns* into two major categories:
## GPLS 
or **globally parallel, locally sequential**, it means that the application is able to *perform multiple tasks concurrently*, with each task running sequentially.
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
maps and reduce workers (so they are 2 different types of workers) can vary in number. the same function is applied to different parts of a single data item
## GSLP
 or **globally sequential, locally parallel**, it means that the application executes as a sequential program, with individual parts of it running in parallel when requested.
 patterns that fall into this category are:
### fork / join
there is a single parent thread of execution, and dynamic creation of *children tasks* at run-time. the tasks may run via threads that spawn, or via use of a static pool of threads.
- children tasks have to finish for parent thread to continue !!! (of course, as its globally sequential)
>[!info] implemented by OpenMP and Pthread

>[!example] ex

```c
mergesort(A, lo, hi):
	if lo < hi: // length of A >=1
		mid = [lo + (hi - lo)/2]
		fork mergesort(A, lo, mid) // child task
		mergesort(A, mid, hi) // done by the main task
		join
		merge(A, lo, mid, hi)
```

### loop parallelism
this technique is employed for *migration of sequential/legacy software to multicore*. it focuses on breaking up loops by manipulating the loop control variable
- a loop has to be in a particular form to support this
the flexibility with this approach is limited, but so is the development effort
>[!info] supported by OpenMP
 
 >[!info] img
 ![[Pasted image 20251022113348.png]]

## trapezoid example
>[!info] idea
>let’s approximate the definite integral of a function by calculating the trapezoids inside it !
>
given two points, $a$ and $b$, we choose an $h$ value that will be the same for each of the $n$ trapezoids
>![[Pasted image 20251028104743.png]]
area of a trapezoid : $\frac{h}{2}[l_{1}+l_{2}]$ ( with $l_{1},l_{2}$ as the two parallel sides, and $h$ the height of the trapezoid)
>
so we divide the the distance from $a$ to $b$ in $n+1$ points:
>$$
>x_{0} = a, x_{1} = a + h, x_{2} = a + 2h, \dots, x_{n-1} = a + (n-1)h, x_{n} = b
>$$
>
to find the length of $l_{1},l_{2}$ for each trapezoid, we calculate their image for $f(x)$. so the area of the trapezoid with 4 vertices $x_{i}, x_{i+1}, f(x_{i}), f(x_{i+1})$ is: $\frac{h}{2}[f(x_{i})+f(x_{i+1})]$
>
and the sum of trapezoid areas is:
>$$
>\sum^{n-1}_{{i=0}} \frac{h}{2}[f(x_{i}) + f(x_{i+1})] = 
>h\left[ \frac{f(x_{0})}{2} + f(x_{1}) +f(x_{2})+\dots+\frac{f(x_{n})}{2}) \right]
>$$
(because each term except the first and the last one appear twice)

the pseudocode to calculate the area of $n$ trapezoids from $a$ to $b$ is:
```c
h = (b-a)/n;
approx = (f(a) + f(b)) /2.0; // first and last term
for (i = 1; i <= n-1; i++) {
	x_i = a + i*h;
	approx += f(x_i);
}
approx = h*approx;
```

>[!info] parallelizing
>we parallelize this program by:
>1. partitioning the problem solution into tasks
>2. indentifying common channels between tasks
>3. aggregating tasks into composite tasks
>4. mapping composite tasks to cores
>
>![[Pasted image 20251028105022.png]]
(master-worker pattern)

parallel pseudo-code:
```c
get a,b,n;
h = (b-a)/n;
local_n = n/comm_sz; // num of trapezoids to calculate
local_a = a + my_rank*local_n*h; // starting point
local_b = a + local_n*h; // ending point
local_integral = trap(local_a, local_b, local_n, h);
if(my_rank != 0){
	send local_integral to process 0;
}else{
	total_integral = local_integral; // (f(a) + f(b))/2
	for (proc = 1; proc < comm_sz; proc++){
		receive local_integral from proc;
		total_integral += local_integral;
	}
}
if (my_rank == 0):
	print total_integral;
```

## input
most MPI programs *only allow process 0 in `MPI_COMM_WORLD` to access to stdin*.
so process 0 must read the data (with `scanfand send it to the other processes !

slide 35
MPI_Allreduce
conceptually, its a `MPI_Reduce()` followed by a `MPI_Bcast()`

recursive doubling algorithm


a good amout of time out of runtime is spent doing collective data ““processing””” (like reduce)