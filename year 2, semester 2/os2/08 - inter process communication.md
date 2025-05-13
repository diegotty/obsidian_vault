---
related to: 
created: 2025-03-02T17:41
updated: 2025-05-13T11:44
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
in questo modo però, se un processo legge e scrive dalla stessa pipe, rischia di leggere i propri dati.  per questo motivo, è necessario limitare la scrittura e la lettura del processo padre e del processo figlio:
>- il padre scrive in `pipe1`  e legge da `pipe2`
>- il figlio scrive in `pipe2` e legge da `pipe1`
>
in questo modo vengono chiusi dei canali di read e write:
![[Pasted image 20250513111716.png]]
>
arrivando quindi a questo tipo di stato:
![[Pasted image 20250513111737.png]]

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
i socket consentono la comunicazione tra processi, anche residenti su macchine diverse, nel paradigma client-server. in particolare
- **client**: definisce il suo socket, e crea una connessione sul socket: una volta connesso al server, ricava un file descriptor (full-duplex) sulla connessione
- **server**: definisce il suo socket, e accetta connessioni su esso da parte di uno o più client. per ogni connessione, ricava un file descriptor (full-duplex) sulla connessione
le syscall usate nella comunicazione con socket sono:

| syscall  | descrizione                                   |
| -------- | --------------------------------------------- |
| `socket` | crea la struttura dati della socket           |
| `bind`   | associa un nome alla socket                   |
| `listen` | mette un processo in ascolto su di una socket |
| `accept` | accetta una connessione su di una socket      |
esistono diverse tipologie di socket, definite da 3 attributi:
- **domain** (o family): specifica la modalità di collegamento
	- `AF_LOCAL/AF_UNIX`: client e server devono risiedere sulla stessa macchina
	- `AF_INET`: client e server comunicano in rete con il protocollo IPv4
	- `AF_INET6`: client e server comunicano in rete con il protocollo IPv6
- **type**: specifica la semantica del collegamento
	- `SOCK_STREAM`: il flusso (di byte) è bidirezionale ed affidabile, in quanto basato su conessione (socket TCP !)
	- `SOCK_DGRAM`: supporta comunicazione datagram, senza connessione e con sequenze inaffidabili di messaggi (messaggi non ordinati) (socket UDP)
	- `SOCK_RAW`: usati per l’accesso diretto (**raw** socket) alle comunicazioni di rete (protocolli ed interfacce)
- **protocol**: specifica il protocollo usato
	 - noi consideriamo solo TCP per `SOCK_STREAM` e UDP per `SOCK_DGRAM`(ma studieremo solo TCP) . possiamo impostarlo a `0`
### anatomia server TCP
>[!info] interazione client-server
![[Pasted image 20250513112944.png]]
#### server TCP
definiamo i passaggi per creare un server socket TCP:
1. viene definito il socket, invocando `socket()`(viene creato un **unnamed socket**, cioè una struttura dati che rappresenta il socket ma al quale non è associato un indirizzo (nome))
2. si associa un nome al socket, invocando `bind()` (il socket diventa quindi un **named socket**)
3. viene definita la lunghezza della coda d'ingresso (max numero di connessioni pending) con `listen()`
4. il processo viene messo in ascolto sul socket, in attesa di una richiesta di connessione del client a cui rispondere `accept()`, che ritorna un `fd` usato per comunicare con il client
5. (arriva una richiesta da parte di un client:) viene creato un figlio, che userà `fd` per comunicare con il socket
6. il processo torna in ascolto sulla socket per una nuova connessione (punto 4)
>[!info] codice per creare un server socket TCP
>```c
>int main() {
>	int sd = socket(AF_INET, SOCKET_STREAM, 0);
>	bind(sd, ...);
>	listen(sd, MAX_QUEUED);
>	// disabilito il segnale SIGCHLD per evitare di creare zombie process
>	while (1) {
>		int client_sd=accept(sd, ...);
>		if (client_sd==-1) {
>			perror("Errore accettando connessione dal client");
>			continue;
>		}
>		if (fork()==0) { // eseguito dal client
>			// read/write (o send) su client_sd
>			close(client_sd);
>			exit(0);
>		}
>	}
>}
>```
#### client TCP
definiamo i passaggi per creare un client socket TCP:
1. viene definito il socket con `socket()`
2. si imposta una struttura dati `sin` di tipo `struct sockaddr_in`, in modo da scriverci le informazioni del server al quale ci si vuole connettere
3. ci si connette al server con la syscall `connect()`, al quale viene passato il socket e l’indirizzo del server al quale connettersi
>[!info] codice per creare un client socket TCP
>```c
>int main (){
>	int cfd;
>	int cfd = socket(AF_INET, SOCK_STREAM,0);
>	// set sockaddr_in structure
>	if (connect(cfd,….)!=0) {
>		perror(“connessione non riuscita”);
>	}
>	// read(cfd,…) e write(cfd,…) da/verso server
>	close(cfd);	
>}
>```