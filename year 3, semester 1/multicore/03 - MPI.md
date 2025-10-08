---
related to:
created: 2025-03-02T17:41
updated: 2025-10-07T18:55
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


the return value of MPI functions is `int`, and it always returns a code that indicates an error


debug fico; -g + valgrind / gdb

## communicators
a communicator is a collection of processes that can send messages to each other

somma in distribuito: reduce

id process is relative to what communicator it is in ! (the rank in `MPI_Comm_rank`)

`int MPI_Comm_size` return the size of the communicator `comm` in `comm_sz_p`

slide 35: dipende dallo scheduling !
### $\verb|MPI_Send|$
- indirizzo di inizio del blocco di memoria da inviare
- number of elements of the message
- type of data being sent (int, char, float, ….) (macros, for custom structs a new mpi datatype needs to be made !)
- rank of the receiver
- arbitrary value (4 now)
- communicator 

mpi preserves endianess !

#### mpi_recv
- inizio del buffer su cui scrivere messaggio
- `source`: rank of the sender (MPI_any_RANK exists !)
- `tag`: has to match with sender’s `tag`
- `status_p`: info on what happened on the receive (ex: rank of the sender if MPI_ANY_RANK)(changed by the fuction)