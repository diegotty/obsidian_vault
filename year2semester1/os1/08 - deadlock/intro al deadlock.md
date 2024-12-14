---
created: 2024-12-10
related to: "[[intro alla concorrenza]]"
updated: 2024-12-14T06:58
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
- valido per le risorse riusabili(non per le risorse consumabili !)
il sistema ha uno **stato**, che è l’attuale allocazione delle risorse hai processi
- uno stato è **safe**(sicuro) se da esso parte almeno un cammino che non porta ad un deadlock
- uno stato non sicuro è **unsafe** (insicuro)(insecure ??? man up kid)

>[!info] algoritmo del banchiere: strutture dati
>![[Pasted image 20241211080539.png]]
>- $m$ è il numero di tipi di risorse diverse (es: stampante, mouse, tastiera, monitor sono 4 tipi di risorse diverse)
>- in $R_i$ è presente il valore che rappresenta il numero di istanze per l’$i$-esimo tipo di risorsa
>- in $V_i$ è presente il valore che rappresenta il numero di istanze per l’$i$-esimo tipo di risorsa **ancora non allocate**, quindi disponibili
>- la matrice $C$ è composta da $m$ colonne (una per ogni tipo di risorsa), ed $n$ righe, una per ogni processo che sto monitorando, e un valore si legge come: il processo $i$-esimo (riga) prima o poi chiederà x istanze del tipo di di risorsa $j$-esima (colonna)
>- la matrice $A$ ha la stessa struttura della matrice $C$, e contiene le il numero di allocazioni correnti di ogni tipo di risorsa per ogni processo 
>- esiste anche un altro vettore, `request`(che vedremo nel codice), che contiene la prossima (?) richiesta da parte di un processo. è un vettore di $m$ elementi ed ogni valore corrisponde al numero di istanze di un tipo di risorsa richiesto dal processo

>[!example] esempio di stato sicuro
![[Pasted image 20241211081415.png]]
**verifichiamo che questo sia uno stato sicuro**:
supponiamo di eseguire P2 fino al suo completamento:
![[Pasted image 20241211083543.png]]
da questo punto in poi, abbiamo abbastanza risorse (tra cui quelle liberate da P2), per portare a termine tutti i processi rimanenti:
eseguiamo P1 fino al suo completamento:
![[Pasted image 20241211083811.png]]
eseguiamo P3 fino al suo completamento:
![[Pasted image 20241211083845.png]]
e possiamo eseguire anche P4 fino al suo completamento. lo stato è quindi sicuro
abbiamo quindi trovato un cammino che non porta ad un deadlock, quindi lo stato è sicuro

>[!example] esempio di stato non sicuro
partiamo da queso stato (sicuro). 
![[Pasted image 20241211084002.png]]
arriviamo al seguente stato dopo una richiesta da parte di P1 di una unità di R1 ed una unità di R3 (perchè l’algoritmo del banchiere era stato implementato male ??????? o xke non c’era l’algoritmo del banchiere ?????)
![[Pasted image 20241211084211.png]]

### implementazione dell’algoritmo del banchiere
```c
**strutture dati
struct state {
	int resource[m];  // R
	int available[m]; // V
	int claim[n][m];  // C
	int alloc[n][m];  // A
}
```

```c
**implementazione algoritmo del banchiere
if(alloc[i,*]+request[*] > claim[i,*]) 
	<error>;                           // total request > claim
else if(request[*] > available[*])
	<suspend process>;
else {
	<define newstate by:
	alloc[i,*] = alloc[i,*] + request[*];
	available[*] = available[*] - request[*]>;
}
if(safe(newstate)) {
	<carry out allocation>;
} else {
	<restore original state>;
	<suspend process>;
}

boolean safe(state S) {
	int currentavail[m];
	process rest[<number of processes>];
	currentavail = available;
	rest = {all processes};
	possibile = true;
	while(possible) {
		<find a process Pk in rest such that
		claim[k, *]-alloc[k, *] <= currentavail;>
		if(found) {
			currentavail = currentavail+alloc[k,*];
			rest = rest - {Pk}
		} else possible = false;
	}
	return (rest == null)
}
```
#### breakdown
>[!important] l’algoritmo del banchiere è implementato nel SO, e viene quindi eseguito in kernel mode
>è per questo che l’algoritmo può bloccare i processi (quando una richiesta non si può soddisfare !)

- devo verificare se una richiesta **fatta da un processo tra quelli che sto monitorando** è soddisfabile o no
- `if(alloc[i,*] + request[*] > claim[i,*])` viene usato per controllare che i processi non richiedano più risorse di quante avevano dichiarato di volerne chiedere (basta una risorsa che sfora il claim !!)
- se un processo viene sospeso perchè la richiesta è maggiore delle risorse allocate, esso può esser messo in più code d’attesa ! una per ogni risorsa che ha chiesto che al momento della richiesta non era disponibile (nel numero di istanze necessario)
- se invece le risorse sono disponibili, viene creato un nuovo stato con la situazione delle tabelle futura (eventuale), e se lo stato è safe, viene effettuata l’allocazione delle risorse. se lo stato non è safe, il processo viene sospeso
- come viene controllato se uno stato è safe ? con `safe(state S)` (pseudocodice scritto un po con i piedi)
>[!info] osservazioni
>i processi devono essere indipendenti! quindi senza requisiti di sincronizzazione
>- devono quindi essere liberi di andare in esecuzione in un qualsiasi ordine, altrimenti non si può simularne l’esecuzione fino al completamento
>- quindi, l’unica sincronizzazione presente è proprio quella sulle richieste delle risorse !
## rilevare 
il SO lascia che eventualemente ci sia deadlock, ma deve rilevare se ciò accade (ogni tanto, il SO, vede se si è verificato, e notifica all’utente o prende decisioni per rimuoverlo)
si usa la stesse strutture dati dell’algortimo del banchiere, tranne per la **claim** matrix, che viene sostituita da una matrice contenente le richieste effettuate da tutti i processi (in un dato momento) (come `request`, ma per tutti i processi)
### algoritmo
1. marca tutti i processi che non hanno allocato nulla
2. $w \leftarrow V$
3. (inizio ciclo) sia $i$ un processo non marcato t.c. $Q_{ik}\leq w_{k}$, $\forall 1\leq k \leq m$(cioè che il suo vettore di richiesta è soddisfabile). le sue risorse possono essere accordate
 4. se $i$ non esiste, vai al passo 6 (c’è almeno un processo non marcato)
 5. marca $i$ e aggiorna $w \leftarrow w + A_i$ , poi ritorna al passo 3 (facciamo finta il processo venga eseguito a termine e che le sue risorse vengano liberate)
 6. (fine ciclo)
 7. c’è un deadlock se e solo se esiste un processo non marcato
 >[!info] rappresentazione delle strutture dati
 ![[Pasted image 20241214062640.png]]
 in questo caso ho un deadlock !! riesco solo a marcare P3

una volta rilevato il deadlock, si può:
- terminare forzosamente tutti i processi coinvolti nel deadlock (soluzione più comune, e almeno un processo non in deadlock resta sempre)
- mantenere dei punti di riprisitino, ed effettuare il ripristino al punto precedente ( lo stallo può verificarsi nuovamente, ma è improbabile che lo faci all’infinito)
- terminare forzosamente i processi coinvolti nel deadlock **uno ad uno**, finchè lo stallo non c’e più
- sottrarre forzosamente risorse ai processi coinvolti nel deadlock **uno ad uno**, finchè lo stallo non c’e più
## ignorare 
il SO lascia che il deadlock accada: se dei processi vanno in deadlock, è colpa dell’utente (questa gestione non è accettabile, in generale, per i processi del SO)
- banale, il SO non fa niente ( e non c’è niente da dire :))
>[!info] vantaggi e svantaggi
![[Pasted image 20241214064933.png]]
# deadlock in linux
come sempre, Linux implementa una gestione minimale ma il più efficiente possibile:
se dei processi utente sono scritti male e possono andare in deadlock, peggio per loro, che ci vadano !!!! (FUCK EM !!!)
- vorrà dire che saranno tutti bloccati (`TASK_INTERRUPTIBLE`)
- sta all’utente accorgersene e killarli, e visto che sono solo processi utente, non possono fare grande danno
invece, per quanto riguarda il kernel, c’è la **prevenzione dell’attesa circolare** ( con un ordinamento crescente delle risorse, come abbiamo visto sopra)
- i lock vengono sempre acquisiti in un ordine fisso e predeterminato
# filosofi a cena