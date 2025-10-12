---
related to:
created: 2025-03-02T17:41
updated: 2025-10-12T18:06
completed: false
---
# introduction
MPI is a library used by distributed-memory systems
it used the **SPMD** ((**single-program multiple-data**) parallel programming computing model, where one program is compiled and it gets executed by multiple processes.
we use if-else statements to specify what each process must do (similarly to what happens when you fork a process)
 - as the systems are distributed-memory, communications happen through message passing

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

>[!info] misc info
identifiers start with `MPI_`, and the first letter following underscore is uppercase

	
the return value of MPI functions is `int`, and it always returns a code that indicates an error

>[!info] compiling and execution
>```bash
>mpicc -g -wall -o mpi_hello mpi_hello.c
>```
>- `-g` produces debug information
>- `-Wall` turns on all warnings
>it is advised to use some kind of debugger (gdb (GNU debugger, fully terminal: ddd is its graphical frontend) / valgrind (old))
>```bash
>mpiexec -n <number of processes> <executable>
>```
>- `-n` specifies the number of processes that will execute the program
>```bash
>mpiexec -n 4 ./test: -n ddd ./test : -n 1 ./test
>```
runs the 5th process under the ddd debugger, and the other 5 normally

each process is identified by a **rank**, that starts from `0` to `p-1`
## communicators
a communicator is a collection of processes that can send messages to each other
- `MPI_Init()` defines a standard communicator, that consists of all the processes created when the program is started: `MPI_COMM_WORLD`

>[!warning] ranks and communicators
>id process is relative to what communicator it is in ! (the rank in `MPI_Comm_rank()`)

>[!info] return values
> all MPI functions return an `int`, which is used to communicate a state/error

### functions
- `int MPI_Comm_size(MPI_Comm comm, int* comm_sz_p)`
	- return the size (cardinality) of the communicator `comm` in `comm_sz_p`
- `int MPI_Comm_rank(MPI_Comm comm, int* my_rank_p)`
	- returns the rank of the process making the call, relative to the communicator `comm`, in `my_rank_p`
## message sending
>[!info] sending order
MPI requires that messages be **nonovertaking**: if process $q$ sends two messages to process $r$, the first message sent by $q$ must be available to $r$ before the second message
>- however, there is no restriction on the arrival of messages sent from different processes
### functions
- `int MPI_Send(void* msg_buf_p, int msg_size, MPI_Datatype msg_type, int dest, int tag, MPI_Comm communicator)`
	- `msg_buf_p`: start address of the block of memory to be sent
	- `msg_size`: number of elements of the message (of type `msg_type`)
	- `msg_type`: type of data being sent (parallel to int, char, float, …., but we pass a MPI enum value. for custom types (structs) , a new macro needs to be made !)
	- `dest`: rank of the receiver
	- `tag`: needs to match the receiver’s tag
	- `communicator`: communicator to be “used”
- `int MPI_Recv(void* msg_buf_p, int buf_size, MPI_Datatype buf_type, int source, int tag, MPI_Comm communicator, MPI_Status* status_p)`
	- `msg_buf_p`: start address of the memory to be filled with the message
	- `source`: rank of the sender (special values like `MPI_any_RANK` exist !)
	- `tag`: has to match with sender’s `tag`
	- `status_p`: info on what happened on the receive (ex: rank of the sender if `MPI_ANY_RANK` or `MPI_ANY_SOURCE` was used). its an struct that contains fields like `MPI_SOURCE`, `MPI_TAG`, `MPI_ERROR`
- `int MPI_Get_count(MPI_Status* status_p, MPI_Datatype type, int* count_p)`
	- given the status (which identifies the communication), and the type of the data recevied, it fills `count_p` with the number of `type` received
### data types

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

a message is **successfully** received if:
- `recv_type = send_type`
- `recv_buf_sz >= send_buf_sz`
also, a  receiver can get a message withouth knowing:
- the amount of data in the message
- the sender of the message (`MPI_ANY_SOURCE`)
- the tag of the message (`MPI_ANY_TAG`)
## issue with send and receive
the exact behaviour of send and receive functions is determined by the MPI implementation, but !! even if you nkow your MPI implementation, stick to what the defined standard says ! don’t make implementation-specific assumptions, otherwise code will not be portable
- **send**: when the send function returns, the sender does not know  if the message got delivered successfully, or if the message was sent at all. it doesn’t even know if its still in its memory, waiting to be sent. **however**, after the send function returns, the sender can alter the content of the buffer sent, as MPI guarantees the message will not be lost (MPI has a buffer where it stores **some** messages that were sent but still not received by anyone (as nobody called the receive function with the matching parameters)
	- however, this cant be done with very big messages, and in that cases, it behaves differerntly:
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

slide 33