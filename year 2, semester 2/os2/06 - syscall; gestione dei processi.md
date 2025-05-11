---
related to: 
created: 2025-03-02T17:41
updated: 2025-05-11T22:20
completed: false
---
# gestione dei processi
## creazione
`init` è il processo 0 (con `pid=1`), padre di tutti i processi in esecuzione del sistema.
da `init` vengono creati tutti i processi, mediante la syscall `fork()`, che consisten nella duplicazione del processo, e della creazione di relazioni **padre-figlio**!

>[!example]- esempio
notiamo come in questo caso, il terminale su cui è stato lanciato `pstree` è figlio di i3, e `pstree` è figlio di `zsh`
![[Pasted image 20250511210433.png]]

ogni processo nella gerarchia fa riferimento al processo padre:
- alla nascita, eredita codice e parte dello stato dal processo padre
- quando termina/muove, ritorna l’exit status al processo padre
e se il processo padre termine/muore prima del processo figlio, il processo figlio vene adottato da `init`
>[!info] rappresentazione
![[Pasted image 20250511210739.png]]
`PPID` contiene proprio il `pid` del processo padre
### $\verb |pid_t fork(void)|$
la syscall `fork()` crea un nuovo processo che è la copia del processo chiamante, a parte alcune strutture dati (es: il `pid`)
- una chiamate `fork()` ritorna 2 volte per ogni volta che viene chiamata:
	- una volta ritorna al proceso che l’ha invocata
	- l’altra volta ritorna al nuovo processo che è stato generato dall’eseguzione di `fork()`
in caso di errore ritorna `-1` al chiamante, e non viene creato nessun processo figlio

in particolare, un processo figlio creato con `fork`,
 - **eredita dal padre**:
	 - `uid`, `euid`, `gid`, `egid`
	 - groups id
	 - working directory
	 - ambiente del processo
	 - descrittori dei file
	 - terminale di controllo
	 - memoria condivisa
- **non eredita dal padre**:
	- `pid`: ogni figlio ha il suo proprio `pid`
	- `ppid`: nel figlio, il `ppid` è uguale al `pid` del padre (non al `ppid` del padre)
	- i timer: ogni processo ha i propri timer
	- record lock / memory lock: due processi non possono avere gli stessi lock
	- contatori risorse: i contatori dell’utilizzo delle riorse sono impostati a zero per il figlio
	- coda dei segnali: la coda dei segnali in attesa viene svuotata nel figlio
## terminazione
### zombie
un processo **zombie** è un processo terminato, ma il cui PCB è mantenuto dalla **process table** del kernel per permettere al processo padre di leggere l’exit status di esso
>[!warning] se il padre di un processo muore/termina mentre il figlio è in stato zombie, esso rimane in stato zombie !!
## syscall per controllo processi
richiedono `<sys/types.h>` e `<unistd.h`

| syscall                 | azione                                                                        | risultato                                               |
| ----------------------- | ----------------------------------------------------------------------------- | ------------------------------------------------------- |
| `pid_t getpid(void)`    | ritorna `pid` del processo chiamante                                          | termina sempre con successo                             |
| `pid_t getppid(void)`   | ritorna `pid` del padre del processo chiamante                                | ””                                                      |
| `uid_t getuid(void)`    | ritorna `uid` (utente proprietario) del processo chiamante                    | ””                                                      |
| `uid_t geteuid(void)`   | ritorna l’`euid` (effective uid) del processo chiamante                       | “”                                                      |
| `int setuid(uid_t uid)` | imposta l’`euid` del processo chiamanete a `uid` (parametro)                  | ritorna `-1` in caso di errore, `0` in caso di successo |
| `int setgid(uid_t uid`  | imposta l'`egid` (effective `gid`) del processo chiamante a `gid` (parametro) | “”                                                      |
vediamo le syscall utili per terminare un proceso:
### $\verb |void _exit (int status)|$
`_exit` è una syscall che termina il processo che la invoca, senza invocare handler. in particolare:
- chiude tutti i file descriptor
- i child vengono ereditati dal processo con `pid=1` (`init`)
- invia il segnale `SIGCHILD` al processo padre
- ritorna `status` e l’exit status al processo padre
### $\verb |void exit (int status)|$
`exit` è una funzione definita in `<stdlib.h>` che:
- invoca sono stati registrati nel processo con `atexit` e `on_exit`
- chiude tutti i file descriptor, svuota gli stream `stdio` e li chiude
- termina il processo
- ritorna `status & 0377` al padre \\add info
possono essere passate, come parametro per `status`, `EXIT_SUCCESS` e `EXIT_FAILURE` (autoesplicative)
>[!info] come viene lanciato e terminato un programma di C
![[Pasted image 20250511212852.png]]

### $\verb |void abort(void)|$
`abort`è una funzione definita in `<stdlib.h>`, che invia il segnale `SIGABRT` per il processo chiamante. quando viene lanciato `SIGABRT`, il processo termina in modo anormale (non vengono chiusi file o svuotati buffer)
- `SIGABRT`può essere intercettata e gestita
- per mandare `SIGABRT`, `abort` usa una syscall

guardiamo ora delle funzioni usate per:
- attendere cambiamenti di stato di un figlio del processo chiamante, e ottenere informazioni a riguardo
avviene un cambimento di stato quando:
- un processo figlio è terminato
	- in particolare, se un figlio viene terminato, `wait/waitpid` permettono al sistema di rilasciare correttamente le risorse associate al figlio (altrimenti, senza attesa, il figlio terminato rimane in uno stato zombie)
- un processo figlio viene arrestato da un segnale (`abort` !)
- un processo figlio viene ripristinato da un segnale
se un figlio ha già cambiato stato, le chiamate ritornano immediatamente (cambiato rispetto a cosa ? ) altrimenti si bloccano fino a quanto un figlio non cambia stato o un gestore di segnale non interrompe la chiamata
### $\verb |pid_t wait(int *status)|$
la syscall `wait` sospende l’esecuzione del processo chiamante fino a quando uno dei suoi figli non termina (ritorna `-1`) in caso di errore
### $\verb |pid_t waitpid(pid_t pid, int *status, int options)|$
la syscall `waitpid` sospende l’esecuzione del processo chiamante fino a quando un figlio, specificato dall’argomento `pid`, ha cambiato stato
il valore di `pid` può essere:
- `< -1`: attesa di qualunque figlio il cui `gid` del processo sia uguale al valore assoluto di `pid` (?)
- `-1`: aspettare qualunque processo figlio
- `0`: aspettare qualunque processo figlio il cui `gid` sia uguale a quello del processo chiamante
- `> 0`: aspettare il figlio il cui `pid` sia uguale al valore di `pid` (argomento)
il comportamento di default di `waitpid` è di aspettare finchè i figli non terminano, ma tale comportamento è modificabile attraverso l’argomento `options`:
- `WNOHANG`: torna immediatamente se nessun figlio è uscito
- `WUNTRACED`: torna anche se un figlio si è arrestato (ma non è tracciato attraverso la syscall `ptrace`). in questo caso viene fornito anche lo stato del figlio arrestato, anche se l’opzione non è specificata
- `WCONTINUED`: torna anche se un figlio arrestato è stato riesumato inviando `SIGCONT`
(i valori di sopra possono essere messi in OR)
>[!info] l’argomento `status`
l’argomento `status` viene usato da `wait/waitpid` per memorizzare il valore dello stato del processo figlio (o `NULL`)
in particolare, `status` può essere verificato con le seguenti macro: 
>- `WIFEXITED`: ritorna true se è terminato normalmente
>- `WEXTISTATUS`: ritorna l’exit status del figlio
>- `WIFSIGNALED`: ritorna true se è terminato per la ricezione di un segnale
>- `WTERMSIG`: ritorna il numero del segnale che ha causato la terminazione
>- `WCOREDUMP`: ritorna true se ha generato un core dump
>- `WIFSTOPPED`: ritorna true se il processo è in stato stopeed per la ricezione di un segnale
>- `WSTOPSIG`: ritorna il numero del segnale che ha causato lo switch in stato di stopped
>- `WIFCONTINUED`: ritorna true se il processo ha continuato per la ricezione di un segnale `SIGCONT`

guardiamo ora delle syscall e funzioni libreria che permettono la sostituzione dell’immagine del processo che la invoca con una nuova immagine
- la famiglia di funzione `exec` contiene diverese syscall (7 credo), ognuna con funzione minimalmente differente/argomenti diversi, noi vedremo la principlae
### $\verb |int execve(const char *filename, char * const argv[], char *const envp[])|$
la syscall `execve` sostituisce l’immagine del processo con quella contenuta in `filename` (che è un file binario, oppure )