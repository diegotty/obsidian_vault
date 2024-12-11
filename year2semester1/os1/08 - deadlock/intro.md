---
created: 2024-12-10
related to: "[[intro alla concorrenza]]"
updated: 2024-12-11T07:55
---
**deadlock**: blocco permanente di un insieme di processi, che competono per delle risorse di sistema o comunicano tra loro
- il motivo di base è la richiesta contemporanea delle stesse risorse da parte di due o più processi !
- non esiste una soluzione efficace, ma esistono diversi approcci, da valutare nei vari casi
- anche se il deadlock potrebbe essere causato da un **corner case** (una combinazione rara di eventi),  va comunque prevenuto 
>[!example] esempio di dealock
![[Pasted image 20241210083651.png]]
>in questo esempio (a) esistonon tante combinazioni possibili che risultano in una gestione corretta, ma ne esiste anche uno che finisce in deadlock (b)
>- if you do this you are braindead. and i hate you
## joint progress diagram
utilizzato per gestire deadlock tra pochi processi(2,3 procesi, xke 1 processo per asse)
>[!example] esempio 1
>![[Pasted image 20241210084017.png]]
x-axis: progress of P 
y-axis: progress of Q
le linee marcate in nero sono le possibili progress path di P e Q
se il dispatcher gestisce i processi in modo tale da seguire la linea (3) o (4), è assicurato un deadlock (che avviene nelle zone a righe, non nella zona grigia ! però, per arrivare nelle zone a righe, si ““entra”” solo dalla zona grigia(?))

>[!example] esempio 2
![[Pasted image 20241210090428.png]]
in questo caso invece, non si arriverà mai a un deadlock (infatti le zone a righe esistono, ma non esiste una zona grigia attraverso cui arrivarci)
>- in questo caso, P è molto più “generoso” di prima, in quanto non necessita di possedere 2 risorse contemporaneamente

## risorse riusabili
usabili da un solo processo alla volta,  e il fatto di essere usate non le “consuma”. una volta che un processo ottiene una risorsa riusabile, prima o poi la rilascia, cosicchè altri processi possano usarla a loro volta
- es: processori, I/O channels, memoria primaria/secondaria, dispositivi, file, database, etc
per le risorse riusabili, lo stallo può esistere solo se un processo ha una risorsa e ne richiede un’altra
>[!example] esempio 1 con 2 processi
![[Pasted image 20241210091149.png]]
perform funzion: sezione critica
i lock sono semplificazioni dei semafori (`request(D) + lock(D) == semWait(D)`)

>[!example] esempio 2 con 2 processi
![[Pasted image 20241210091533.png]]
supponiamo di avere 200KB di memoria disponibili:
>- il deadlock avverà quando uno dei due processi farà la seconda richiesta (se anche l’altro processo ha effettuato la prima richiesta)
>- in questo esempio, se P1 venisse eseguito nella sua interezza, la memoria usata e poi rilasciata da P1 può essere **riusata** da P2


## risorse consumabili
vengono create(prodotte) e distrutte(consumate)
- es: interruzioni, segnali, messaggi, informazione dei buffer di I/O
in questo caso il deadlock si verifica se si fa una richiesta (bloccante) di una risorsa non ancora creata
>[!example] esempio con 2 procesi
![[Pasted image 20241210091941.png]]
who made this mf burger dawg
## grafo dell’allocazione delle risorse
grafo diretto che rappresenta lo stato di risorse e processi
- ogni quadrato è un tipo di risorsa, tanti pallini nel quadrato quante istanze di una stessa risorsa
>[!info] rappresentazione grafo allocazione risorse
![[Pasted image 20241210095101.png]]

>[!important] per le risorse consumabili, non esiste mai la freccia “Held by”, bensì i pallini compaiono(quando vengono prodotti) e scompaiono(quando vengono consumati)
## condizioni per il deadlock
condizioni necessarie affichè si verifichi il deadlock
### risorse riusabili
- mutua esclusione: solo un processo alla volta può usare una data risorsa
- **hold and wait**: un processo può richiedere risorse mentre ne tiene già bloccate altre
- niente preemption per le risorse: nono si può sottrarre una risorsa ad un processo senza che quest’ultimo non la rilasci
- attesa circolare: esiste una catena chiusa di processi, in cui ciascun processo detiene una risorsa che è richiesta (invano) dal processo che lo segue nella catena
### risorse consumabili
- mutua esclusione: la risorsa va a chi ne riesce a fare richiesta per primo
- **hold and wait**: diventa: si può richiedere(in modo bloccante), una risorsa quando ancora non sia stata creata
- niente preeemption per le risorse: infatti non appena viene concessa una risorsa, essa viene distrutta
- attesa circolare (same as above)
>[!example] esempio di attesa circolare
![[Pasted image 20241210100016.png]]
>- in (a) si verificano : mutua esclusione, hold and wait, mancanza di preemption, e attesa circolare
>- in (b) viene risolto il deadlock allocando più istanze delle risorsa necessarie

>[!example] esempio delle 4 macchine
![[Pasted image 20241210100332.png]]
rappresentazione grafica del deadlock nel primo esempio visto in questo file

la **possibilità di deadlock** si verifica quando sono presenti:
- mutua esclusione
- hold-and-wait
- niente preemption per le risorse
l’**esitenza di un deadlock** avviene quando, oltre alle condizioni di sopra, si aggiunge l’attesa circolare
# deadlock e SO
il SO può decidere di gestire il deadlock in modo diverso:
## prevenire 
il SO fa sì che una delle 4 condizioni per il deadlock sia sempre falsa (quindi deadlock impossibile,  “per costruzione”)
- mutua esclusione: non c’è un modo per evitare che la mutua esclusione sia un requisito per certe risorse
- hold-and wait: si impone ad un processo di richiedere tutte le risorse di cui potrebbe aver bisogno in una volta (può essere difficile per software complessi, e si tengono risorse bloccate per un tempo troppo lungo)
- niente preemption per le risorse: il SO può richiedere ad un processo di rilasciare le sue risorse(e lo dovrà richiedere in seguito), (ciò si può fare solo per risorse/processi particolari)
- attesa circolare: si definisce un ordinamento crescente delle risorse: una risorsa viene data solo se segue (di ordine) quelle che il processo già detiene (ciò impedisce l’attesa circolare)
## evitare 
il SO fa sì che il dealock possa accadere, ma il sistema si muova in modo che il deadlock non capiti (decidendo di volta in volta cosa fare con l’assegnazione delle risorse)
- in particolare, ammette mutua esclusione, hold-and-wait e niente preemption per le risorse, ma si concentra sull’attesa circolare: evita di farla accadere
occorre decidere se l’attuale richiesta di una risorsa può portare ad un deadlock, se esaudita( ciò richiede la conoscenza delle richieste future (quindi in generale complesso)). ci sono quindi 2 possibilità:
- non mandare in esecuzione un processo se le sue richieste possono portare a deadlock
- non conedere una risorsa ad un processo se allocarla può portare a deadlock 
### algoritmo del banchiere
## rilevare 
il SO lascia che eventualemente ci sia deadlock, ma deve rilevare se ciò accade (ogni tanto, il SO, vede se si è verificato, e notifica all’utente o prende decisioni per rimuoverlo)
- mutua esclusione: 
- hold-and wait:
- niente preemption per le risorse:
- attesa circolare
## ignorare 
il SO lascia che il deadlock accada: se dei processi vanno in deadlock, è colpa dell’utente (questa gestione non è accettabile, in generale, per i processi del SO)
- mutua esclusione: 
- hold-and wait:
- niente preemption per le risorse:
- attesa circolare