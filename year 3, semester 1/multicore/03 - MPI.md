---
related to:
created: 2025-03-02T17:41
updated: 2025-10-07T18:01
completed: false
---
used by distributed-memory systems
we compile one program that gets executed by multiple processes. we se if-else statements to specify what each process must do (similarly to what happens when you fork a process)
 - communications happen through message passing

```c
#include <stdio.h>
#include <mpi.h>

int main(void){
	MPI_Init(NULL, NULL);
	.... // where we can add MPI class
	printf("hello, world\n");
	MPI_Finalize();
	return 0;
}
```

identifiers start with `MPI_`, and the first letter following underscore is uppercase


the return value of MPI functions is `int`, and it returns a code that indicates an error