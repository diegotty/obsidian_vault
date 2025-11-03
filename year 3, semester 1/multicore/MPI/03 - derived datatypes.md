---
related to:
created: 2025-11-03, 15:29
updated: 2025-11-03T15:39
completed: false
---
# derived datatypes
*derived datatypes* are used to represent any collection of data items in memory
- this is achieved by storing both their type and their relative locations in memory
if a function that sends data knows this information about a collection, it can collect the items from memory to send them !
- same thing for a function that receives data: it can distribute the items into their correct destinations in memory
>[!example] esempio
>a naive implementation like the folllowing doesnt work because of the possibility of different implementation of a structure’s layout on different platforms !
>```c
>const int N = ...;
>typedef struct Point{
>	int x;
>	int y;
>	int color;
>};
>
>Point image[N];
>MPI_Send(image, N*sizeof(Point), .....);
>```
>![[Pasted image 20251103153422.png]]

## implementation
formally, it consists of a sequence of basic MPI data types, together with *a displacement* for each of the data types
## `MPI_Type_c`
```c
int MPI_Type_create_struct(
	int           count,                    // in
	int           array_of_blocklengths[],  // in
	MPI_Aint      array_of_displacements[], // in
	MPI_Datatype  array_of_types[],         // in
	MPI_Datatype* new_type_p                // out
);
```