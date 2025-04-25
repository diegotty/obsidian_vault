---
related to: 
created: 2025-03-02T17:41
updated: 2025-04-25T21:56
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
