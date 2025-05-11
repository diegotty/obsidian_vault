---
related to: 
created: 2025-03-02T17:41
updated: 2025-05-11T18:50
completed: false
---
# programmazione di sistema
come abbiamo visto nel primo modulo, il **kernel** è il componente del sistema operativo che gestisce le risorse disponibili ed offre l’accesso e l’utilizzo della risorse da parte dei processi
le principali risorse gestite dal kernel sono CPU, RAM e dispositivi di I/O
le **system call** offrono **un limitato numero** (che varia con la versione del sistema) di punti di accesso al kernel: permettono ad un processo di accedere ai servizi offerti dal kernel, e di conseguenza permettono al programmatore di creare programmi che interagiscono direttamente con il kernel !
>[!info]
![[Pasted image 20250425153955.png]]
## system call
le systemcall sono definite in linguaggio C, indipendentemente dalla tecnica usata dallo specifico sistema per invocare la system call, e **per ogni** system call esiste una funzione C avente lo stesso nome
- quando viene chiamata una syscall, il processo utente invoca questa funzione usando la sequenza standard C, che a sua volta invoca il corrispondente servizio del kernel utilizzando la tecnica necessaria (…….), ad esempio  mettendo i parametri della funzione C in un registro e poi generando un interrupt per il kernel, usando istruzioni macchina
le informazioni sulle syscall si trovano nella sezione 2 del man : `man 2 nome_system_call`
## funzioni general purpose
le funzioni dall’interno delle librerire di sistema (general purpose) non sono punti di accesso ai servizi del kernel !
- possono invocare una o più syscall (es: `prinf()` usa la syscall `write`), ma possono anche non usarne nessuna (es: `strcpy()`)
le informazioni sulle funzioni general purpose si trovano nella sezione 3 del man: `man 3 nome_funzione_libreria`
>[!info] differenza tra syscall e funzioni di sistema
>- sono entrambe funzioni C
>- entrambe forniscono servizi ad un’applicazione
>- **una funzione di sistema può essere rimpiazzata, ma una syscall no !!!**
>>[!example] esempio
>la syscall `sbrk` permette di allocare la memoria, e la funzione `malloc()` è implementata usando `sbrk`. possiamo implementare la nostra `malloc()`, ma dovremo sempre utilizzare la syscal `sbrk` !
>le syscall introducono una separazione di compiti: `sbrk` alloca chunk di memoria (in kernel mode), mentre `malloc()` gestisce l’area di memoria, in user mode
![[Pasted image 20250425215449.png]]
>- le funzioni di libreria **semplificano** l’uso delle syscall, che espongono una interfaccia minimale. le funzioni forniscono funzionalità elaborate e semplificano la gestione delle strutture dati di input e output !


# syscall
## gestione errori
l’esecuzione di una syscall può interrompersi e non andare a buon fine per vari motivi, tra cui, principalmente:
- il processo che la invoca non ha sufficienti privilegi per l’esecuzione
- non ci sono sufficienti risorse per l’esecuzione
- gli argomenti in ingresso alla syscall non sono validi
>[!tip] è fondamentale controllare i valori di ritorno per rilevare e segnalare all’utente il verificarsi di errori !
e, per un corretto funzionamento del programma, **è fondamentale gestire in maniera opportuna** l’eventuale errore verificatosi
### `errno`
`errno` è una variabile globale, impostata dalle syscall che termine con un errore ad un codice specifico, relativo all’errore che si è generato durante l’esecuzione
- le syscall che terminano con successo invece, lasciano invariato `errno`
ha senso utilizzare `errno` **solo dopo essersi accertati** che la syscall abbia ritornato un valore di errore (di solito -1), altrimenti si rischia di considerare un valore di `errno` incosistente (potrebbe essere l’`errno` di una syscall precedente all’ultima)
### $\verb |void perror(const char *prefix)|$
`perror()` è una funzione della libreria standard (`stdio.h`), che stampa su `stderr` il messaggio di errore convertendo il codice di `errno` in una stringa, formata da:
$$
\text{<prefix>:<errno\_string>}
$$
- $\text{<errno\_string>}$ rappresenta il messaggio di errore in formato stringa associato al valore di `errno`
>[!example] esempio
`perror("main")` invia su `stderr`:
$\text{"main:messaggio\_errore\_mnemonico\_=\_errno"}$
### $\verb |char *strerror(int errum)|$
`strerror()` è una funzione di libreria (`string.h`) che consente di convertire un codice di errore numerico `errno`, che acquisisce come parametro di input, nella sua equivalente rappresentazione in stringa

>[!info] debug syscall
è spesso utile monitorare il comportamento di un processo relativamente all’invocazione di syscall: è possibile scaricare il comando `strace` che permette di tracciare l’invocazione di syscall da parte di un processo e i relativi parenti
## allocazione della memoria
abbiamo visto le [[04 - introduzione a C#allocazione dinamica|funzioni per l’allocazione dinamica]] di memoria **nell’heap**
- nella libreria `alloca.h` è definita la funzione `void *alloca(size_t size)`, che permette di allocare dinamicamente **memoria nello stack** (da evitare per grandi dimensioni !)
le funzioni `malloc()`, `calloc()` e `realloc()` usano le vere syscall per la gestione della memoria, ad esempio `nmap()` (alloca memoria) e `brk()` (cambia dimensione del [[03 - processi#aree di memoria|data segment]] del processo)
- le funzioni (che allocano memoria dinamicamente) ritornano un puntatore (lo indica `void *nome(parametri)`), e come visto in precedenza serve fare il casting al tipo di puntatore relativo di dato contenuto nella memoria per poter utilizzare correttamente l’aritmetica dei puntatori
>[!warning] memory leakage
l’esecuzione di un programma che non gestisce correttamente la liberazione della memoria non più utilizzata può causare un aumento del consumo della memoria di sistema: questo può portare al fallimento del programma stesso, non riuscendo più ad allocare altra memoria da utilizzare, ed in generale può portare al deterioramento delle performance e del funzionamento del sistema
>- ricordiamoci di usare `free()` :)
### $\verb |void *memset(void *s, int c, size_t n)|$
- assegna il valore intero `c` a `n`byte contigui dell’area di memoria puntata da `s` (cool !)
### $\verb |void *memcpy(void *dest, const void *src, size_t n)|$
- copia `n` bytes contigui a partire da `src` in `dest`
	- le due aree di memoria non possono sovrapporsi !!!
	- es: utile per duplicare rapidamente un array

## memory-mapped I/O
### $\verb |void *mmap(void *addr, size_t length, int prot, int flags, int fd, off_t offset)|$
 è inclusa in `<sys/mman.h>`, e crea un’area di memoria per mappare un file a partire da un indirizzo specificato (o non)
 è usata per mappare file o altre risorse presenti sul disco in memoria
 - in questo modo, lettura/scrittura dal/sul buffer risultano in lettura/scrittura sul disco, senza accedere al disco
 - restituisce l’indirizzo di partenza della regione mappata (buffer) se OK, `MAP_FAILED` se errore !
 
 argomenti:
 - `addrs` è l’indirizzo iniziale dell’area di memoria in cui vogliamo mappare il file. se `addr = 0`, sceglie il sistema
 - `fd` è il file descriptor (in quanto il file va aperto prim)
 - `len`è il numero di byte da trasferire
 - `off` indica l’offset nel file da cui iniziare a trasferire
 - `prot` indica il livello di protezione:

| `prot`       | description               |
| ------------ | ------------------------- |
| `PROT_READ`  | region can be read        |
| `PROT_WRITE` | region can be written     |
| `PROT_EXEC`  | region can be executed    |
| `PROT_NONE`  | region cannot be accessec |
-  i valori di `flag` possono essere:
	- `MAP_SHARED`: le operazioni sulla memoria modificano il file (R/W)
	- `MAP_PRIVATE`: viene creata una copia privata del mapped file e solo questa viene modificata
>[!tip] malloc()
`mmap()` è una syscall, e viene usata spesso da `malloc()` per grandi allocazioni

 >[!info] rappresentazione
 ![[Pasted image 20250511172930.png]]
 i file vengono mappati nella immagine del processo !


### $\verb|int msync (void *addr, size_t len, int flags)|$
permette di scrivere sul file le modifiche in memoria, e funziona solo se il flag passato a `mmap()` è `MAP_SHARED`
### $\verb|int munmap(void *addr, size_t len)|$
permette di *unmap*-are una regione di memoria precedentemente mappata con `mmap()`
- è l’equivalente di `free()` per la memoria mappata
## gestione file I/O
ricordiamo che:
- quando viene aperto un file mediante `open()`, viene creato un **file descriptor**
- le operazioni `write()` e `read()` su file operano mediante syscall
- è buona regola chiudere esplicitamente i file invocando `close()`
### file descriptor
un **file descriptor** è il riferimento ad un file aperto: è rappresentato da un intero piccolo, generato sequenzialmente dal valore 0. per default, ogni processo ha i associato i seguenti file descriptor:
- `0: stdin`
- `1: stdout`
- `2: stderr`
>[!info]- file descriptor
![[Pasted image 20250511175258.png]]

quando un file viene chiuso, il suo file descriptor viene liberato e può essere riutilizzato. (è quindi possibile aprire uno stesso file ed ottenere file descriptor diversi)

esistono dei flag associati ad ogni file descriptor:
- **file status flags**: associati allo stato di un file, e sono condivisi tra tutti i file descriptor che sono stati ottenuti per duplicazione da un unico file descriptor. si dividono in 3 categorie:
	- **modalità di accesso**: `read,write, read&write`, vengono specificati in `open()`, e non possono essere modificati una volta aperto il file
		- `O_RDONLY, O_WRONLY, O_RDWR`
	- **di apertura**: definiscono il comportamento di `open()` e non vengono mantenuti (quindi altri parametri di `open()`)
		- `O_CREAT`, `O_EXCL`(quando specificato insieme a `O_CREAT`, genera errore se il file esiste già)
	- **modalità operative**: definiscono il comportamento delle operazioni `read()` e `write()`. vengono sempre specificati in `open()`, e possono essere modificati anche dopo l’apertura del file
		- `O_APPEND`, `O_SYNC`(scrittura sincrona: la call ritorna solamente quando la scrittura dei dati nel file è terminata),  `O_TRUNC`(se il file esiste, e consente la scrittura, viene troncato dalla posizione 0)
- **file descriptor flags**: associati ai singoli file descriptor, descrivono proprietà e comportamento delle operazioni effettuate sul file
	- alcuni flag sono definiti solo per alcuni tipi di file speciali
>[!info] rappresentazione dei flag
i flag sono rappresentati mediante maschere di bit (es: `MACRO1 = 010000000`, `MACRO2=00010000`), e possono essere combinati mettendo in OR le maschere (es: `MACRO1 OR MACRO2 = 01010000` (`MACRO1` e `MACRO2` sono settati))
j

studiamo ora le syscall per la gestione dei file
### $\verb |int open(const char *pathname, int flags, mode_t)|$
restituisce `-1` se c’è stato un errore, altrimenti restituisce il file descriptor
- il parametro flags corrisponde ai file status flags (ed è equivalente al parametro `mode` di `fopen()
- `mode`indica le modalità di creazione del file (esiste una segnatura di `open()` senza questo parametro)

>[!info] differenza tra `open` e `fopen`
>- `fopen` restituisce un puntatore ad un FILE object, una struttura che contiene le informazioni richiesta dalle librerie I/O standard:
>	- il file descriptor
>	- un puntatore a un buffer (?) per lo stream
>	- la dimensione del buffer
>	- un conto del numero di caratteri attualmente presenti nel buffer
>	- una error flag
>	- etc
### $\verb|ssize_t read(int fd, void *buf, size_t count)|$
permette di leggere un file: restituisce `-1` se errore, altrimenti il numero di byte letti, che può essere < `count`se si raggiunge `EOF`
- `fd`: file descriptor
- `buf`: puntatore all’area di memoria in cui memorizzare i byte letti
- `count`: numero di byte da leggere
>[!info] differenza tra `read` e `fread`
>- `fread` legge da uno stream di tipo FILE, è bufferizzata e richiede la dimensione del tipo di dato da leggere. ritorna un FILE object
> - `read` non è bufferizzata e lavora sui byte indipendentemente dal tipo di dati in essi contenuto
### $\verb |ssize_t write (int fd, const void *buf, size_t count)|$
permette di scrivere un file: restituisce `-1` se errore, altrimenti il numero di byte scritti, che può essere < `count`se `write` viene interrotta (per esempio da un segnale (?))
- `fd`: file descriptor
- `buf`: puntatore all’area di memoria da cui leggere i byte (dichiarata `const` per non essere modificata dalla funzione)
- `count`: numero di byte da leggere
### $\verb |int close(int fd)|$
chiude il file descriptor `fd`: restituisce `-1` in caso di errore, altrimenti `0`
- nel caso venga chiuso l’ultimo file descriptor che fa riferimento ad un file rimosso, allora il file viene cancellato
## misc. syscall
### $\verb |int dup(int oldfd)|$
duplica il file descriptor (`oldfd`) e restituisc