---
created: 2024-11-25
related to: 
updated: 2024-12-08T13:23
---
per i SO moderni è essenziale supportare più processi in esecuzione che sia:
- multipogrammazione(un solo processore)
>[!figure] 
![[Pasted image 20241125104126.png]]
quando c’è un solo processore, i processi si alternano nel suo uso (**interleaving***)
- multiprocessamento (+ processori)
>[!figure]
![[Pasted image 20241125104237.png]] 
i processi si alternano nell’uso di un processore(**interleaving**), e possono sovrapporsi nell’uso dei vari processori
- computazione distribuita (cluster, più computatori)

un grosso problema da affrontare è la **concorrenza**, ovvero gestire il modo in cui i processi interagiscono

la concorrenza si manifesta nelle seguenzi occasioni:
- applicazioni multiple (bisogna condividere il tempo di calcolo)
- applicazioni strutturate per essere parallele
- struttura del sistema operativo (quando sli stessi SO sono costituiti da svariati processi o thread in esecuzione (es: in Linux ci sono i kernel thread))
## terminologia 
-  **operazione atomica**: una sequenza indivisibile di comandi (il dispatcher non può togliere il processore ad un processo se esso sta eseguendo una operazione atomica). inoltre nessun altro processo può vedere uno stato intermedio della sequenza (o interrompere, appunto, la sequenza)
 - **sezione critica**: parte del codice di un processo in cui c’è un accesso **esclusivo** ad una risorsa (nessun altro processo che voglia accedere in modo esclusivo alla stessa risorsa può farlo)
- **mutua esclusione**: requisito che impone che un solo processo sia un una data sezione critica (per risorsa condivisa). cioè quando due processi richiedono di accedere ad una stessa risorsa in comune, che però è fatta in modo tale che solo un processo alla volta possa accedere ad essa. in questa situazione, i processi si escludono mutualmente: se un processo accede a tale risorsa, esclude l’altro dall’utilizzo dal fare lo stesso
- **race condition**(corsa critica): violazione della mutua esclusione ( più di un processo riesce ad accedere alla stessa sezione critica)

- **deadlock(stallo)**: situazione in cui due o più processi non possono procedere con la prossima istruzione, perchè ciascuno attende l’altro
- **livelock(stallo attivo)**: situazione in cui due o più processi cambiano continuamente il proprio stato, l’uno in risposta dell’altro, senza fare alcunchè di “utile” (quindi sempre in stallo)
- **starvation(morte per fame)**: quando un processo, pur essendo ready, non viene mai scelto dallo scheduler
## difficoltà della concorrenza
- **LA** difficoltà della concorrenza è che non si può fare nessuna assunzione sul comportamento dei processi, e neanche su come funzionerà lo scheduler (però devo avere una soluzione che funziona per tutti i casi)
- un’altro grande problema è la condivisione di risorse (es: una stampante, o anche semplicemente 2 thread the accedono alla stessa variabile globale)
- bisogna anche gestire l’allocazione delle risorse condivise:
	- ad esempio, un processo potrebbe richiedere un dispositivo I/O e poi essere rimesso in `ready` prima di poterlo usare. quel dispositivo I/O va considerato `locked` oppure no ?
- spesso il manifestarsi di un errore dipende dallo scheduler e dagli altri processi presenti, quindi rilanciare l’ultimo processo spesso **non** riproduce lo stesso errore

### esempio facile
```
/* chin e chout sono variabili globali */
void echo(){
	chin = getchar();
	chout = chin;
	putchar(chout);
}
//questo codice prende un carattere in input e lo stampa sullo schermo (esempio)
```

>[!example] esempio su un processore
se il codice venisse eseguito da 2 processi su un solo processore, lo scheduler protrebbe decidere di assegnare il processore in questo modo:
![[Pasted image 20241125111107.png]]
in questo caso, il carattere preso in input dal processo P1 viene preso, in quanto chin è globale e viene sovrascritta prima che P1 possa salvarla (e stamparla)

>[!example] esempio su più processori
![[Pasted image 20241208125616.png]]
>come si può notare, il problema potrebbe non risolversi anche se si usano 2 processori

## restrizione all’accesso singolo
si fa in modo che la funzione `echo()` diventi atomica, cioè: 
- dentro la funzione ci può entrare solo un processo alla volta. una volta che ci entra P1, a P2 viene negata l’entrata finche P1 non finisce. a quel punto, P2 viene riesumato e può essere completato
in questo modo non ci sono più comportamenti indesiderati, ma ci vuole un modo per gestire l’atomicità della funzione

## race condition
si ha una race condition quando:
- più processi o thread leggono e scrivono su una stessa risorsa condivisa, e lo fanno in modo tale che lo stato finale della risorsa dipende dall’ordine di esecuzione dei detti processi e thread
	- in particolare, il risultato può dipendere dal processo o thread che finisce per ultimo
**sezione critica** è la parte del codice di un processo che può portare ad una corsa critica

il SO deve quindi assicurare che processi ed output siano indipendenti dalla velocità di computazione (ovvero da che scheduling accade)

# tipi di interazione tra processi

| comunicazione                                                                       | relazione    | influenza                                                                                                                                   | problemi di controllo                                     |
| ----------------------------------------------------------------------------------- | ------------ | ------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------- |
| nessuna (ogni processo pensa di essere solo)                                        | competizione | risultato di un processo è indipendente dagli altri, tempo di esecuzione di un processo è dipendente dagli altri                            | mutua esclusione, deadlock, starvation                    |
| memoria condivisa (i processi sanno che c’è qualche altro processo)                 | cooperazione | risultato di un processo è dipendente dall’informazione data da altri processi, tempo di esecuzione di un processo è dipendente dagli altri | mutua esclusione, deadlock, starvation, coerenza dei dati |
| primitive di comunicazione ( i processi sanno anche i PID di alcuni altri processi) | cooperazione | risultato di un processo è dipendente dall’informazione data da altri, tempo di esecuzione di un processo è dipendente dagli altri          | deadlock, starvation                                      |
primitive di comunicazione: i processi possono mandare dati ad altri processi di cui conoscono il PID
memoria condivisa: un processo scrive nell’area di memoria condivisa, non sa chi la andrà a leggere

## processi in competizione
il problema principale in questo caso è che per l’accesso alle risorse i processi devono fare richiesta al SO (tramite syscall) (es: accesso a tastiera, monitor, disco …)
>[!info] mutua esclusione per processi in competizione
![[Pasted image 20241208131930.png]]
ogni processo chiama una funzione che entra nella sezione critica, fa l’operazione, ed esce

>[!info] mutua esclusione per processi cooperanti
in questo caso, è il programmatore che si occupa di scrivere `entercritical` ed `exitcritical`, in quanto spesso la risorsa condivisa riguarda il processo stesso
![[Pasted image 20241208131930.png]]

>[!example] esempio di starvation
>- A richiede accesso prima alla stampante
>- B fa la stessa cosa
>- il SO da la sta