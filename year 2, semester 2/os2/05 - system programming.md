---
related to: 
created: 2025-03-02T17:41
updated: 2025-05-10T23:34
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
le funzioni `malloc()`, `calloc()` e `realloc()` usano le vere syscall per la gestione della memoria, ad esempio `nmap` (alloca memoria) e `brk` (cambia dimensione del [[03 - processi#aree di memoria|data segment]] del processo)
- le funzioni (che allocano memoria dinamicamente) ritornano un puntatore (lo indica `void *nome(parametri)`), e come visto in precedenza serve fare il casting al tipo di puntatore relativo di dato contenuto nella memoria per poter utilizzare correttamente l’aritmetica dei puntatori
### memory leakage
l’esecuzione di un programma che non gestisce correttamente la liberazione della memoria non più utilizzata può causare un aumento del consumo della memoria di sistema: questo può portare al fallimento del programma stesso, non riuscendo più ad allocare altra memoria da utilizzare, ed in generale può portare al deterioramento delle performance e del funzionamento del sistema
- ricordiamoci di usare `free()` :)
### $\verb |void *memset(void *s, int c, size_t n)|$
- assegna il valore intero `c` a `n`byte contigui dell’area di memoria puntata da `s` (cool !)
### $\verb |void *memcpy(void *dest, const void *src, size_t n)|$
- copia `n` bytes contigui a partire da `src` in `dest`
	- le due aree di memoria non possono sovrapporsi !!!
	- es: utile per duplicare rapidamente un array
### $\verb |void *nmap(void *addr, size_t length, int prot, int flags, int fd, off_t offset)|$
: crea un’area di memoria per mappare un file a partire da un indirizzo specificato, con livello di protezione indicato (`prot`)
