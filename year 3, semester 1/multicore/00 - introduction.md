---
related to:
created: 2025-03-02T17:41
updated: 2025-09-30T12:34
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
// private variables for each core
int my_sum = 0;
int my_first_i = ..
```
