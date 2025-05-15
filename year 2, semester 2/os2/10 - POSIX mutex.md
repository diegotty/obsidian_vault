---
related to: "[[09 - threads]]"
created: 2025-03-02T17:41
updated: 2025-05-15T09:15
completed: false
---
## flussi di esecuzione concorrente
come abbiamo studiato, la fonte di complicazione maggiore nei SO è costituita dall’esistenza di flussi di esecuzione concorrenti, in quanto:
- la coerenza delle strutture dati private di ciascun flusso di esecuzione **è garantita** dal meccanismo di cambio del flusso
- la coerenza delle strutture dati condivise tra i vari flussi di esecuzione **non è garantita** da tale meccanismo !
### race condition
abbiamo già visto che se due o più flussi di esecuzione hanno una struttura dati in comune, la concorrenza dei flussi può determinare uno stato della struttura **non coerente** con la logica di ciascuno dei flussi ! 
- possono infatti accadere **race condition**, situazioni in cui lo stato della memoria condivisa tra due o + flussi di esecuzione concorrenti **dipende dall’ordine esatto degli accessi in memoria** (grave !)
>[!tip] operazioni atomiche
come abbiamo visto in precedenza, le uniche operazioni atomiche sono quelle effettivamente atomiche in linguaggio assembly (es: gli assegnamenti in C non sono operazioni atomiche)

>[!example] esempio di race condition
![[Pasted image 20250515090247.png]]

### sezione critica
una **sezione critica** è una sequenza di istruzioni di un flusso di esecuzione che accede ad una struttura di dati condivisa, e che quindi non deve essere eseguita in modo concorrente ad un’altra regione critica
- per rendere utilizzabile una sezione critica, ovvero per avere l’accesso esclusivo ad una risorsa condivisa, si può usare il **semaforo**, inventato dar Dijkstra 
#### semafori
i **semafori** si basano su una variabile intera che agisce da contatore, e grazie a due primitive atomiche `wait()` e `signal()` garantiscono la **mutua esclusione** ad una risorsa condivisa
- citiamo i **semafori contatore**, in cui una risorsa può essere acquisita da $n>1$ flussi di esecuzione concorrentemente, e i **semafori binari**, in cui $n=1$
## POSIX mutex
il **POSIX mutex** (**MUTual EXclusion device**) viene utilizzato per proteggere strutture dati condivise e realizzare sezioni critiche. li studieremo nell’ambito dei thread !
### $\verb |int pthread_mutex_init(pthread_mutex_t *mutex, const pthread_mutexattr_t *mutexattr|$
la funzione `pthread_mutex_init()` inizializza un mutex e imposta gli attributi a `mutex_attr`
- gli attributi determinano il comportamento del semaforo quando il thread invoca un `lock/unlock` più volte consecutivamente
	- `fast`:
	- `recursive`:
	- `error checking`: 