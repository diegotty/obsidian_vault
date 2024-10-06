il sistema operativo deve gestire le diverse computazioni che si vuole eseguire su un sistema computerizzato, in particolare:
- permettere l’esecuzione alternata di processi multipli (interleaving), avendo più processi di processori
- assegnare risorse ai processi
- permettere lo scambio di informazioni tra processi
- permettere la sincronizzazione tra processi
# processo
un processo è un’istanza di un programma in esecuzione(un programma può avere più processi)
ha le seguenti caratteristiche:
- una sequenza di istruzioni da eseguire(codice), di solito memorizzati su archiviazione di massa(mentre i processi creati dal sistema operativo di solito sono già caricati)
- uno stato corrente
- un insieme associato di risorse(dati)
## elementi di un processo
- identificatore
- stato (running, …)
- priorità
- l’attuale situazione di tutti i registri della CPU (hardware context)
- puntatori alla memoria principale
- informazioni sullo stato dell’I/O usato
- informazioni di accounting (quale utente lo esegue)
## process control block
 contiene gli elementi del processo, ed è stored nella zona di spazio riservata al kernel (altrimenti tutti i programmi potrebbero gestire i processi)
- è creato e gestito dal SO, e gli permette di gestire più processi contemporaneamente
- contiene sufficienti info per poter bloccare/far riprendere un programma
- le sue informazioni possono essere raggruppate in 3: categorie: 
### identificazione
- PID(numero identificativo e unico assegnato ad ogni processo)
- Parent PID (PID del processo padre)
- identificatore dell’utente proprietario
### stato
informazioni sullo stato del processore(hardware context):
- registi utente(quelli accessibili in assembly)
- PC
- stack pointer
- registri di stato(flags)
- PSW
### controllo
- stato del processo
- priorità
- informazioni sullo scheduling
- l’evento da attendere per tornare ad essere ready, se attualmente in attesa
## process image
l’insieme di:
- programma sorgente
- dati
- stack delle chiamate
- PCB (solo il PCB si trova nel kernel !!)
>[!figure] ![[Pasted image 20241005221533.png]]
i dati che non si trovano nel PCB si trovano nella memoria virtuale
# dispatcher
il dispatcher è un piccolo programma che decide quando sospendere un processo per farne andare un altro in esecuzione
- è sempre in memoria, e fa parte del sistema operativo


>[!figure] ![[Pasted image 20241004115509.png]]
>gestione, da parte del dispatcher, dei processi


# fasi di un processo
## creazione
in un certo istanse, in un sistema operativo ci sono n ≥ 1 processi(c’è sempre un processo master, che non può essere ucciso (se non spegnendo il computer))
i processi vengono spawnati attraverso il **process spawning**
- un processo(processo padre) crea un altro processo(processo figlio)
- si passa quindi da n a n+1 processi
## terminazione
un processo può terminare in 2 modi: 
- normale completamento(grazie a un’istruzione macchina, che genera un’interruzione per il SO)
- uccisioni (dal SO, dall’utente, o da un altro processo)
## blocked
lo stato di un processo quando sta aspettando un evento (es: ha fatto una richiesta all’I/O module, che è molto più lento) e quindi non è pronto per essere eseguito dal processore. il dispatcher non sceglie tra gli eventi blocked, che vengono accodati in una coda separata e vengono riamessi alla coda di eventi ready solo quando l’evento che stavano aspettando occorre(quindi, viene chiamata un’interruzione in cui il module comunica la fine dell’evento)
## suspended
può capitare che molti eventi siano blocked (aspettando I/O)dato che il processore è molto più veloce. tutti questi processi, caricati nella memoria primaria, stanno quindi occupando spazio prezioso nella RAM (supponiamo di avere molti processi) senza poter neanche essere eseguiti.
Lo stato blocked diventa suspeded quando il processo, per liberare memoria e non lasciare il processore inoperoso, viene swappato su disco
ci sono due tipi di suspend (due stati nel ciclo):
- blocked/suspend (swappato mentre era bloccato)
- ready/blocked (swappato mentre non era bloccato(era ready, running, blocked/suspended o addirittura new))

>[!figure] ![[Pasted image 20241004122152.png]]
### motivi per sospendere un processo

| motivo                       | commento                                                                                                                      |
| ---------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| swapping                     | SO ha bisogno di rilasciare abbastanza memoria per portarci dentro un processo ready(motivo per sospendere un processo ready) |
| interno al SO                | SO sospetta che il processo stia causando problemi                                                                            |
| richiesta utente interattiva | debugging                                                                                                                     |
| periodicità                  | il processo viene eseguito periodicamente, e può venire sospeso in attesa della prossima esecuzione                           |
| richiesta del padre          | il padre vuole sospendere l’esecuzione di un figlio per esaminarlo o modificarlo, o per coordinare l’attività tra più figli   |
# creazione di processi
 per creare un processo, il SO deve:
 - assegnargli un PID unico
 - allocargli spazio in memoria principale
 - inizializzare il PCB