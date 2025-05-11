---
related to: 
created: 2025-03-02T17:41
updated: 2025-05-11T21:20
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
	 - descrittori
## zombie
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
