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
in un certo istanse, in un sistema operativo ci sono n ≥ 1 proce