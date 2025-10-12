---
related to:
created: 2025-03-02T17:41
updated: 2025-10-12T17:06
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
- `status_p`: info on what happened on the receive (ex: rank of the sender if MPI_ANY_RANK, same with ANY_SOURCE)(changed by the fuction)

MPI requires that messages be **nonovertaking**: if process $q$ sends two messages to process $r$, the first message sent by $q$ must be available to $r$ before the second message
- however, there is no restriction on the arrival of messages sent from different processes

## data types

| MPI datatype       | C datatype           |
| ------------------ | -------------------- |
| MPI_CHAR           | signed char          |
| MPI_SHORT          | signed short int     |
| MPI_INT            | signed int           |
| MPI_LONG           | signed long int      |
| MPI_LONG_LONG      | signed long long int |
| MPI_UNSIGNED_CHAR  | unsigned char        |
| MPI_UNSIGNED_SHORT | unsigned short int   |
| MPI_UNSIGNED       | unsigned int         |
| MPI_UNSIGNED_LONG  | unsigned long int    |
| MPI_FLOAT          | float                |
| MPI_DOUBLE         | double               |
| MPI_LONG_DOUBLE    | long double          |
| MPI_BYTE           |                      |
| MPI_PACKED         |                      |

## message matching
a message is successfully received if:
- `recv_type = send_type`
- `recv_buf_sz >= send_buf_sz`

a received can get a message withouth knowing:
- the amount of data in the message
- the sender of the message (`MPI_ANY_SOURCE`)
- the tag of the message (`MPI_ANY_TAG`)


## issue with send and receive
- **send**: when the send function returns, i  have no idea if the message got delivered successfully, if the message was sent at all or its still in the sender’s process. however, after sending the message, i can alter the things in buffer that was sent (as MPI guarantees that by like making a copy or smn)

mpi has a buffer where it stores messages that were sent but still not receveid by anyone (nobody called the receive funciton with the matching parameters)
- this cant be done with very big messages
- to fix this, mpi asks the received if it is ready to receive the message. during this period of time, the send può essere bloccante (ma può essere bloccante anche se è piccola)

so if two processes send to each other at the same time, if the send is bloccante, then deadlock happens
the only assumption we can make is that if the send returns, we can alter the buffer’s content
- to keep it portable, we shouldn’t make such assumptions even if we know how the implementation we use works


accorpiamo send xke ci sono tante copie durante un send !!!

## non-blocking communication
buffered sends are considered bad for performance, because the caller has to block, waiting for the copy to take place
buffered sends are considerereed bad for performance, becau9se the caller has to block, wai10ting for the copy to take place
	by using non-blocking communication, we allow computation and communication to overlap (as MPI/NIC handles the communication), as the send returns as soon as the MPI takes notice of the send call

however, non-blocking calls don’t guarantee the altering buffer thing, and the completion of the operations has to be queried explicitly !


rendezvous: send chiede al receiver se è pronto

### $\verb |MPI_Isend()|$
 - `req`: request id, needs to be used in the `wait()`

i can use `MPI_Test()` to check for completion non-blockingly, or `MPI_Wait()` to check for completion blockingly
- several variant available
