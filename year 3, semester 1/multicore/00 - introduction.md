---
related to:
created: 2025-03-02T17:41
updated: 2025-10-07T17:45
completed: false
---
the performance of microprocessors has stopped increasing in a fast rate in the last 20 years, going from 50% a year  from 1986 to 2003, to a 4% increase in the last 10 years 
also, traditionally we relied on performance increase caused by increase in the **density of transistors**. said approach is not sustainable anymore, as the increased power consumption increases the heat, making the processors unreliable. we run into physical limits !
for that reason, instead of trying to have more powerful monolithic processors, we are putting **multiple processors on a single integrated circuit**

to benefit from this approach, we must be aware of the multiple processors and know how to use them. we must **explicitly** write programs to exploit the available parallelism (as this can’t be done automatically in a reliable way)

>[!example] designing a new algorithm instead of repurposing a sequential one
let’s write an algorithm that computes $n$ values and adds them together
>```c
//sequential
>int sum = 0;
>for(int i = 0; i < n; i++){
>	x = compute_next_value(..);
>	sum += x;
>}
>```

```c
// parallel_1
// private variables for each core
int my_sum = 0;
int my_first_i = ...;
int my_last_i = ...;
for(my_i = my_first_i; my_i < my_last_i; my_i++){
	my_x = compute_next_value(...);
	my_sum += my_x;
}
```
each core will have a partial sum in `my_sum`, and each core will send its `my_sum` value to a **master** core which adds the final result
however, in this way, the master is doing all the work that comes with summing, while the other cores just send their data

let’s try pairing the cores so that core 0 adds its result with core 1’s, core 2 with core 3’s, etc..
after this first round, then core 0 adds its result with core 2, core 4 with core 6, etc..
// todo ADD IMG
## writing parallel programs
**task parallelism**: partition various tasks among the cores (each core works on every piece of data)
**data parallelism**: partition the data used in solving the problem among the cores (each core fully computes the data it works on)
>[!example] example
i need to grade 300 exams, each with 15 questions, and i have 3 teaching assistants
data parallelism: each assistant grades 100 exams
task parallelism: assistant 1 grades all the exams, but only questions 1-5. assistant 2 does the same but only questions 6-10. etc….

to write parallel programs, we need to coordinate the cores, for different reasons:
- **communication**: one core sends its partial sum to another core
- **load balancing**: share the work even3
- **synchronization**: each core works at its own pace, but must make sure some core does not get too far ahead

to write parallel programs, we will use four different extensions of the C API:
- **message-passing interface**
- **posix threads (pthreads)** 
- **openMP**
- **CUDA**
higher level libraries exist, but the tradeoff is performance
### types of parallel systems
#### memory
- **shared-memory**: all the cores can share access to the computer’s memory and the cores can be coordinated by having them examine and update shared memory locations
- **distributed-memory**: each core has its own, private memory. they communicate explicitly by sending messages across a network !
#### instructions
- **multiple-instruction multiple-data (MIMD)**: each core has its own control units (can execute different instructions, and have different fetch cycles) and can work independently from the others
- **single-instruction multiple-data (SIMD)**: the same instruction is executed across all cores, but each code does so on different data (if a core wants to execute another instruction, it has to stay idle while the other core does its instruction)
	- aka vector processor, this is the GPU’s architecture

|      | shared-memory          | distributed-memory |
| ---- | ---------------------- | ------------------ |
| SIMD | CUDA                   |                    |
| MIMD | pthreads, openMD, CUDA | MPI                |
|      |                        |                    |
la GPU è divisa in gruppi di 32 core, e i gruppi al loro interno sono SIMD, ma tra i diversi gruppi si può usare MIMD (ogni gruppo ha una control unit)

### types of systems
- **concurrent**: multiple tasks can be in progress at any time (at the same time) and each task can be independent from any other (in necessarily in parallel, can be done by interleaving)
- **parallel**: multiple tasks can in in progress at any time but need to *cooperate cloosely*
- **distributed**: a program *might* need to cooperate with other programs

>[!info] hardware
to write efficient code, its better to know on which hardware we are running the program on, to optimize for that !

## von neumann architecture
tranferring data from memory to the registers is a bottleneck, known as **von neumann bottleneck**

cache is a SRAM