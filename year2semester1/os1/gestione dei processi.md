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
 contiene gli elementi del processo, ed è stored nella zona di spazio riservata al kernel (altrimentti tutti i programmi potrebbero gestire i processi)
- è creato e gestito dal SO, e gli permette di gestire più processi contemporaneamente
- contiene sufficienti info per poter bloccare/far riprendere un programma

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
lo stato di un processo quando sta aspettando un evento (es: ha fatto una richiesta all’I/O module, che è molto più lento) e quindi non è pronto per essere eseguito dall CPU. il dispatcher non sceglie tra gli eventi blocked, che vengono accodati in una coda separata e vengono riamessi alla coda di eventi ready solo quando l’evento che stavano aspettando occorre(quindi, viene chiamata un’interruzione in cui il module comunica la fine dell’evento)
## bloced