---
related to: 
created: 2025-03-02T17:41
updated: 2025-05-13T10:44
completed: false
---
# IPC
UNIX mette a disposizione due tipi di **IPC** (**inter process communication**): 
- **FIFO** (named pipe)
- **pipe** (unnamed pipe)
lettura e scrittura dei dati su una PIPE/FIFO avvengono in maniera sequenziale(**first in, first out**)
## FIFO
la **FIFO** è un tipo di file speciale, che può essere creato per mezo delle syscall `mkfifo` o `mknod`
- è half-duplex (per avere comunicazione da entrambi i lati, di solito vengono utilizzate due FIFO)
### $\verb |int mkfifo(const char *pathname, mode_t mode)|$
la funzione `mkfifo` crea una FIFO con nome `pathname` e modalità di accesso `mode`
- una volta creata la FIFO, può essere acceduta da qualsiasi processo che abbia i diritti per l’accesso al file, in lettura/scrittura. tuttavia, le operazioni devono essere simultanee: un processo che apre la fifo in lettura rimane bloccato finchè non arriva un altro processo che la apre in scrittura (ciò per garantire la sincronizzazione tra processi)
	- è possibile anche aprire una FIFO in maniera non bloccante, passando il flag `O_NONBLOCK` alla syscall `open`
- `mkfifo` ritorna `0` in caso di successo e `-1` in caso di errore, impostando `errno` di conseguenza
- una volta creata, FIFO può essere gestita come un qualsiasi altro file (con syscall `open, read, write, close`)
## pipe
la **pipe** è una struttura dati **in memoria** (quindi non un file come la **FIFO**)