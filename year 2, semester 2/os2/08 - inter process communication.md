---
related to: 
created: 2025-03-02T17:41
updated: 2025-05-13T11:14
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
la **pipe** è una struttura dati **in memoria** (quindi non un file come la **FIFO**), ed è half duplex (unidirezionali).
la creazione della pipe può essere effettuata con la syscall `pipe`, che crea due file descriptor: uno in lettura ed uno in scrittura
>[!figure]- rapp
![[Pasted image 20250513111032.png]]

>[!info] esempio di utilizzo con `fork`
>quando un processo padre crea le 2 pipe per creare un canale di comunicazione bidirezionale con il figlio creato con `fork`, il figlio eredita tutti e 4 i file descriptor
>![[Pasted image 20250513111435.png]]

i dati della pipe sono bufferizzati dal kernel finchè non sono letti (su FIFO sono memorizzati su file), e hanno quindi una dimensione massima.
- se un processo legge una pipe vuota, rimane bloccato
- se un processo scrive su una pipe piena, rimane bloccato
- una pipe viene chiusa quando tutti e due i processi hanno invocato `close`
- le operazioni di lettura (`read`) su una pipe il cui `fd` di scrittura è stato chiuso con `close` ritornano `0`
- le operazioni di scrittura (`write`) su una pipe il cui `fd` di scrittura è stato chiuso con `close` ritornano `-1` e ricevono il segnale `SIGPIPE`
### $\verb |int pipe(int pipefd[2])|$
- `pipefd[0]`: file descriptor di input
- `pipefd[1]`: file descriptor di output
## socket 