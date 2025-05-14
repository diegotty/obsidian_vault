---
related to: 
created: 2025-03-02T17:41
updated: 2025-05-14T08:05
completed: false
---
# multithreading
in un’**applicazione tradizionale**, il programmatore definisce un unico flusso di esecuzione delle istruzioni, che segue la logica del programma. quando il flusso di esecuzione arriva ad eseguire la funzione `exit()`, l’applicazione termina
le **applicazioni multithread** consentono al programmatore di definire diversi flussi di esecuzione: 
- ciascun flusso di esecuzione condivide le strutture dati principali dell’applicazione (esiste quindi della memoria condivisa)
- ciascun flusso di esecuzione procede in modo **concorrente ed indipendente** dagli altri flussi
- l’applicazione finisce solo quando tutti i flussi di esecuzione vengono terminati !
>[!info] rappresentazione
![[Pasted image 20250514075556.png]]
>
e finalmente la foto vista in almeno altri 3 corsi:
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
- **riduzione del tempo di risposta**:
- **migliore condivisione delle risorse**:
- **maggiore efficienza**:
- **maggiore scalabilità**: