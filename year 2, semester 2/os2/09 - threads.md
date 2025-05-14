---
related to: "[[threads]]"
created: 2025-03-02T17:41
updated: 2025-05-14T09:08
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
e ciò che caratterizza le applicazioni multithread è la relazione tra thread utente e thread kernel
 - la differenza tra thread utente e thread kernel è la diversa modalità di esecuzione: user mode o kernel mode
### modello “da molti a 1”
nel modello “**da molti a 1**”, detta anche implementazione **a livello utente** o **green threads** (corrisponde alla implementazione con [[threads#ULT (User Level Thread)|user level threads]]):
- una applicazione multithread è costituita da un **singolo processo** nel SO
- a diversi thread utente corrisponde un singolo thread kernel (quindi “da molti a 1”)
- il nucleo del SO non è coinvolto nella gestione dei flussi dell’applicazione
- l’applicazione, eventualmente usando una libreria di sistema, gestisce autonomamente i thread utente: schedula i vari flussi di esecuzione e si occupa della gestione di stack UM(user mode) e i contesti dei vai thread
- se un thread invoca una chiamata di sistema bloccante, il processo (e quindi tutti i thread utente) vengono bloccati
- è impossibile sfruttare in modo implicito il parallelismo interno del calcolatore
### modello “da 1 a 1”
nel modello “**da 1 a 1**”, detta anche implementazione **a livello kernel** o **native threads** (corrisponde alla implementazione con [[threads#KLT (Kernel Level Thread)|kernel level thread]]):
- ciascun thread utente dell’applicazione corrisponde ad un singolo thread kernel (quindi “da 1 a 1”)
- il nucleo del SO si occupa della gestione e schedulazione dei thread kernel (perciò gestisce anche i thread utente, dato che sono 1 a 1 ?)
- l’applicazione utilizza le API definita in una libreria di sistema: si occupa di creare/distruggere i thread utente e di gestire la comunicazione e sincronizzazione tra essi
	- la libreria implementa i servizi richiesti dall’applicazione invocando opportune chiamate di sistema
- ciascun thread utente può invocare chiamate di sistema bloccanti senza bloccare gli altri thread
- l’applicazione sfruttra in modo implicito il parallelismo interno del calcolatore
>[!info] differenza nell’implementazione
![[Pasted image 20250514084521.png]]

| user level                                                         | kernel level                                                                        |
| ------------------------------------------------------------------ | ----------------------------------------------------------------------------------- |
| SO inconsapevole dei threads                                       | SO consapevole dei threads                                                          |
| facile implementazione dei thread                                  | difficile implementazione dei threads                                               |
| context switch veloce, senza supporto hardware                     | context switch e costoso a livello di tempo e risorse, e richiede supporto hardware |
| una chiamata bloccante fatta da un thread blocca l’intero processo | una chiamata bloccante fatta da un thread non blocca l’intero processo              |
| ss. POSIX, Java threads                                            | es. Windows, Solaris                                                                |
### modello “da molti a molti”
nell’implementazione “**da molti a molti**”:
- gli $n_{u}$ thread utente della applicazione corrispondono a $n_{k}$ thread kernel (con $n_{k} \leq n_{u}$)
- il nucleo del SO di occupa della gestione e schedulazione dei thread kernel (quindi non gestisce direttamente i thread utente)
- l’applicazione utilizza le API definite in una librerira di sistema per:
	- definire il numero $n_{k}$ di thread kernel
	- creare e distruggere i thread utente
	- mappare i thread utente sui thread kernel
- la libreria di sistema, a sua volta:
	- usa chiamate di sistema per gestire i thead kernel
	- gestisce la schedulazione e lo stack UM dei thread utente mappati sullo stesso thread kernel
	- gestisce comunicazione e sincronizzazione tra thread (utente immagino)

>[!info] tutti i maggiori SO oggi supportano i thread nativi (quindi le implementazioni “da 1 a 1” e “da 1 a molti”)
la scelta del modello tra i due appena menzionati dipende dalla libreria thread utilizzata, non dal SO !
>- linux, MS windows, macOS tendono ad adottare il modello “da uno a uno”
>
>invece le librerie che implementano i thread a livello utente possono essere utilizzato con qualunque SO

## librerie dei thread
come abbiamo visto, generalemente il programmatore utilizza una libreria di sistema per realizzare una applicazione multithread
- le API offerte dalla libreria non sono direttamente correlate con la tipologia di thread utilizzata ! (?)
- alcune librerie sono specifiche per un determinato SO e tipologia di thread (es: libreria per i thread delle API win32 (?))
- alcune librerie di thread invece sono specifiche di un linguaggio ad alto livello (es: la libreria di thread di Java), e la libreria utilizza una libreria di thread di più basso livello
### pthreads
la libreria `pthreads` (POSIX threds) è definita dallo standard POSIX, che definisce le API, ma non quale debba essere la loro implementazione in uno specifico SO
>[!info] lore
in linux sono coesistite tre diverse implementazioni:
> - `LinuxThreads`: prima implementazione, basata sul modello “da uno a uno”. non più supportata
>- `NGPT`(nex generation POSIX threads):  svluppata da IBM sul modello “da molti a molti”. non più supportata
>- `NPTL`(nex generation POSIX threads): ultima implementazione, più efficiente, più aderente allo standard, basata sul modello “da uno a uno”
>
>**oggi in linux si utilizza esclusivamente `NPTL` !**

