---
related to: 
created: 2025-03-02T17:41
updated: 2025-05-14T08:15
completed: false
---
# multithreading
in un’**applicazione tradizionale**, il programmatore definisce un unico flusso di esecuzione delle istruzioni, che segue la logica del programma. quando il flusso di esecuzione arriva ad eseguire la funzione `exit()`, l’applicazione termina
le **applicazioni multithread** consentono al programmatore di definire diversi flussi di esecuzione: 
- ciascun flusso di esecuzione condivide le strutture dati principali dell’applicazione (esiste quindi della memoria condivisa)
- ciascun flusso di esecuzione procede in modo **concorrente ed indipendente** dagli altri flussi
- l’applicazione finisce solo quando tutti i flussi di esecuzione vengono terminati !
>[!info] processi e threads
![[Pasted image 20250514075556.png]]
> un processo per un’applicazione monothread è costituito da:
> - codice
> - strutture dati (variabili globali in memoria, heap)
> - file aperti
> - contenuto dei registri della CPU (process context)
> - posizione e contenuto dello stack 
>
>in una applicazione multithread alcune risorse sono comuni e condivise tra tutti i thread:
>- codice
>- strutture dati
>- file aperti
>
>mentre altre risorse sono private per ciascun thread:
>- contenuto dei registri della CPU
>- posizione e contenuto dello stack

| processi                                                                                       | thread                                                                                                                                   |
| ---------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| istanze di un programma in esecuzione (heavyweight)                                            | componente di un processo e più piccola unità di esecuzione (lightweight)                                                                |
| cambio di contesto (context switching) richiede interazione con SO                             | context switch non richiede interazione con SO                                                                                           |
| ogni processo ha il suo spazio di memoria                                                      | usano la memoria del processo a cui appartengono                                                                                         |
| richiedono più risorse di sistema                                                              | richiedono meno risorse di sistema                                                                                                       |
| difficili da creare                                                                            | facili da creare                                                                                                                         |
| comunicazione tra processi lenta in quanto ogni processo ha un differente indirizzo di memoria | comunicazione tra thread è veloce in quanto i thread condividono lo stesso indirizzo (e area) di memoria del processo a cui appartengono |
| ogni processo esegue indipendentemente (isolato)                                               | ogni thread può leggere, scrivere, modificare dati di altri thread                                                                       |
| grazie flavio                                                                                  | sperandeo.it                                                                                                                             |

>[!info]- e finalmente la foto vista in almeno altri 3 corsi:
![[Pasted image 20250514075815.png]]

>[!tip] perchè le applicazioni multithread sono importanti ?
**è difficile scrivere applicazioni tradizionali che sfruttino a fondo il parallelismo interno del calcolatore**, che comprende:
>- DMA
>  - **hyperthreading**: supporto a diversi flussi di esecuzione, ciascuno con un proprio insieme di registri, che si alternano sulle unità funzionali della CPU
>- **multicore**: diversi core di calcolo integrati sullo stesso chip e che condividono alcune risorse harware, quali cache L2, MMU, etc..
>- **multiprocessori**: diverse CPU integrate sulla stessa scheda madre

>[!example] esempio di applicazione multithread
un browser web potrebbe essere costituito dai seguenti thread:
>- thread principale di controllo dell’applicazione
>- thread per l’interazione con l’utente
>- thread per il rendering delle pagine in formato HTML
>- thread per la gestione dei trasferimenti di pagine e file dalla rete
>- thread per l’esecuzione dei frammenti di script integrati nelle pagine web
>- thread per l’esecuzione dei programmi java, flash, etc…
## vantaggi del multithreading
il multithreading ha numerosi vantaggi:
- **riduzione del tempo di risposta**: anche se una parte dell’applicazione è bloccata in attesa di eventi esterni, un altro thread può essere eseguito per interagire con l’utente o gestire altri eventi !
- **migliore condivisione delle risorse**: tutti i thread di una applicazione condividono delle risorse (strutture dati in memoria e file aperti), e la comunicazione tra i thread è immediata
- **maggiore efficienza**: rispetto ad una applicazione costituita da più processi cooperanti, l’applicazione multithread è più efficiente, perchè **il SO gestisce i thread più rapidamente**
	- in linux, creare un thread richiede $\frac{1}{10}$ del tempo richiesto per creare un nuovo processo
- **maggiore scalabilità**: i thread possono sfruttare in modo implicito il parallelismo interno del calcolatore
## implementazione di applicazioni multithread
esistono principalmente 2 componenti in un implementazione delle applicazioni multithread: 
- implementazione a livello utente
- implementazione a livello kernel